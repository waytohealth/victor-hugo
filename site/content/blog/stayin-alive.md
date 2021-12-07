---
title: Stayin' Alive
date: 2021-12-07
summary: My most recent fun ("fun") technical challenge: at the intersection of application behavior, network behavior, OS behavior, and far more TCP details than I normally deal with as an application developer.
image: /images/uploads/cleanshot-2021-12-07-at-11.16.03-2x.png
authorname: Michael Y. Kopinsky
authorimage: /images/uploads/kopinsky.jpg
label: technical
---
<blockquote>
<p>Ah, ha, ha, ha, stayin' alive, stayin' alive.</p>
<p>Ah, ha, ha, ha, stayin' alive.</p>
<p> - The Bee Gees</p>
</blockquote>
<p></p>

I recently dealt with a thorny technical issue that forced me to think far more about firewalls and TCP than I usually do. I heard that other teams in the health system are dealing with a similar problem, and thought this write-up would be worth sharing.

The issue we're seeing is that the application (a queue worker on a low-volume application which only gets work at sporadic intervals) is keeping a connection open to the database open for hours with no activity. After an hour the firewall is dropping the connection, but in a way that neither the client (the PHP queue worker) nor the server (MySQL) knows about it. On the server, `show processlist` shows the connection as still connected and in a `Sleep` status, on the client side `netstat -an` shows the connection as alive and in an `ESTABLISHED` state, but when the client sends anything down the pipe, the packets are just being dropped by the firewall. This is explained well in [this post on Network Engineering StackExchange](https://networkengineering.stackexchange.com/questions/46025/how-would-a-firewall-kill-a-tcp-connection-without-rst-or-fin).

We've seen similar manifestations of this in the past, where a long running task would start by writing to the database (`INSERT INTO task_log(...)`), shell out to do a bunch of work, and then write to the database (`UPDATE task_log SET end_time=? WHERE id=?`). The final write to the database, which was sometimes hours later, would error with `MySQL server has gone away`. We "fixed" that by slicing the work into smaller chunks that would finish before the firewall disconnected the connection.

There are a few possible paths to fix this - in the firewall, in the client  OS, and in the application.

1. On the firewall: `session-ttl` controls how long to allow sessions to idle, defaulting to 3600 seconds. We could increase it (and attempted to do so to 7200 at one point, though we accidentally edited the wrong policy) but that just defers the problem. There shouldn't be an arbitrary cutoff of inactivity after which a database connection freezes for 15 minutes, and making that timeout be 4 hours (or even 24 hours) instead of 2 hours delays it but doesn't solve it.
2. On the firewall: `set timeout-send-rst enable` tells the firewall to send an RST (reset) packet to both the client and server when the session times out. If the client and the server are notified that the connection timed out, they should hopefully be smart enough to reconnect rather than trying to use the black-holed connection.
3. On the client OS, tell the networking stack to send a TCP keepalive - at a frequency (`tcp_keepalive_time`) shorter than the firewall's `session-ttl`. By default Linux sends a keepalive every 120 minutes, but this is configurable. By sending a TCP keepalive from within the networking stack, this keeps the connection active and prevents the database from disconnecting it. https://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
4. One additional option would be to set something on the application level, to force the application to send a query like `SELECT 1` every 15 minutes. https://github.com/doctrine/dbal/pull/414 is an example of a library adding that functionality. This is contingent on database library support, and the application being able to send that 15-minute ping when otherwise idle.


# What we did:
*Monday:* we temporarily implemented fix 3, setting the TCP keepalive interval in the client OS to 10 minutes instead of 2 hours. I observed through netstat timers (see below) that it was resetting the keepalive timer every 10 minutes, and we saw in the firewall session table that the session was indeed staying alive:

```
session info: proto=6 proto_state=01 duration=4238 expire=3563 timeout=3600 flags=00000000 sockflag=00000000
```

This tells us that although the connection has been ongoing for more than an hour (duration=4238), it only has 37 seconds (3600 - 3563) of inactivity. We also observed that with that fix in place, the application did behave as expected, and did not time out when sending a query to MySQL.

We also implemented fix 2, telling the firewall to terminate connections loudly (with an RST packet) rather than dropping it gracefully. However, the client-side TCP keepalive fix meant that this point was never reached so we didn't know yet if it was working. We removed fix 3 to let things run with only fix 2 in place, so we can see how the client and server handle the disconnection with RST.

*Tuesday:* With only fix 3 (`set timeout-send-rst enable`) in place, we let things run for much of a day. I didn't watch as closely this time, but... results.... nope. Didn't work. After the 1hr timeout, the client application and MySQL server both reported the connection as alive, but the session table on the firewall didn't show it, and as expected, the application froze when it came time to do work. (Don't we all, sometimes?) I don't know why the `set timeout-send-rst enable` setting didn't work.

Rather than going back to fix 2 which would require updating the `tcp_keepalive_time` setting on every server in our environment, we realized that increasing the Firewall's session TTL to > 7200 (e.g. 7500 seconds) would allow the Linux default `tcp_keepalive_time` to refresh the TCP connection sooner than the Firewall would disconnect

So in the end, the fix we seem to be going with is Fix 1, without the downside I thought it would carry. It's been working great for half a day, and assuming it continues to work, I expect that the increased session TTL will move from a local override in our firewall policy to a global firewall setting.

# Diagnostic tools

There were a few tools I learned about (or learned more intimately) during this process.

## strace

The main way to see the freezing behavior in action is using strace. [This blog post](https://shubhamjain.co/2015/09/10/debugging-stuck-php-fpm-process-with-strace/) was super helpful in interpreting the output from strace.

Here's a trimmed down and annotated strace from when I watched this timeout behavior in action:

```
# Getting job from queue (beanstalkd)
Wed Dec  1 12:30:15 2021 sendto(10, "reserve-with-timeout 0\r\n", 24, MSG_DONTWAIT, NULL, 0) = 24
Wed Dec  1 12:30:15 2021 recvfrom(10, "RESERVED 75649007 136\r\n{\"display"..., 8192, MSG_DONTWAIT, NULL, NULL) = 161

# Logging to disk that it's starting the job
Wed Dec  1 12:30:15 2021 sendto(8, "[2021-12-01 12:30:15][75649007] "..., 58, MSG_DONTWAIT, NULL, 0) = 58

# Getting job details from the queue (beanstalkd)
Wed Dec  1 12:30:15 2021 sendto(10, "stats-job 75649007\r\n", 20, MSG_DONTWAIT, NULL, 0) = 20
Wed Dec  1 12:30:15 2021 recvfrom(10, "OK 165\r\n---\nid: 75649007\ntube: l"..., 8192, MSG_DONTWAIT, NULL, NULL) = 175

# Sending a query to MySQL
Wed Dec  1 12:30:15 2021 sendto(11, "\v\2\0\0\3SELECT q.id AS q__id, q.for"..., 527, MSG_DONTWAIT, NULL, 0) = 527

# Exactly a minute later: SIGALRM triggered from the Laravel queue worker runner. This is the job trying to timing out, but failing because it's still stuck with a pending network request
Wed Dec  1 12:31:15 2021 --- SIGALRM {si_signo=SIGALRM, si_code=SI_KERNEL} ---
Wed Dec  1 12:31:15 2021 rt_sigreturn({mask=[]})                 = -1 EINTR (Interrupted system call)

# 15 minutes later: Get an ETIMEDOUT from the database file descriptor
Wed Dec  1 12:45:43 2021 recvfrom(11, 0x7fab7cd8e000, 32768, MSG_DONTWAIT, NULL, NULL) = -1 ETIMEDOUT (Connection timed out)

# Close the job
Wed Dec  1 12:45:43 2021 close(12)                               = 0
Wed Dec  1 12:45:43 2021 kill(9235, SIGKILL)                     = ?
Wed Dec  1 12:45:43 2021 +++ killed by SIGKILL +++
```

When the connection isn't timing out, the MySQL query gets an immediate response from the server.

```
# Sending a query to MySQL
Dec 06 16:01:14 sendto(11, "\v\2\0\0\3SELECT q.id AS q__id, q.for"..., 527, MSG_DONTWAIT, NULL, 0) = 527

# Getting an immediate response
Dec 06 16:01:14 recvfrom(11, "\1\0\0\1\0172\0\0\2\3def\vw2hprod_lgh\1q\tqueu"..., 32768, MSG_DONTWAIT, NULL, NULL) = 1189
```

As a side note: the timestamps shown above are not output by strace; I added them on by piping strace output to `ts`, which is installed as part of `moreutils`. strace prints it output to stderr, so to pipe to ts the full command is `strace -e trace=network -p $PID 2>&1 | ts`.

## netstat timers

The keepalive timer can be seen by adding the `--timers` or `-o` flag to netstat. This is what the connections look like during an idle period, when the connection is still established and the keepalive counter is happily counting down from 7200 down to 0.

```
Every 1.0s: netstat -tapno |egrep '(8376|8506|8540|8694)'|grep 3306                              Mon Dec  6 17:06:11 2021

tcp        0    0 172.16.100.193:46030    172.16.32.8:3306        ESTABLISHED 8506/php             keepalive (4584.28/0/0)
tcp        0    0 172.16.100.193:46052    172.16.32.8:3306        ESTABLISHED 8540/php             keepalive (4584.28/0/0)
tcp        0    0 172.16.100.193:47020    172.16.32.8:3306        ESTABLISHED 8376/php             keepalive (4584.27/0/0)
tcp        0    0 172.16.100.193:46202    172.16.32.8:3306        ESTABLISHED 8694/php             keepalive (4584.26/0/0)
```

The `ss -o` command is similar, and I used it at times. Here's what it looks like when the PHP process is shouting in the void. The last column has 3 components:

* `on`: indicates that there is a TCP retransmission timer active
* `14sec`, `9.814ms`, etc: the amount of time remaining on that timer
* `8`, `9`, `10`: The retransmission count

I ran the command at sporadic intervals, and below you can see it with 14 seconds and then 9.8 seconds left on retransmission 8, then 1min37sec on retransmission 9, and then 1min27sec, 1min23sec, and 1min21sec on retransmission 10. If you keep watching long enough I think it gets to retransmission 15 and then gives up.

```
[w2h@w2h-prd1-tasks2 ~]$ ss -o|grep 42724
tcp    ESTAB      0      527    172.16.100.193:42724                172.16.32.8:mysql                 timer:(on,14sec,8)
[w2h@w2h-prd1-tasks2 ~]$ ss -o|grep 42724
tcp    ESTAB      0      527    172.16.100.193:42724                172.16.32.8:mysql                 timer:(on,9.814ms,8)
[w2h@w2h-prd1-tasks2 ~]$ ss -o|grep 42724
tcp    ESTAB      0      527    172.16.100.193:42724                172.16.32.8:mysql                 timer:(on,1min37sec,9)
[w2h@w2h-prd1-tasks2 ~]$ ss -o|grep 42724
tcp    ESTAB      0      527    172.16.100.193:42724                172.16.32.8:mysql                 timer:(on,1min27sec,10)
[w2h@w2h-prd1-tasks2 ~]$ ss -o|grep 42724
tcp    ESTAB      0      527    172.16.100.193:42724                172.16.32.8:mysql                 timer:(on,1min23sec,10)
[w2h@w2h-prd1-tasks2 ~]$ ss -o|grep 42724
tcp    ESTAB      0      527    172.16.100.193:42724                172.16.32.8:mysql                 timer:(on,1min21sec,10)
```

More details on how to interpret these netstat timers can be found at https://superuser.com/a/904747/57372.

# Conclusion

This has been a fun journey to figure out. Thanks to Kyle McGrogan, Albi Kohen, and Dave Weise for all the help!


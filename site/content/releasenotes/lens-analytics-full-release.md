---
title: Lens Analytics full release
date: 2022-04-11
summary: >-
  Spring has sprung and we are pleased to announce the full release of our new
  business intelligence tool, **Lens Analytics**. Lens Analytics is Way to
  Health's product offering that provides deep visibility into program data. 


  Under the hood, we also restructured our **user account framework** to allow for maximum flexibility for modern programs.
---
## Product updates from March 2022

Lens Analytics product offering consists of three main components:

1. [Lens Analytics portal](https://waytohealth.atlassian.net/wiki/spaces/WTHST/pages/2236645393/Lens+Analytics+Portal): Way to Health's analytics tool where users can ask questions of their data and have them visualized in meaningful ways. 
2. Data stores: Our custom analytics databases replicating production data.
3. ETLs: ('Extract, Transform, Load') These are [standard and custom data sets](https://waytohealth.atlassian.net/wiki/spaces/WTHST/pages/2237628417/Standard+Datasets) which can be displayed and queried on via the Lens Analytics portal.

### Lens Analytics custom reports and visualizations

You can now build custom reports and data visualizations in the Lens Analytics portal. See [Creating and publishing questions](https://waytohealth.atlassian.net/wiki/spaces/WTHST/pages/2237333505/Creating+and+publishing+questions) for instructions.

{screen shot of query builder}

### Customize the W2H homepage

The home page can be set up with standard or custom widgets powered by Lens. See [Customize the W2H homepage](https://waytohealth.atlassian.net/wiki/spaces/WTHST/pages/2236874773/Customize+the+W2H+homepage) for instructions.

{screen shot of standard home page}

### Embed Lens dashboards in the W2H portal

You can embed custom data visualizations and reports on in the Analytics section of W2H. See [Setting up embedded analytics](https://waytohealth.atlassian.net/wiki/spaces/WTHST/pages/2236907551/Setting+up+embedded+analytics+in+W2H) for instructions.

{screenshot}

## User account refactor

We've decoupled staff and participant accounts to meet the needs of modern programs. What is the impact?

- Now only staff accounts require unique email address across the platform.
- Participant accounts are only required to be unique within a program and not platform wide, thus allowing participants to enroll in multiple studies or programs simultaneously.

As a result, participant accounts can have less requirements


The team performed a lot of work under the hood to streamline the way accounts work for both staff and participants to meet the needs of modern programs. Previously accounts for both staff and participants required a unique email address or username across the entire platform. This caused problems if someone had been enrolled in a program in the past and needs to use the same email address to enroll in a new program. Now only staff accounts require a unique email address across the platform. Participant accounts are only required to be unique within the program they are enrolled in, and if the program doesn’t leverage our participant portal to allow participants to log back in during or after enrollment, they don’t need to have an email address or username at all! Additionally more profile fields, such as external id and MRN, can be forced to be unique to prevent duplicate enrollment issues. Lastly, since participant accounts are now per-program rather than platform wide, we’ve added tools on the participant portal to help people find the right place to log in with ease in case they end up with the wrong link.
---
title: Optimizing W2H Epic Embed
date: 2021-08-23
summary: >-
  After multiple conversations with our clinical users, our team focused our efforts to redesign the Way to Health (W2H) Epic Embed. The goal was to streamline the clinician experience. The new design makes viewing a patient enrolled in a W2H program easier and optimizes the experience for the clinician. Here are some changes you can expect to see.
---

## Product updates from August 2021

The Way to Health embed within Epic has made life a lot easier for clinicians. This work is an attempt based on their feedback to make it even more streamlined and simple for one-time or ongoing use. 


## Intuitive Layout

### Main programs page will highlight relevant patient programs

Clicking on the W2H embed will now bring up a list of programs that the patient is either currently enrolled in OR has been previously enrolled in.

You can scroll down to find a list of additional programs that you could enroll the patient into.

<img src="/images/releasenotes/index.jpg" width="50%" />
<br/>

### View patient’s details in a single program at a time

All program specific information is now summarized in a single tab - no more clicking around across multiple tabs to get to the information you need.

<img src="/images/releasenotes/index2.jpg" width="50%" />

<img src="/images/releasenotes/index3.jpg" width="50%" />
<br/>

## Organize program sections based on relevance

Within a single program page, you’ll be able to see sections for different components of the program. We’ve created smart ordering so these sections change order depending on relevance.

For example, if your program is using an adherence snapshot to view patient data on a high-level, you’ll see that at the top of the page for quick access.

## Easier Access

### Unenroll or re-enroll a patient directly from the main programs page. 

On the main programs page, you’ll still be able to un-enroll and re-enroll patients from a program. This will be available at the top of the page so you won’t have to scroll anymore.

<img src="/images/releasenotes/index4.jpg" width="50%" />
<br/>


### Favorite programs you want to jump directly for any patient. 

If there’s a program you are managing and always want to jump to that program, hover over the row and a star icon will show up. Clicking on that icon makes that program a “Favorite” and will be bumped to the top of the list. Additionally, whenever you open the W2H Embed, you’ll jump directly to that program’s tab for every patient.

<img src="/images/releasenotes/index5.jpg" width="50%" />
<br/>

## Improved Enrollment

### Allow patient enrollment to use voice-over-internet numbers. 

During enrollment, W2H confirms in the backend the cell phone entered is a valid mobile number to ensure the patient will receive texts. We found that not all patients have a mobile number or cell phone plan and use texting apps to receive texts. To improve access, we adjusted the backend algorithm so patients with texting apps can enroll with the app number.

### Order enrollment fields based on priority. 

Now you can order the enrollment fields for a program by priority or based on what makes sense for the program.The default option was to show fields in alphabetical order which often doesn’t make sense.

For example 

1. Enroll in ERAS/P 

2. Location 

3. Enroll in Ostomy Output 

4. Surgery Date. 

This layout isn’t intuitive. Now we can make adjustments so that the order is: 

1. Location 

2. Surgery Date 

3. Enroll in ERAS/P 

4. Enroll Ostomy Output. 

If you would like to change the order of enrollment fields for your program, please contact your W2H implementation lead with the ideal order. 

<img src="/images/releasenotes/index6.jpg" width="50%" />
<br/>

## Other Changes

### Setting in W2H to automatically send patient data to flowsheets. 

Instead of having to check through patient data and push a button to send it to Epic flowsheets, we can now programmatically send the data to Penn Chart. If this setting is something you’d like to implement for your program, please contact your W2H implementation lead.

### Improved landing page for easier access. 

If you haven’t used the W2H Embed or are a first-time user,  you can now get access by simply clicking “Get Access”. 




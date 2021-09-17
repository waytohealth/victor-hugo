---
title: Summer Wrap Up
date: 2021-09-16
summary: >-
  It was a busy summer! We wrapped up a successful upgrade of our embedded view within Epic and made great strides in handling remote monitoring at scale. On the operations front, we successfully launched two studies at Penn Medicine focused on improving patient experience and outcomes among cancer patients. We also launched versions of Heart Safe Motherhood at other local Philadelphia hospitals.
---

## Product updates from August 2021

We continued to focus on performance and monitoring, while also tackling enhancements to our text messaging toolbox and integrations with the electronic health record.

Below is a summary of enhancements and bug fixes completed in August 2021.


## Enhancements

### Text Messaging
- Ability to trigger messaging and actions off of an inbound photo/media message
- Ability to configure project to respond or not respond to Apple reactions
- Ability to update custom participant timing through a survey

### Integrations
- “visit_type” field added to Epic Appointments Connector

### Performance & Monitoring
- Quit “Be Active” Fitbit reports to increase reporting speeds across platform
- Add request logging to surveyjs-service
- Quit monthly Twilio report now that ETL service is in place
- Remove old job queueing system

### Misc
- Ability to set default time zone across a project
- Add median as an option for a participant data variable definition
- Ability to use a subquestion from a survey in Event Block criteria
- Upgrade survey editor to 1.8.63 (surveyjs)
- Make W2H compatible with MariaDB 10.6
- Project-Specific: Add more fields to Healthy Lungs Lens query

### Bug Fixes
- Event title translations missing from participant’s view of surveys
- Script error in Epic embedded view of surveys
- Remove persistent page number on Variables page across projects
- Javascript error occurring in W2H links in Epic embed
- Make variable names unique within each project
- Inbox images not displaying in Epic embed
- Participant data variable missing field after copy study
- Manage Data view sorting incorrectly
- Delete user causes an error in the autocomplete widget
- Re-authorizing OAuth device breaks current authorization
- Calculated values on partially completed surveys do not update
- Whitespace messes up numeric value trigger
- XSS vulnerability in survey and event pages
- Calendar displaying a conversation event for longer than what it’s configured to be
- Viewing a participants event details screen with another participant ID should give 401 error
- Filters on includes (specifically participant incident) are always treated as `in` even if the filter was `not`.
---
title: "The foundation of analytics: ETLs"
date: 2021-12-20
summary: >-
  With the end of the quarter approaching, we are getting closer towards our
  next major release: Simplifing and speeding up integration between Way to
  Health and our analytics tool, Lens. See product updates below for more
  details.


  On the operations front, we launched nine implementations last month. Of those, several projects focused on understanding and improving behaviors around substance abuse disorders. Projects include a trial focused on increasing Narcan carrying among patients with opioid use disorder (OUD), a pilot developed to better understand needs of patients with OUD leaving the hospital built on the COVID Watch model, and remote engagement strategies targeted at improving nursing engagement for patients with substance abuse disorders.
---
## Product updates from November 2021

In early 2022, we'll be releasing a custom report builder and the availability of standard dashboards. During November, we laid the foundation to make this possible: the creation of standard Extract, Transform, and Load process (ETL), used to get data out of the core W2H database and transform it into a usable format for building reports and dashboards. The improved and integrated Lens will mean more information at your finger tips and less clicks to get there.

Below is a summary of enhancements and bug fixes completed in November 2021.

## Enhancements

### Work Towards Major Release: Enhanced Lens Analytics
- Ability to turn on analytics data sets for reporting and dashboard creation
- Use of database replica for analytics
- Advanced setting built to transition away from static reports

### Project Management Tools

- New filter on Incidents page for Attached Events
- Enforce cell phone uniqueness across participants in the same study

### Monitoring

- Ability to send some grafana alerts to #servers channel
- Development of All Systems Dashboard

### Project Specific

- Custom dashboard updates for Penn Partners in Care
- Custom dashboard updates for Heart Safe Motherhood Penn Medicine
- Custom report for STEP Together
- Custom report for Healthy Lungs
- Participant upload failure insights report for Nursing Care for Substance Use Disorder
- Add calculated values to TrueMotion trip data
- Add providerIDs to FDA Opioid enrollment flow

### Bug Fixes

- Pharmacy SDE messages fail after first try
- Clarity delivery feed is not running for THEA 2.0
- Some automated text messages appears manual in inbox view
- Epic Inbasket message incidents should not resolve on send
- Epic Inbasket messages that failed are not retried
- Adherence snapshot displaying outside the width of the Epic embed view
- Accessing API with invalid JWD returned an empty response (Omni integration)
- Allow lottery number selections by study staff when completing lottery number enrollment steps from admin portal
- Create a software incident if lottery event runs for participant with no number
- Silenced participants should not have conversations started
- Don't send ClinCard number for participants who have already been registered
- Alerts page generates 500 error when the study has no sources
- Dropdown have events from other event blocks but cannot be used to evaluate
- Use sourceID instead of formID when checking for submissions to schedule events by participant preference
- Migration to un-delete iFast events

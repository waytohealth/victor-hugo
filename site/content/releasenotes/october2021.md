---
title: Focus on analytics
date: 2021-11-08
summary: >-
  The last quarter of 2021 is focused on surfacing data more easily and readily to our users. October laid the groundwork for the anticipated release of standard dashboards and a report builder through our analytics tool, Lens. Projects launched this month include THEA 2.0 which layers education content on top of existing prenatal blood pressure monitoring program, Breast Cancer Screening Outreach, and Step 4Life, an RCT leveraging gamification concepts to improve physical activity among patients with Alzheimers.
---

## Product updates from October 2021

Our October release includes a myriad of useful tools for both project managers and our implementation leads. A noteworthy enhancement is the ability to schedule events to repeat on certain days of the week. This simplifies building new interventions and makes updates to existing ones a breeze.

Below is a summary of enhancements and bug fixes completed in October 2021.

## Enhancements

### Project Management Tools

- Change participant upload feature so variables are not case-sensitive
- Allow project manager role to access Personnel page without access group restriction
- Add preferred language field to ‘create participant’ screen
- Add hyperlink to participant profile from transactions page

### Building Tools

- Ability to schedule day of week for multiple days

### Integrations

- Pull standard demographics from Epic for clinical programs to build Lens dashboards
- Ability to refresh tabs in the Epic embed
- Show more explicit error when Epic demographics sync is turned on and no valid number is found

### Optimization

- Remove demo.sync-csv task
- Remove release environment
- Implement feature branch deploy previews
- Remove per-week event pages
- Consolidate pm2 config across environments
- Move the core site’s deploy script into waytohealth/waytohealth repo

### Project Specific

- Enroll Spanish speakers from Results Reporting into Covid Watch Spanish
- Run photo analysis report for department of dermatology
- Run migration to change COVID Safe participant email addresses

### Bug Fixes

- Error with Omron connector
- Notification trigger for enrollment step reminder not user-friendly
- Fix out of memory issue with updating 22000 date variables
- Access issue with SMS testing tool
- Gestational age variable causes error when displayed in Epic embed
- Internationalization feature should require that language is selected and saved
- Migration to populate missing PennMedConnect provider names
- Heart Safe Motherhood ACOG Adherence values dropped inaccurately
- Log cleanup job not working for TaskLog


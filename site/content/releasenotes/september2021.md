---
title: Fresh Start Effect
date: 2021-10-29
summary: >-
  Our team is no stranger to the power of behavioral economics. Leveraging that “fresh start” effect with the new school year, September is a time for planning and preparation. Our operations team is back to full steam with a new full time analyst and our product owner back from parental leave. Projects launched this month include PennMedConnect at HUP, SAM Oncology at Lancaster General Hospital, Heart Safe Motherhood at Jefferson, and Intermittent Fasting RCT.
---

## Product updates from September 2021

We continue to optimize the Way to Health platform for our users. This includes better defaults and views for researchers, as well as more building tools for our implementation leads. We also placed particular focus on automating enrollments for clinical programs at Penn Medicine through integrations with the electronic health record.

Below is a summary of enhancements and bug fixes completed in September 2021.

## Enhancements

### Project Management Tools
- Default participant views to show arm names
- Expand event detail view for all event types to show logic details

### Building Tools
- Add logic action to schedule an event block
- Add username to the list of variables available in messaging

### Integrations
- New LocationCategory field added to PennChart ADT feed

### Performance & Monitoring
- Improve the formatting of Grafana alerts sent to Slack

### Misc
- Adjust link shortener used for surveys to use https instead of http
- Add advanced setting to toggle on standard ETL tables for Lens reports and dashboards

### Project Specific

- Automated enrollment flow for PennMedConnect (SPP2)
- Custom Epic InBasket notification process for PennMedConnect
- SIC query and automated unenrollment for PennMedConnect
- Enrollment dashboard for PennMedConnect
- Custom reports (Consort and Baseline) for Healthy Lungs Program
- Automate pre- and post-surgery module scheduling for POOP (Penn Ostomy Output Program)
- Automate enrollment for THEA 2.0 prenatal blood pressure and education program

### Bug Fixes

- Migration to update missing BP severity for SupportBP
- Remove alerts for deprecated monitoring system, health.json
- Remove a few of the slowest sources from reports
- Reduce time of our update-apis task
- Stop surveys and conversations that use calculated values from creating empty rows in Manage Data
- Enrollment surveys not showing correct action buttons
- API docs do not indicate what operations are available for updateIncident(s)
- Participant being assigned to phantom arm when stratification survey is completed
- Error when re-enrolling a patient who never started and has a date variable in enrollment form
- Unable to load U-REEACT participant’s inbox
- Survey tool is trimming whitespace from question names
- Deid task is broken
- Optimize Fitbit queue
- Make HidrateSpark not error to sentry if there's no data


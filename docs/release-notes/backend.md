# Release notes for Backend plugin

## > 0.2.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.2.0)

## Summary

This new release introduces local APIs that will support the transition from using the Backstage proxy to call PagerDuty APIs and instead use the local APIs provide by the backend plugin. This will allowed the backend to control which data is exposed to Backstage, replacing the current mechanism that exposes all data returned by PagerDuty APIs.

This release only exposes an API route to get all escalation policies which will work together with the frontend plugin and the scaffolder custom action to replace the existing text field expecting an Escalation Policy Id with a Drop-down control that list all escalation policies available to the user, improving their user experience.

![create-service-dropdown](../images/create-pagerduty-service.png)

### Changes

- improv(ux): Adding support for escalation policy dropdown on custom action

## > 0.1.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.1.2)

## Summary

This release introduces a scaffolder custom action that allows users to create a PagerDuty service from a Software Template and configure the frontend plugin in one single step. Go [here](https://pagerduty.github.io/backstage-plugin-docs/advanced/create-service-software-template/) to see how to do it.

### Changes

- feat(scaffolder): custom action to create service in PagerDuty
- docs(release): :memo: simplified readme

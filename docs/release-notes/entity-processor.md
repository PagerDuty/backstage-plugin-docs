# Release notes for Entity Processor module

## > 0.3.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-entity-processor/releases/tag/0.3.1)

### Summary

Release **0.3.1** removes a limitation that prevented users from enabling PagerDuty as their main source for syncing service dependencies.

With the help of the Backstage team, we were able to implement a few changes that overcome this limitation and users are now able to set PagerDuty as their main source for service dependencies and get their dependencies sync to Backstage for all mapped entities.

### Changes

- feat: enable pagerduty service dependency sync

### Dependencies

- `@pagerduty/backstage-plugin-common: 0.2.1`

## > 0.3.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-entity-processor/releases/tag/0.3.0)

### Summary

This release introduces support for service dependency syncing and automatically creating Backstage integrations in PagerDuty for all new mappings.

With these changes, we are reducing the manual effort on configuring integration keys and service dependencies into Backstage configuration files and ensuring they stay in-sync with PagerDuty.

‼️ **Important**: Due to a Backstage design decision it is not possible to fully overwrite the relations specified in each entity's configuration file. For that reason the option to synchronise strictly from PagerDuty side is not available.

### Changes

- refactor: removing PagerDuty sync to limitation in Backstage
- chore: updating backstage plugin config
- feat: service dependency sync

### Dependencies

- `@pagerduty/backstage-plugin-common: 0.2.1`

## > 0.2.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-entity-processor/releases/tag/0.2.1)

### Summary

This release fixes an issue with the missing plugin config schema. Without it the Backstage instance will not load.

### Changes

- fix: add missing config schema

### Dependencies

- `@pagerduty/backstage-plugin-common: 0.2.0`

## > 0.2.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-entity-processor/releases/tag/0.2.0)

### Summary

This PR adds multi-account support on entity processor. Now if the `account` property exists in the database for the mapped entity it will get added as an annotation to the Backstage entity.

### Changes

- feat: add support for multi account

### Dependencies

- `@pagerduty/backstage-plugin-common: 0.2.0`

## > 0.1.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-entity-processor/releases/tag/0.1.0)

### Summary

This release introduces entity processor capabilities to PagerDuty plugin for Backstage which will allow users to automatically update the entity configuration files according to the setting defined in the new PagerDuty page.

This features makes it easier to map existing PagerDuty services into Backstage entities.

### Changes

- release: 0.1.0

### Dependencies

- `@pagerduty/backstage-plugin-common: 0.1.5`

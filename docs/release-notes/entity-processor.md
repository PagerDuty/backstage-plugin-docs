# Release notes for Entity Processor module

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

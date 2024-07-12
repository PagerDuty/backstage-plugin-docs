# Release notes for Common library

## > 0.2.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.2.0)

### Summary

This release adds type support for multi-account configuration for all aspects of the PagerDuty plugin for Backstage:

- UI Components:
  - `PagerDutyCard`
  - `PagerDutySmallCard`
- Entity Mapping Page
- Entity Processor
- Scaffolder Action

With these changes users with multiple PagerDuty accounts can now configure their services in the same Backstage instance.

### Changes

- feat: add account field to PagerDuty types
- chore(deps): Bump @azure/identity from 4.0.0 to 4.3.0
- chore(deps): Bump braces from 3.0.2 to 3.0.3

## > 0.1.5

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.1.5)

### Summary

This release adds support for entity mapping types to be used by processor, backend and frontend components for easy service mapping between Backstage and PagerDuty services.

### Changes

- feat: adding support for entity mapping types
- chore(deps): Bump ws from 8.16.0 to 8.17.1

## > 0.1.4

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.1.4)

### Summary

This release introduces a new type to support pagination when listing all services in PagerDuty.

### Changes

- feat: add support for service listing
- chore(deps): Bump tar from 6.2.0 to 6.2.1

## > 0.1.3

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.1.3)

### Summary

This release introduces a couple of security patches related to dependencies.

- express
- webpack-dev-middleware

### Changes

- build(deps): Bump webpack-dev-middleware from 5.3.3 to 5.3.4
- build(deps): Bump express from 4.18.2 to 4.19.2

## > 0.1.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.1.2)

### Summary

This release introduces new types to support the new PagerDutyCard UI that requires Service Standards, Service Status and Service Metrics.

### Changes

- chore(deps): Bump follow-redirects from 1.15.5 to 1.15.6
- feat: add types to support updated UI

## > 0.1.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.1.1)

### Summary

This release introduces a security patch to a third-party dependency.

### Changes

- build(deps): Bump ip from 2.0.0 to 2.0.1

## > 0.1.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.1.0)

### Summary

Release **0.1.0** adds the type necessary for OAuth support in Backstage plugin configuration. With this new type users will be able to configure the following OAuth parameters in Backstage `app-config.yaml` file.

```yaml
pagerDuty:
  oauth:
    clientId: ${PD_CLIENT_ID}
    clientSecret: ${PD_CLIENT_SECRET}
    subDomain: ${PD_ACCOUNT_SUBDOMAIN}
    region: ${PD_ACCOUNT_REGION}           // Optional. allowed values: 'us', 'eu'. Defaults to 'us'.
```

### Changes

- feat: adds type to allow OAuth support

## > 0.0.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.0.2)

### Summary

Version 0.0.2 introduces a fix for a configuration issue that was forcing the backend component to load the common library as an ES6 module which is not yet fully supported in Backstage backend components.

### Changes

- fix: update package publishing configuration

## > 0.0.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-common/releases/tag/0.0.1)

### Summary

This release introduces a common package that contains `types` and `constants` to be used by Backstage backend and frontend plugins. This allows us to maintain consistency between types across plugins.

#### Constants

- `PAGERDUTY_INTEGRATION_KEY` maps to 'pagerduty.com/integration-key' annotation
- `PAGERDUTY_SERVICE_ID` maps to 'pagerduty.com/service-id' annotation

#### Types

- `PagerDutyIncident`
- `PagerDutyService`
- `PagerDutyUser`
- `PagerDutyChangeEvent`
- `PagerDutyEscalationPolicy`
- `PagerDutyIncidentUrgencyRule`
- `PagerDutyIntegration`
- `PagerDutyTeam`
- `PagerDutyOnCall`
- `PagerDutyVendor`
- `PagerDutyOnCallsResponse`
- `PagerDutyOnCallUsersResponse`
- `PagerDutyServiceResponse`
- `PagerDutyServicesResponse`
- `PagerDutyIncidentsResponse`
- `PagerDutyChangeEventsResponse`
- `PagerDutyIntegrationResponse`
- `PagerDutyEscalationPoliciesResponse`
- `PagerDutyAbilitiesResponse`

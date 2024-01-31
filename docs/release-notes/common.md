# Release notes for Frontend plugin

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

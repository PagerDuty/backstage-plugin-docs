# Release notes for Backend plugin

## > 0.4.4

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.4.4)

### Summary

This release fixes an issue on the on-call section that was retrieving an unordered list of on-call users instead of an ordered list.

It also fixes a security issue on a backstage dependency (@backstage/backend-common).

### Changes

- build(deps): Bump @backstage/backend-common from 0.19.9 to 0.19.10
- fix(oncall-api): return all users from lowest escalation level

## > 0.4.3

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.4.3)

### Summary

This release resolves an issue reported in backstage-plugin ([#74](https://github.com/PagerDuty/backstage-plugin/issues/74)) which prevents users from overriding the REST API base url (e.g. for EU based accounts). This feature was possible through the Backstage proxy configuration.

With this, users will be able to add a new configuration to the PagerDuty plugin in `app-config.yaml` like the example below. 

```yaml
pagerDuty:
  apiBaseUrl: https://api.eu.pagerduty.com     #defaults to https://api.pagerduty.com
```

### Changes

- fix: adding support for API base url override

## > 0.4.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.4.2)

### Summary

This release introduces a security patch to a third-party dependency.

### Changes

- build(deps): Bump ip from 2.0.0 to 2.0.1

## > 0.4.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.4.1)

### Summary

This release changes the start behaviour for the backend plugin. The former behaviour was to prevent the plugin from starting when credentials were missing. This is not the behaviour for most plugins and therefore this release makes that change to comply with existing behaviour.

Release **0.4.1** allows the plugin to start if credentials are missing, logs an error to simplify troubleshooting and shows an error message in the PagerDuty Card.

<img width="1245" alt="image" src="https://github.com/PagerDuty/backstage-plugin-backend/assets/2689939/5893560a-2988-4e9d-ac2a-75e9a7b26d57">

### Changes

- refactor: change start behaviour on missing credentials

## > 0.4.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.4.0)

### Summary

Release **0.4.0** introduces a minor change that adds a new option configuration field `oauth`. This new configuration improves security when calling PagerDuty REST APIs by replacing the current `apiToken` configuration that assigns full-access privileges to Backstage. 

With **Scoped OAuth** support the PagerDuty admin can grant only the necessary permissions to Backstage instead of access to all APIs and operations.

The new configuration can be defined in `app-config.yaml` with the following parameters:

```yaml
pagerDuty:
  oauth:
    clientId: ${PD_CLIENT_ID}
    clientSecret: ${PD_CLIENT_SECRET}
    subDomain: ${PD_ACCOUNT_SUBDOMAIN}
    region: ${PD_ACCOUNT_REGION}           // Optional. allowed values: 'us', 'eu'. Defaults to 'us'.
```

### Changes

- feat: add OAuth support
- build(deps): Bump @backstage/backend-app-api from 0.5.8 to 0.5.10
- docs: updated readme with new features - REST APIs

## > 0.3.3

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.3.3)

### Summary

This release refactors HTTP error handling in REST API endpoints for backend routes. The new payload expected when an HTTP error is captured looks like the following.

```json
{
    "errors": [
        "Failed to get change events for service. Caller is not authorized to view the requested resource."
    ]
}
```

This helps in providing a better user experience to the user from a frontend perspective.

### Changes

- refactor: improve http error handling in REST API endpoints

## > 0.3.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.3.2)

### Summary

This release bumps up the version of `@pagerduty/backstage-plugin-common` package to version 0.0.2 to solve an issue that was preventing the backend plugin to start.

### Changes

- chore(deps): upgrade common package version to fix issue preventing backend from starting

## > 0.3.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.3.1)

### Summary

This release was aimed at removing the dependency on the Backstage proxy. We have replace it with new REST API endpoints for all operations executed from the frontend plugin that currently interact with the PagerDuty REST API directly. With this change we:

- Removed the dependency on the Backstage proxy
- Improved security by limiting the actions performed on the backend API
- Slightly increase the performance by limiting the data used by the frontend to the essential

**Endpoints added:**

- **/oncall-users** - returns PagerDutyOnCallUsersResponse with list of users oncall
- **/services** - uses integration_key and returns PagerDutyServiceResponse with PagerDuty service information
- **/services/:serviceId** - returns PagerDutyServiceResponse with PagerDuty service information
- **/services/:serviceId/change-events** - returns PagerDutyChangeEventsResponse with list of last 5 change events for the defined service
- **/services/:serviceId/incidents** - returns PagerDutyIncidentsResponse with list of incidents for the defined service

With this change, the proxy configuration on `app-config.yaml` is no longer required.

### Changes

- feat: migrate apis to backend
- build(deps): Bump follow-redirects from 1.15.3 to 1.15.4

## > 0.2.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.2.1)

### Summary

This release introduces the capacity to **enable noise reduction through alert grouping and auto pause of notifications**.

**Note:** This feature requires AIOps. If you don't have the required plan alert grouping will not be enabled.

![create-service-with-noise-reduction](../images/service-noise-reduction.png)

The user will be able to pass an optional parameter (intelligent, time, content_based) from within a software template and during service creation alert grouping will be enabled automatically and using recommended defaults.

Auto pause of notifications will also be enabled by default.

### Changes

- feat: add support for alert grouping

## > 0.2.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.2.0)

### Summary

This new release introduces local APIs that will support the transition from using the Backstage proxy to call PagerDuty APIs and instead use the local APIs provide by the backend plugin. This will allowed the backend to control which data is exposed to Backstage, replacing the current mechanism that exposes all data returned by PagerDuty APIs.

This release only exposes an API route to get all escalation policies which will work together with the frontend plugin and the scaffolder custom action to replace the existing text field expecting an Escalation Policy Id with a Drop-down control that list all escalation policies available to the user, improving their user experience.

![create-service-dropdown](../images/create-pagerduty-service.png)

### Changes

- improv(ux): Adding support for escalation policy dropdown on custom action

## > 0.1.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.1.2)

### Summary

This release introduces a scaffolder custom action that allows users to create a PagerDuty service from a Software Template and configure the frontend plugin in one single step. Go [here](https://pagerduty.github.io/backstage-plugin-docs/advanced/create-service-software-template/) to see how to do it.

### Changes

- feat(scaffolder): custom action to create service in PagerDuty
- docs(release): :memo: simplified readme

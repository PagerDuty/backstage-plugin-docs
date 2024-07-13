# Configuring Backstage

!!! note
    To setup the PagerDuty plugin on Backstage you need to have an API Key - or client id and client secret for OAuth to generate an access token - and an Integration Key generated both for your account and service. If you don't have this information already you must follow the steps described in the [PagerDuty integration](/backstage-plugin-docs/getting-started/pagerduty) section.

## Installing the plugin

!!! note
    The following steps assume you already have a Backstage project created. If that is not the case, follow the [Getting Started](https://backstage.io/docs/getting-started/) guide on Backstage.io to create one or create one by running `npx @backstage/create-app` on your terminal.

To install the PagerDuty plugin into Backstage run the following commands from your Backstage root directory.

```bash
yarn add --cwd packages/app @pagerduty/backstage-plugin # (1)!
```

1. This command adds `@pagerduty/backstage-plugin` package to the `packages/app` folder because it is a frontend plugin.

```bash
yarn add --cwd packages/backend @pagerduty/backstage-plugin-backend # (1)!
```

1. This command adds `@pagerduty/backstage-plugin-backend` package to the `packages/backend` folder because it is a backend plugin.

**That's it!** Now it's time to add the plugin to Backstage and your services.

## Add the frontend plugin to your application

The frontend plugin needs to be added to your application and currently that requires some code changes to the Backstage application. We will do that by updating the `EntityPage.tsx` file in `packages/app/src/components/catalog`.

Add the following imports to the top of the file:

```Typescript
import {
    isPluginApplicableToEntity as isPagerDutyAvailable,
    EntityPagerDutyCard,
} from '@pagerduty/backstage-plugin';
```

Find `const overviewContent` in `EntityPage.tsx`, and add the following snippet inside the outermost `Grid` defined there, just before the closing `</Grid>` tag:

```html
<EntitySwitch>
  <EntitySwitch.Case if={isPagerDutyAvailable}>
    <Grid item md={6}>
      <EntityPagerDutyCard />
    </Grid>
  </EntitySwitch.Case>
</EntitySwitch>
```

Once you are done, the `overviewContent` definition should look similar to this:

```html
const overviewContent = (
  <Grid ...>
    ...
    <EntitySwitch>
      <EntitySwitch.Case if={isPagerDutyAvailable}>
        <Grid item md={6}>
          <EntityPagerDutyCard />
        </Grid>
      </EntitySwitch.Case>
    </EntitySwitch>
  </Grid>
);
```

Now the PagerDuty plugin will be displayed in all your components that include PagerDuty annotations.

!!! note
    The code samples provided above reflect the default configuration of the `PagerDutyCard` entity. You have at your disposal some parameters that allow you to [prevent users from creating incidents](/backstage-plugin-docs/advanced/enable-read-only-mode), or [hide the change events tab](/backstage-plugin-docs/advanced/hide-change-events) or even [hide the on-call](/backstage-plugin-docs/advanced/hide-oncall) section of the card.

## Configure the Frontend plugin

The frontend plugin for PagerDuty is now added to your application but in order for it to show you need to configure your entities and the application itself.

### Annotating entities

For every component that shows up in your Backstage catalog you have a `.yaml` file with its configuration. Add an annotation to the entity like this:

```yaml
annotations:
  pagerduty.com/integration-key: [INTEGRATION-KEY] #(1)!
```

1. The integration key is generated per service and is obtained on the service integrations page on the PagerDuty console. If you don't have one, follow the steps at [Create a service integration for Backstage](/backstage-plugin-docs/getting-started/pagerduty/#create-a-service-integration-for-backstage).

!!! note
    You can optionally decide to annotate with a **service id** instead but you **won't be able to create incidents** from Backstage if you do so and the `Create Incident` button will not show on the PagerDuty card.

    ```yaml
    annotations:
        pagerduty.com/service-id: [SERVICE-ID] #(1)!
    ```

      1. The service id can be found by navigating to a Service with PagerDuty console and pulling the ID value out of the URL (e.g. https://[YOUR-ACCOUNT].pagerduty.com/service-directory/[SERVICE-ID]).

!!! note
    If you are using multiple PagerDuty accounts in your setup you should add an `account` annotation to your Backstage entities. This way the plugin knows with which instance to communicate with. If you don't provide one, the account that was selected as the default one will be used.

    ```yaml
    annotations:
        pagerduty.com/account: [PAGERDUTY-ACCOUNT] #(1)!
    ```

      1. The account value defined here is the PagerDuty account subdomain where the PagerDuty service mapped to your Backstage entity resides (e.g. https://[PAGERDUTY-ACCOUNT].pagerduty.com).

## Add the backend plugin to your application

!!! note
    Version _0.6.0_ of the backend plugin (`@pagerduty/backstage-plugin-backend`) introduced support for Backstage's [new backend system](https://backstage.io/docs/backend-system/) which simplifies the backend configuration and requires less code.

If you followed the steps in _"Installing the plugin"_, the backend plugin for PagerDuty is now added to your application but in order for it to expose its capabilities to the frontend plugin you need to configure it. There are two approaches for that: **Legacy Backend System** or the **New Backend System**.

!!! warning
    If you were using the PagerDuty plugin for Backstage before the release of `@pagerduty/backstage-plugin-backend@0.6.0` then you already have the Backend configured and you are probably looking for some migration steps, correct?

    Check the migration guidance [here](/backstage-plugin-docs/advanced/backend-system-migration).

### Legacy Backend System

Create a new file called `pagerduty.ts` at `packages/backend/src/plugins/pagerduty.ts` and add the following content:

```Typescript
import { Router } from 'express';
import { PluginEnvironment } from '../types';
import { createRouter } from '@pagerduty/backstage-plugin-backend';

export default async function createPlugin(
    env: PluginEnvironment,
): Promise<Router> {
    return await createRouter({
        config: env.config,
        logger: env.logger,
    });
}
```

This creates the backend plugin that you can now configure in your application.

In `packages/backend/src/index.ts` import your plugin and add a route for the APIs exposed by PagerDuty's backend plugin.

```TypeScript
import pagerduty from './plugins/pagerduty';
// ...
async function main() {
  // ...
  const pagerdutyEnv = useHotMemoize(module, () => createEnv('pagerduty'));
  // ...
  apiRouter.use('/pagerduty', await pagerduty(pagerdutyEnv));
```

### New Backend System

Backstage's new backend system requires less code to setup plugins. Just open `packages/backend/src/index.ts` file and add the PagerDuty backend plugin to your Backstage App as shown below.

```typescript

// pageduty plugin
backend.add(import('@pagerduty/backstage-plugin-backend'));

```

## Configure API Authorization

The PagerDuty plugin requires access to PagerDuty APIs and so we need to configure our Backstage app with the necessary credentials to reach the APIs. This step requires you to use an access token - for OAuth - or an API token.

!!! note
    If you followed previous steps you should have this information already but if you haven't done so follow the steps in [Register an App](/backstage-plugin-docs/getting-started/pagerduty/#register-an-application-for-scoped-oauth-recommended) to get the _client id_ and _client secret_ for OAuth authorization or [Generate a General Access REST API Token](/backstage-plugin-docs/getting-started/pagerduty/#generate-a-general-access-rest-api-token) to generate a REST API Token.

### Single PagerDuty Account

If all your services exist in a single PagerDuty account you should follow the instructions below.

#### Scoped OAuth (recommended)

In `app-config.yaml` file add the following configuration and set your OAuth configuration:

```yaml
pagerDuty:
  oauth:
    clientId: ${PD_CLIENT_ID}
    clientSecret: ${PD_CLIENT_SECRET}
    subDomain: ${PD_ACCOUNT_SUBDOMAIN}
    region: ${PD_ACCOUNT_REGION}  // Optional. allowed values: 'us', 'eu'.
                                  // Defaults to 'us'.
```

#### REST API Token

In `app-config.yaml` file add the following configuration to set your REST API Token:

```yaml
pagerDuty:
  apiToken: ${PAGERDUTY_TOKEN}
```

!!! warning
    If you were using the plugin **before version 0.8.1** of the frontend or **version 0.3.1** of the backend you need to configure a proxy configuration instead. That configuration is now deprecated so use this configuration instead.

### Multiple PagerDuty Accounts

Companies may have more than one PagerDuty account for many reasons. It might be because they want to segregate access to information to specific teams, or maybe because the company acquired any company that was already a PagerDuty customer.

Independent of the reason, we added multi-account support on version 0.14.0 of the frontend plugin (@pagerduty/backstage-plugin). In order to configure it you should follow the steps below.

#### Scoped OAuth (recommended)

In `app-config.yaml` file add the following configuration and set your OAuth configuration:

```yaml
pagerDuty:
  accounts:
  - id: ${PD_ACCOUNT_SUBDOMAIN_1}    // The ID must be the subdomain for the account
    isDefault: true                  // Only one account can be defined as the default/fallback
    apiBaseUrl: ${PD_API_BASE_URL_1}
    oauth:
      clientId: ${PD_CLIENT_ID_1}
      clientSecret: ${PD_CLIENT_SECRET_1}
      subDomain: ${PD_ACCOUNT_SUBDOMAIN_!}
      region: ${PD_ACCOUNT_REGION_1}  // Optional. allowed values: 'us', 'eu'.
                                  // Defaults to 'us'.
  - id: ${PD_ACCOUNT_SUBDOMAIN_2}
    apiBaseUrl: ${PD_API_BASE_URL_2}
    oauth:
      clientId: ${PD_CLIENT_ID_2}
      clientSecret: ${PD_CLIENT_SECRET_2}
      subDomain: ${PD_ACCOUNT_SUBDOMAIN_2}
```

#### REST API Token

In `app-config.yaml` file add the following configuration to set your REST API Token:

```yaml
pagerDuty:
  accounts:
  - id: ${PD_ACCOUNT_SUBDOMAIN_1}    // The ID must be the subdomain for the account
    isDefault: true                  // Only one account can be defined as the default/fallback
    apiBaseUrl: ${PD_API_BASE_URL_1}
    apiToken: ${PAGERDUTY_TOKEN}
  - id: ${PD_ACCOUNT_SUBDOMAIN_2}
    apiToken: ${PAGERDUTY_TOKEN}
```

!!! NOTE
    In the new multi-account setup you can have accounts configured with **Scoped OAuth** and others with **REST API Token**. You can also specify custom API base url and events url for some and not others. All optional properties will revert to default values if not present.

## Test your configuration

Start your Backstage app, passing the PagerDuty API token or OAuth parameters as a environment variables.

### For Scoped OAuth

```bash
PD_CLIENT_ID='<ID>' PD_CLIENT_SECRET='<SECRET>' PD_ACCOUNT_SUBDOMAIN='<SUBDOMAIN>' PD_ACCOUNT_REGION='<REGION>'  yarn dev
```

### For REST API Token

```bash
PAGERDUTY_TOKEN='<TOKEN>' yarn dev
```

This will add an `Authorization` header to all the requests made to PagerDuty REST APIS.

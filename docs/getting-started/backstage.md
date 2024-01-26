# Configuring Backstage

!!! note
    To setup the PagerDuty plugin on Backstage you need to have an API Key and an Integration Key generated both for your account and service. If you don't have this information already you must follow the steps described in the [PagerDuty integration](/getting-started/pagerduty) section.

## Installing the plugin

!!! note
    The following steps assume you already have a Backstage project created. If that is not the case, follow the [Getting Started](https://backstage.io/docs/getting-started/) guide on Backstage.io to create one or create one by running `npx @backstage/create-app` on your terminal.

To install the PagerDuty plugin into Backstage run the following commands from your Backstage root directory.

```bash
yarn add --cwd packages/app @pagerduty/backstage-plugin @pagerduty/backstage-plugin-common # (1)! 
```

1. This command adds `@pagerduty/backstage-plugin` and `@pagerduty/backstage-plugin-common` packages to the `packages/app` folder because it is a frontend plugin.

```bash
yarn add --cwd packages/backend @pagerduty/backstage-plugin-backend @pagerduty/backstage-plugin-common # (1)! 
```

1. This command adds `@pagerduty/backstage-plugin-backend` and `@pagerduty/backstage-plugin-common` packages to the `packages/backend` folder because it is a backend plugin.

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

## Configure the Frontend plugin

The frontend plugin for PagerDuty is now added to your application but in order for it to show you need to configure your entities and the application itself.

### Annotating entities

For every component that shows up in your Backstage catalog you have a `.yaml` file with its configuration. Add an annotation to the entity like this:

```yaml
annotations:
  pagerduty.com/integration-key: [INTEGRATION-KEY] #(1)!
```

1. The integration key is generated per service and is obtained on the service integrations page on the PagerDuty console. If you don't have one, follow the steps at [Create a service integration for Backstage](/getting-started/pagerduty/#create-a-service-integration-for-backstage).

!!! note
    You can optionally decide to annotate with a **service id** instead but you **won't be able to create incidents** from Backstage if you do so.

    ```yaml
    annotations:
        pagerduty.com/service-id: [SERVICE-ID] #(1)!
    ```

      1. The service id can be found by navigating to a Service with PagerDuty console and pulling the ID value out of the URL (e.g. https://[YOUR-ACCOUNT].pagerduty.com/service-directory/[SERVICE-ID]).

### Configure API Authorization

The PagerDuty plugin requires access to PagerDuty APIs and so we need to configure our Backstage app with the necessary credentials to reach the APIs.

This step requires you to use the API token you generated and stored safely in previous steps. If you haven't done so follow the steps in [Generate a General Access REST API Token](/getting-started/pagerduty/#generate-a-general-access-rest-api-token).

In `app-config.yaml` file add the following configuration and set the API token:

```yaml
pagerDuty:
  apiToken: ${PAGERDUTY_TOKEN}
```

!!! warning
    If you were using the plugin **before version 0.8.1** you need to configure a proxy configuration instead. That configuration is now deprecated so use this configuration instead.

## Add the backend plugin to your application

If you followed the steps in *"Installing the plugin"*, the backend plugin for PagerDuty is now added to your application but in order for it to expose its capabilities to the frontend plugin you need to configure it.

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
  apiRouter.use('/pagerduty', await todo(pagerdutyEnv));
```

### Configure Backend plugin API credentials

The PagerDuty backend plugin exposes local APIs that query PagerDuty APIs without going through the Backstage proxy. Therefore it requires access to the API Token used in the previous step.

If you plan to use the backend plugin add the following to your `app-config.yaml` file.

```yaml
pagerDuty:
  apiToken: ${PAGERDUTY_TOKEN}
```

!!! warning
    If you were using the plugin **before version 0.3.1** you need to configure a proxy configuration instead. That configuration is now deprecated so use this configuration instead.

## Test your configuration

Start your Backstage app, passing the PagerDuty API token as an environment variable:

```bash
PAGERDUTY_TOKEN='<TOKEN>' yarn dev
```

This will proxy the request by adding an `Authorization` header with the provided token.

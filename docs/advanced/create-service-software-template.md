# Create PagerDuty service with Software Templates

The PagerDuty backend plugin provides a custom action that you can use in your [Backstage Software Templates](<https://backstage.io/docs/features/software-templates/>) to create services in PagerDuty easily. This custom action not only creates the service, but also automatically configures a Backstage integration and adds its integration key to the Backstage Service configuration.

By doing so, it enables and configures the PagerDuty Card provided by the frontend plugin directly on your service page, eliminating the need for manual configuration and enhancing the user experience.

## Adding the Scaffolder Actions plugin

!!! note
    Version 0.6.0 of `@pagerduty/backstage-plugin-backend` introduced support for the Backstage's new backend system which forced the extraction of the scaffolder actions to a separate package ([@pagerduty/backstage-plugin-scaffolder-actions](https://www.npmjs.com/package/@pagerduty/backstage-plugin-scaffolder-actions)).

    If you were already using the scaffolder actions before this, follow the migration guide [here](/backstage-plugin-docs/advanced/backend-system-migration) as you need to update the package used in the code.

```bash
yarn add --cwd packages/backend @pagerduty/backstage-plugin-scaffolder-actions @pagerduty/backstage-plugin-common # (1)!
```

1. This command adds `@pagerduty/backstage-plugin-scaffolder` and `@pagerduty/backstage-plugin-common` packages to the `packages/backend` folder because it is a backend module.

## Adding the custom action to the project

You can add the custom action to the project in two different ways. Using the **legacy backend system** or the **new backend system**. Follow one of the approaches detailed below.

### Legacy backend system

Backstage Scaffolder capabilities can be extended with custom actions that support Software Templates. For that to happen you need to update `packages/backend/src/plugins/scaffolder.ts` and add custom actions.

Now, the list of scaffolder actions cannot be appended so you need re-create it and append your custom action. Otherwise all actions will be replaced just with yours.

```typescript

// Add imports
import { createBuiltinActions } from '@backstage/plugin-scaffolder-backend';
import { ScmIntegrations } from '@backstage/integration';
import { createPagerDutyServiceAction } from '@pagerduty/backstage-plugin-scaffolder-actions';

export default async function createPlugin(
  env: PluginEnvironment,
): Promise<Router> {

  ...

  // Pull integrations
  const integrations = ScmIntegrations.fromConfig(env.config);

  // Rebuild built-in actions
  const builtInActions = createBuiltinActions({
    integrations,
    catalogClient,
    config: env.config,
    reader: env.reader,
  });

  // Append PagerDuty custom action to the list
  const actions = [
    ...builtInActions, 
    createPagerDutyServiceAction()
  ];

  // Add new action list to the scaffolder
  return await createRouter({
    actions,                // this is the only update needed on this list
    logger: env.logger,
    config: env.config,
    database: env.database,
    reader: env.reader,
    catalogClient,
    identity: env.identity,
    permissions: env.permissions,
  });
}
```

This step registers the custom action with the Scaffolder and allows it to be used in the Software template which you will configure on next step.

### New backend system

Backstage's new backend system simplifies the configuration of backend plugins and requires less code to setup plugins. To add the PagerDuty scaffolder actions to your Backstage application add the following in `packages/backend/src/index.ts`.

```typescript
backend.add(import('@pagerduty/backstage-plugin-scaffolder-actions'));
```

## Adding the custom action to a Software Template

Software Templates can be simple or complex depending on the practices you are trying to standardize across your teams. Here, we provide a simple example of a template that requests basic information for a Backstage service and augments that with information relevant for creating the service in PagerDuty.

!!! note
    We assume you have an `examples/template/content` folder which is automatically create when you use `npx @backstage/create-app` to create your Backstage project.

### Create project configuration file

In your `examples/template/content` folder you should create `catalog-info.yaml` file if you don't have one already. Update its content to something like this:

```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: ${{ values.name | dump }}
  annotations:
    pagerduty.com/integration-key: ${{ values.integrationKey | dump }}
    pagerduty.com/service-id: ${{ values.serviceId | dump }}
spec:
  type: website
  lifecycle: experimental
  owner: guests
```

In the software template we will either replace the values for the `integration-key`, `service-id` and `name` or completely remove the parameters from the configuration.

!!! note
    You don't need to use this exact template but to automate the configuration for the PagerDuty Card you need to have at least the `integration-key` or the `service-id` parameters.

### Create the Software Template

Now that we have defined the contents of the new project we will be creating through a Software Template, we need to create the template itself.

Software templates are composed of **input parameters**, **steps** and **outputs**. In the following template we provide the user with two pages:

1. Collecting information about the service itself
2. Collecting information on where the code is going to be hosted

Once all the information is provided by the user we will:

1. create a Service in PagerDuty
2. parse the `catalog-info.yaml` file and replace the variables with the values generated by the previous step
3. publish the code to GitHub
4. register the component in Backstage

!!! note
    For the following template to work, you need to configure the `apiToken` in `app-config.yaml` file. If you haven't done so, follow the steps in [Configure Backend plugin API credentials](/backstage-plugin-docs/getting-started/backstage/#optional-configure-backend-plugin-api-credentials)

    **Note:** If you don't setup this property in your configuration, the backend plugin will fail to start.

!!! note
    The UI component that allows users to select the *Escalation Policy* when creating a new service in PagerDuty depends on an open source component. For the template to work properly please install it before.

    ```bash
    yarn add --cwd packages/app @roadiehq/plugin-scaffolder-frontend-module-http-request-field
    ```

    Once it is installed, go to `/packages/app/src/App.tsx` and find `<ScaffolderPage />`. Add the UI component to `ScaffolderFieldExtensions`. It should look like this when you finish.

    ```yaml
    import { ScaffolderFieldExtensions } from '@backstage/plugin-scaffolder-react';
    import { SelectFieldFromApiExtension } from '@roadiehq/plugin-scaffolder-frontend-module-http-request-field';

    ...

    <Route path="/create" element={<ScaffolderPage />}>
      <ScaffolderFieldExtensions>
        <SelectFieldFromApiExtension />
      </ScaffolderFieldExtensions>
    </Route>
    ```
Once all requirements are in-place, create a `template.yaml` file under `examples/template` and copy the following code in there.

```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: create-pagerduty-service
  title: Create PagerDuty Service
  description: Creates service in PagerDuty
spec:
  owner: pagerduty
  type: service

  parameters:
    - title: PagerDuty Service
      required:
        - service_name
        - description
        - escalation_policy_id
      properties:
        service_name:
          title: Service Name
          type: string
          description: The name of the service
        description:
          title: Description
          type: string
          description: The description of the service
        escalation_policy_id:
          title: Escalation Policy ID
          type: string
          description: The ID of the escalation policy to associate with the service
          ui:field: SelectFieldFromApi #(1)!
          ui:options: #(2)!
            title: PagerDuty Escalation Policy
            description: Select an escalation policy from PagerDuty
            path: 'pagerduty/escalation_policies' #(3)!
            labelSelector: 'label'
            valueSelector: 'value'
            placeholder: '---'
        alert_grouping: #(4)!
          title: Alert Grouping
          type: string
          description: Reduce noise by grouping similar alerts - Defaults to 'None'.
          enum:
            - 'time'
            - 'intelligent'
            - 'content_based'
          enumNames:
            - 'Time-based grouping'
            - 'Intelligent grouping'
            - 'Content-based grouping'

    - title: Choose a location
      required:
        - repoUrl
      properties:
        repoUrl:
          title: Repository Location
          type: string
          ui:field: RepoUrlPicker
          ui:options:
            allowedHosts:
              - github.com

  steps:
    - id: pagerdutyService
      name: Create PagerDuty Service
      action: pagerduty:service:create
      input:
        name: ${{ parameters.service_name }}
        description: ${{ parameters.description }}
        escalationPolicyId: ${{ parameters.escalation_policy_id }}
        alertGrouping: ${{ parameters.alert_grouping }} #(5)!

    - id: fetch-base
      name: Fetch Base
      action: fetch:template
      input:
        url: ./content
        templateFileExtension: '.yaml'
        values:
          name: ${{ parameters.service_name }}
          serviceId: ${{ steps['pagerdutyService'].output.serviceId }}
          integrationKey: ${{ steps['pagerdutyService'].output.integrationKey }}

    - id: publish
      name: Publish
      action: publish:github
      input:
        allowedHosts: ['github.com']
        description: This is ${{ parameters.name }}
        repoUrl: ${{ parameters.repoUrl }}

    - id: register
      name: Register
      action: catalog:register
      input:
        repoContentsUrl: ${{ steps['publish'].output.repoContentsUrl }}
        catalogInfoPath: '/catalog-info'

  output:
    links:
      - title: Open in PagerDuty
        url: ${{ steps['pagerdutyService'].output.serviceUrl }}
    text:
      - title: Integration Key
        text: ${{ steps['pagerdutyService'].output.integrationKey }}

```

1. Open source dropdown component from `@roadiehq` that queries data from a local API
2. Options for the dropdown component
3. The local api exposed by the PagerDuty backend plugin that retrieves a list of key/value pairs
4. This UI field is optional. If you want to enforce a specific method you can just set the value in the backend component.
5. This parameter is optional. You can enforce a specific method by choosing 'intelligent', 'time' or 'content_based'. If not defined, no alert grouping will be configured.
   **[Requires AIOps](https://www.pagerduty.com/platform/aiops/).**

This is an easy mechanism for onboarding new services in an automated way, ensuring that Backstage and PagerDuty services can be **provisioned with one step and in a self-service way**.

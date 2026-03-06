# From Backstage-owned plugin

If you are migrating from the PagerDuty plugin that was maintained by Backstage ([@backstage/plugin-pagerduty](https://www.npmjs.com/package/@backstage/plugin-pagerduty)), the steps to migrate are pretty straightforward.

1. Remove the legacy PagerDuty plugin from your Backstage project

    ```bash
    # From your Backstage root directory
    yarn remove --cwd packages/app @backstage/plugin-pagerduty
    ```

2. Install the new frontend PagerDuty plugin for Backstage

    ```bash
    # From your Backstage root directory
    yarn --cwd packages/app add @pagerduty/backstage-plugin @pagerduty/backstage-plugin-common
    ```

3. Replace all occurrences of `@backstage/plugin-pagerduty` with `@pagerduty/backstage-plugin` in your components

4. Install the backend PagerDuty plugin for Backstage

    ```bash
    yarn --cwd packages/backend add @pagerduty/backstage-plugin-backend @pagerduty/backstage-plugin-common
    ```

5. [Add the backend plugin](/backstage-plugin-docs/getting-started/backstage/#add-the-backend-plugin-to-your-application) to your Backstage project

6. [Configure API credentials](/backstage-plugin-docs/getting-started/backstage/#configure-backend-plugin-api-credentials) to make requests to PagerDuty REST API

7. Remove PagerDuty proxy configuration from your `app-config.yaml` file

8. Re-install dependencies and run Backstage locally to test that everything is working

    ```bash
    # From your Backstage root directory
    yarn install && yarn dev
    ```

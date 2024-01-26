# Migration

If you are migrating from the PagerDuty plugin that was maintained by Backstage, the steps to migrate are pretty straight forward.

1. Remove the Backstage package from your project

    ```bash
    # From your Backstage root directory
    yarn remove --cwd packages/app @backstage/plugin-pagerduty
    ```

2. Install the new PagerDuty plugin for Backstage

    ```bash
    # From your Backstage root directory
    yarn add --cwd packages/app @pagerduty/backstage-plugin @pagerduty/backstage-plugin-common
    ```

3. Replace all occurrences of `@backstage/plugin-pagerduty` with `@pagerduty/backstage-plugin` in your components

4. Re-install dependencies and run Backstage locally to test that everything is working

    ```bash
    # From your Backstage root directory
    yarn install && yarn dev
    ```

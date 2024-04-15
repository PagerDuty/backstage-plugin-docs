# Migrate to Backstage's new backend system

Version 0.6.0 of `@pagerduty/backstage-plugin-backend` introduced support for Backstage's new backend system while remaining backwards compatible. So, if you want to continue using the legacy backend system you can do so. There's no rush!

Still, a couple things changed. Due to an architecture change on the new backend system we add to extract the scaffolder actions to a new Backstage module ([@pagerduty/backstage-plugin-scaffolder-actions](https://www.npmjs.com/package/@pagerduty/backstage-plugin-scaffolder-actions)).

We are aware of the slight impact for existing users but with this decision we are able to support all customers and allow users to migrate their Backstage applications to the new model as they wish.

## Upgrading to the new package only (legacy backend system)

If you choose to upgrade to `@pagerduty/backstage-plugin-backend` 0.6.0 and still use Backstage's legacy backend system follow the steps below for a smooth transition.

1. Add and install the new package

    ```bash
    yarn add --cwd packages/backend @pagerduty/backstage-plugin-backend  # or upgrade to 0.6.0
    yarn add --cwd packages/backend @pagerduty/backstage-plugin-scaffolder-actions
    yarn install
    ```

2. Update package name in `packages/backend/src/plugins/scaffolder.ts`

    ```typescript
    import { createPagerDutyServiceAction } from '@pagerduty/backstage-plugin-scaffolder-actions';
    ```

## The new backend system

The new backend system reduces drastically the amount of code needed to setup plugins in Backstage. Assuming that you already migrated to the new backend system by following [these steps](https://backstage.io/docs/backend-system/building-backends/migrating/) outlined by the Backstage team, follow the steps below to configure the PagerDuty Scaffolder Actions.

1. Add and install the new package

    ```bash
    yarn add --cwd packages/backend @pagerduty/backstage-plugin-backend  # or upgrade to 0.6.0
    yarn add --cwd packages/backend @pagerduty/backstage-plugin-scaffolder-actions
    yarn install
    ```

2. Add backend middleware in `packages/backend/src/index.ts`

    ```typescript
    backend.add(import('@pagerduty/backstage-plugin-backend'));
    backend.add(import('@pagerduty/backstage-plugin-scaffolder-actions'));
    ```

You can now build and run your application. That's it! If you need more details on how to integrated the scaffolder action with Templates follow the guidance provided [here](/backstage-plugin-docs/advanced/create-service-software-template/#create-the-software-template).

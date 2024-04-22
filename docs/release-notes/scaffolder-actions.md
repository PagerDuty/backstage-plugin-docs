# Release notes for Scaffolder Actions module

## > 0.1.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin-scaffolder-actions/releases/tag/0.1.2)

### Summary

This release updates the `moduleId` to a PagerDuty specific one (`pagerduty-actions`). The `moduleId` configured on the initial release (`custom-extensions`) conflicts with the example value from the [backstage documentation for writing custom actions](https://backstage.io/docs/features/software-templates/writing-custom-actions#register-action-with-new-backend-system).

### Changes

- fix: use pagerduty-specific moduleId

### Dependencies

- `@pagerduty/backstage-plugin-common: ^0.1.2`

## > 0.1.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin-scaffolder-actions/releases/tag/0.1.1)

### Summary

This release introduces a small improvement to the migration process to the new backend system. With the change introduced we made the `createPagerDutyServiceAction` parameters optional and therefore existing users just need to update the package reference in `packages/backend/src/plugins/scaffolders.ts`.

### Changes

- refactor: refactor to avoid additional parameters

### Dependencies

- `@pagerduty/backstage-plugin-common: ^0.1.2`

## > 0.1.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin-scaffolder-actions/releases/tag/0.1.0)

### Summary

Version **0.1.0** of `@pagerduty/backstage-plugin-scaffolder-actions` represents the first release of this new Backstage backend module. 

Before version **0.6.0** of `@pagerduty/backstage-plugin-backend`, which introduces support for [Backstage's new backend system](https://backstage.io/docs/backend-system/), both REST APIs and Scaffolder actions were part of the same package.

By adding support for the new backend system, we were forced to isolate the existing Scaffolder Actions in a separate package. Below are the steps to onboard this new package to your Backstage App.

**For existing users using the legacy backend system**

1. Install new package

    ```bash
    yarn add --cwd packages/backend @pagerduty/backstage-plugin-scaffolder-actions
    yarn install
    ```

2. Update your existing reference in `packages/backend/src/plugins/scaffolder.ts` from the backend component to the new one

    ```bash
    import { createPagerDutyServiceAction } from '@pagerduty/backstage-plugin-scaffolder-actions';
    ```

3. Pass the environment _config_ and _logger_ to `createPagerDutyServiceAction`

    ```typescript
      const actions = [
         ...builtInActions, 
         createPagerDutyServiceAction({ 
              config: env.config, 
              logger: env.logger 
         })
      ];
    ```

**For users using the new backend system**

1. Install new package

    ```bash
    yarn add --cwd packages/backend @pagerduty/backstage-plugin-scaffolder-actions
    yarn install
    ```

2. **(Optional)** If this is the first time configuring PagerDuty's plugin you also need the following packages

    ```bash
    yarn add --cwd packages/backend @pagerduty/backstage-plugin-backend @pagerduty/backstage-plugin-common
    yarn install
    ```

3. Add the package to your backend in `packages/backend/src/index.ts`

    ```typescript
    // PagerDuty backend plugin
    backend.add(import('@pagerduty/backstage-plugin-backend'));
   
    // PagerDuty Scaffolder Actions Module
    backend.add(import('@pagerduty/backstage-plugin-scaffolder-actions'));
    ```

### Changes

- chore: extract scaffolder actions from `@pagerduty/backstage-plugin-backend`
- chore(deps): Bump tar from 6.2.0 to 6.2.1

### Dependencies

- `@pagerduty/backstage-plugin-common: ^0.1.2`

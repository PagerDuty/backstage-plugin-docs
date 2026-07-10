# Entity Processor

The PagerDuty plugin for Backstage is made up of three packages: the frontend plugin (`@pagerduty/backstage-plugin`), the backend plugin (`@pagerduty/backstage-plugin-backend`), and the **Entity Processor** module (`@pagerduty/backstage-plugin-entity-processor`).

The frontend and backend plugins are **required** for the plugin to work at all. The Entity Processor is **optional, but highly recommended** — most of the plugin's synchronization capabilities depend on it.

## What does it do?

The Entity Processor is a Backstage catalog backend module. It hooks into Backstage's catalog processing pipeline and, for every relevant entity, reconciles PagerDuty-related configuration into that entity's catalog data as it's processed.

## Do you need it?

There are two ways to define a mapping between a PagerDuty service and a Backstage entity:

1. **Statically, in the entity's `catalog-info.yaml`**, using annotations such as `pagerduty.com/service-id`. These annotations are read directly by the frontend plugin regardless of whether the Entity Processor is installed.
2. **Dynamically, using the `/pagerduty` page's service mapping UI**. Mappings created this way are persisted only to the Backstage database — they are **not** written to `catalog-info.yaml`. The Entity Processor is the only component that reads these database-stored mappings and applies them to the corresponding Backstage entity.

If the Entity Processor is **not installed**, the plugin only works with mappings defined directly in YAML. A mapping created through the `/pagerduty` UI will be saved to the database, but it will never be synced onto the actual entity — so it will silently not work.

## What doesn't work without it

Without the Entity Processor installed:

- **Service mappings created via the `/pagerduty` UI page never take effect.** The mapping is stored in the database but never applied to the Backstage entity. Mappings defined in `catalog-info.yaml` are unaffected and continue to work.
- **[Service dependency sync](configure-service-dependency-sync.md) does not run.** Dependencies are never synced between Backstage and PagerDuty in either direction.
- **Custom field sync to PagerDuty does not run.** Backstage entity data mapped to PagerDuty Custom Fields is never pushed.
- YAML-defined mappings are never backfilled into the database, so mapping-status bookkeeping (e.g. the "in sync" / "out of sync" indicators on the `/pagerduty` page) won't reflect them accurately.

## Installing and configuring the Entity Processor

The setup steps differ depending on whether you are running a self-hosted Backstage instance or Backstage Portal. Follow the section that matches your environment.

!!! note
    The following instructions assume that you already installed the frontend and backend plugin as described in the [Getting Started page](/backstage-plugin-docs/getting-started/backstage).

### Backstage

In a self-hosted Backstage instance you install the package and wire it into the backend system yourself.

First, install the package from your Backstage root directory.

```bash
yarn --cwd packages/backend add @pagerduty/backstage-plugin-entity-processor
```

Then enable the Entity Processor by injecting the dependency in the backend system in `packages/backend/src/index.ts`.

```typescript
  import { createBackend } from '@backstage/backend-defaults';

  const backend = createBackend();
  
  ...

  backend.add(import('@pagerduty/backstage-plugin-entity-processor')); // <-- This is the line you want to add
  
  backend.start();
```

And that is it for the Entity Processor module. This module will process all Backstage entities and if there are any changes persisted to the database they will be applied automatically.

### Backstage Portal

Backstage Portal manages backend modules through its UI, so there are no code changes to make. After installing the package, you enable the module from the Portal admin interface.

1. Install the `@pagerduty/backstage-plugin-entity-processor` package in your Portal instance.
2. In the left navigation bar, go to **Plugins**.
3. Select the **Catalog** plugin.
4. Open the **Modules** tab.
5. Locate the `@pagerduty/backstage-plugin-entity-processor` module and click **Manage module**.
6. Click **Start**.
7. Wait for the **Applying new configuration...** message to disappear.

Once the configuration has been applied, the Entity Processor is active. It will process all Backstage entities and automatically apply any changes persisted to the database.

# Release notes for Frontend plugin

## > 0.7.4

[GitHub release](https://github.com/PagerDuty/backstage-plugin/releases/tag/0.7.4)

### Summary

This release includes a fix to a bug that prevented the PagerDutyCard from refreshing when users leveraged the Backstage search to navigate between components.

### 🐛 Bug fixes

- fix: component now refreshes when navigating between components from Search

## > 0.7.3

[GitHub release](https://github.com/PagerDuty/backstage-plugin/releases/tag/0.7.3)

### Summary

This release updates the Backstage plugin configuration schema to add support for `apiToken`.

```yaml
pagerDuty:
   apiToken: u+a81u12y4ax
```

The latest release of the backend plugin ([0.2.0](https://github.com/PagerDuty/backstage-plugin-backend/releases/tag/0.2.0)) required a schema change for the plugin to start successfully. It does not introduce new features or capabilities to this plugin but it makes sure that the schemas match and avoid misconfigurations.

### 🔧 Maintenance

- chore(schema): updated config schema to match backend plugin

## > 0.7.2

[GitHub release](https://github.com/PagerDuty/backstage-plugin/releases/tag/0.7.2)

### Summary

This release fixes an issue that prevented the installation of the plugin if the Backstage React version was not compatible with the plugin version. It also fixes a moderate security issue, adds support for React 18+ and bumps up the versions of Backstage packages.

### 🔧 Maintenance

- chore(deps): add support for React 18+ (#43)
- chore(deps): bump up Backstage versions (#44)
- docs: Simplify readme file and point to full documentation (#47)
- chore(deps): bump @adobe/css-tools from 4.3.1 to 4.3.2
- fix: move strict dependencies

## > 0.7.1

[GitHub release](https://github.com/PagerDuty/backstage-plugin/releases/tag/0.7.1)

### Summary

With release 0.7.1 we have fixed some issues and added new capabilities to the oncall user list.

- List only users in escalation level 1 - these are the users that are actually oncall.
- Remove duplicate users from the oncall user list
- Add support for user profile picture and fallback to dummy avatar icon when an image is not provided.

### 🌟 Minor Changes

- fix: list only oncall users in plugin

## > 0.7.0

[GitHub release](https://github.com/PagerDuty/backstage-plugin/releases/tag/0.7.0)

### Summary

This release marks the first version under PagerDuty's ownership of the Backstage plugin. You will not notice any major changes. Here's what we did:

- Fixed a bug that was preventing recent changes tab to show
- Removed double headers on plugin UI component for incidents and recent changes
- Patched potential security vulnerabilities on dependencies

### 🔧 Maintenance

- chore(deps): bump zod from 3.22.2 to 3.22.4
- chore(deps): bump browserify-sign from 4.2.1 to 4.2.2
- chore(deps): bump @babel/traverse from 7.22.20 to 7.23.2
- chore(deps): bump postcss from 8.4.29 to 8.4.31
- chore(ci): adding release drafter with config
- chore(deps)Bump graphql from 16.8.0 to 16.8.1
- chore(ci): adding github action to publish package on new release
- chore(docs): adding CODEOWNERS
- chore(ci): changing release step to trigger on pr close and merge
- chore(ci): forcing npm registry by default
- chore(ci): Build and publish package to npm

### 🌟 Minor Changes

- fix(ui): Removing sub-headers from lists

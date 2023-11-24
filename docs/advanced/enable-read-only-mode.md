# Enable read-only mode

PagerDuty plugin allows users to create incidents directly from Backstage. This feature is enabled by default but you can decide to disable it.

![create-incident-button](../images/create-incident-button.png)

## Component read-only mode

To suppress the rendering of the actionable incident creation button, the `PagerDutyCard` can also be instantiated in `readOnly` mode as shown below.

```html
<EntityPagerDutyCard readOnly>
```

This will hide the button for all users and prevent incidents from being created from Backstage.

![read-only-card](../images/create-incident-button-hidden.png)

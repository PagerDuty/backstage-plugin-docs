# PagerDutyCard Component

The `PagerDutyCard` is the standard component that you can add to the Entity page. This card allows you to:

- check the **status** of your service
- access relevant **service metrics**
- track **service standards** compliance
- see all **active incidents** and **recent changes**
- check who is **on call**
- **open service and incidents** in PagerDuty
- **create new incidents**

![pagerdutycard-component](../images/list-incidents.png)

You can add the `PagerDutyCard` to your Backstage application easily by following the steps highlighted in [Configuring Backstage](/backstage-plugin-docs/getting-started/backstage).

The *on call* and *change events* tabs can be removed from the Card by following the steps detailed in [hide change events](/backstage-plugin-docs/advanced/hide-change-events) and [hide on call](/backstage-plugin-docs/advanced/hide-oncall).

You can optionally also disable the chance to *create new incidents* from the card by making it `read-only`. Refer to the [documentation](/backstage-plugin-docs/advanced/enable-read-only-mode) to see how to do it.

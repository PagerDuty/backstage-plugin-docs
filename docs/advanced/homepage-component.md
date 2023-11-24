# Create an homepage component

The most common use case for the plugin is to have an instance of the plugin in each service. Still, you might want to have multiple instances of the plugin in your homepage to provide visibility on multiple services at the same time.

If that is your case you might want to leverage the `PagerDutyHomepageCard` component like so.

```html
...
export const homePage = (
  <Page themeId="home">
    ...
    <Content>
      <CustomHomepageGrid config={defaultConfig}>
        ...
        <PagerDutyHomepageCard
          name="My Service"
          serviceId="<service-id>"
          integrationKey="<integration-key>"
          readOnly={false}
        />
      </CustomHomepageGrid>
    </Content>
  </Page>
);
```

This component requires you to specify the `service-id` and `integration-key` properties which allows for added flexibility.

!!! note
    We are aware that this scenario is not ideal as the homepage will be full of different components and will be difficult to navigate on and for that reason we are working on a component that provides an aggregated view on multiple services.

    Until we release that capability this is the best option if you want to provide visibility on multiple services on the same page.
    
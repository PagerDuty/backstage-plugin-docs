# Welcome 👋

If you are looking for documentation on how to use and configure the **PagerDuty plugin for Backstage** you are in the right place!

Before you start let's just set our baseline straight and explain who we are and why we are doing this.

## What is PagerDuty?

### We're revolutionizing Operations

[PagerDuty](https://www.pagerduty.com/) is transforming critical work for modern business. Our powerful and unique platform makes sure you can take the right action, when seconds matter. From developers and reliability engineers to customer success, security and the C-suite, we empower teams with the time and efficiency to build the future.

### Our motto

Let's build the future!

### Our mission

To revolutionize operations and build customer trust by anticipating the unexpected in an unpredictable world.

### Our vision

An equitable world where we transform critical work so all teams can delight their customers and build trust.

## What is Backstage?

[Backstage](https://backstage.io/) is an open platform for building developer portals. Powered by a centralized software catalog, Backstage restores order to your microservices and infrastructure and enables your product teams to ship high-quality code quickly — without compromising autonomy.

Backstage unifies all your infrastructure tooling, services, and documentation to create a streamlined development environment from end to end.

## Why Backstage?

PagerDuty is a strong advocate of [full-service ownership](https://ownership.pagerduty.com/). Full-service ownership means that people take the responsibility for supporting the software they deliver, at every stage of its lifecycle. This level of ownership brings development teams much closer to their customers (internal or external), the business, and the value being delivered.

We believe that Backstage is a key component of that process by being the platform that allows teams to provide documentation to their customers, provide visibility on how their services are behaving and exposing the dependencies between different components and teams.

Backstage offers a single location where all the knowledge about services exists, and enables users to do it in a self-service way. This reduces interactions between teams, increasing their productivity and focus, allowing them to work on what is important to them.

## The plugin

The plugin was initially started by Backstage and then transitioned into PagerDuty. The plugin currently offers a PagerDuty card for:

- Displaying relevant PagerDuty information about an entity within Bacsktage, such as active incidents or recent changes.
- Quickly check who is on call for a service.
- Trigger an incident to the currently on call responder(s) for a service.

The scope for the plugin is quite limited at the time but we are working on bringing new features based on customer feedback. We welcome all feedback so if you have any [bugs](https://github.com/PagerDuty/backstage-plugin/issues/new?assignees=&labels=bug&projects=&template=bug_report.md&title=) to report or [feature requests](https://github.com/PagerDuty/backstage-plugin/issues/new?assignees=&labels=enhancement&projects=&template=feature_request.md&title=) to raise we **thank you**.

## Roadmap

We are working on some cool new features and capabilities. Here are some of them:

- [ ] Improving UI for the existing PagerDuty Card
- [ ] Adding support for adding multiple services on the same card
- [ ] Adding support for a Team PagerDuty card
- [ ] Create scaffolder action to allow services to be created in PagerDuty

For more details on what is in progress and what we are considering refer to the [project page](https://github.com/orgs/PagerDuty/projects/22) on GitHub.

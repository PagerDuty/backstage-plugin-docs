# Security

Security is a major concern for everyone. Although Backstage is typically deployed internally in organizations it is not immune to bad actors. Here we describe some of the design decisions that might impact security.

## Backstage Proxy

Any plugin that makes HTTP requests to external APIs needs to leverage one of **three options** provided by Backstage:

1. access the API directly
2. leverage a backend plugin to make the calls on-behalf of the frontend plugin
3. use Backstage proxy

!!! note
    If you want more details on these different options read the official documentation [here](https://backstage.io/docs/tutorials/using-backstage-proxy-within-plugin/).

In the current implementation, the PagerDuty plugin requires the `/pagerduty` proxy endpoint be exposed by the Backstage backend as an unprotected endpoint. This enables PagerDuty API access using the configured PagerDuty token for any user or process with access to `/pagerduty` Backstage backend endpoint.

If your regard this has problematic, consider running the plugin in `readOnly` mode and update your Backstage configuration in `app-config.yaml` to only allow `GET` methods like this.

```yaml
proxy:
  '/pagerduty':
    target: https://api.pagerduty.com
    headers:
      Authorization: Token token=${PAGERDUTY_TOKEN}
    allowedMethods: ['GET'] #(1)!
```

1. Prohibits the `/pagerduty` proxy endpoint from servicing non-GET requests

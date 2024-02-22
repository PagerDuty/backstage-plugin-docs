# Custom REST API URL

The default URL used for REST API requests is `https://api.pagerduty.com`. It is possible to override this URL through configuration. Add the following configuration to `app-config.yaml`:

```yaml
pagerDuty:
  apiBaseUrl: 'https://api.eu.pagerduty.com'
```

!!! note
    **PagerDuty accounts based in Europe use a different URL** so you need to override it here if that is your case.

    EU-based accounts: 'https://api.eu.pagerduty.com'
    US-based accounts: 'https://api.pagerduty.com'
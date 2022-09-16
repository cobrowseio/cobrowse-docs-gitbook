# L7 firewall configuration

This guide contains some information that can be useful when needing to secure your implementation behind a L7 firewall. There are two subsets of the API that are generally used by internal vs external roles.

{% hint style="info" %}
Paths are listed as **prefixes only** – all subpaths must be allowed by a firewall configuration.
{% endhint %}

### SDK required APIs

Here you can find the subset of the APIs that must be accessible to end user devices running our SDKs.

```bash
# Only a subset of the REST API is required for SDK use
/api/1/devices
/api/1/sessions

# And the WebSocket API.
# Firewalls must be configured to support WebSocket traffic on these routes 
/sockets/1/
```

#### Pinning the web SDK version

If you choose to [pin the web SDK](web-sdk-pinning.md) using the version that is shipped with your deployment, then you will also need to add that route.

```
/sdk-js/
```

### Agent-side required APIs&#x20;

These are the APIs required by the agent dashboard, or embedded agent side UI.

```bash
# For the agent side, the entire API is required
/api/1/
/proxy/1/
/recording/1/

# And the WebSocket API.
# Firewalls must be configured to support WebSocket traffic on these routes 
/sockets/1/
```

As well as the routes above, the HTML frontend must also be accessible for agents. Paths include.

```bash
# The frontend routes for HTML, CSS and JS
/index.html
/favicon.png
/apps/
/static/
```

**Important:** The frontend is built as a static single page application, with an `index.html` entrypoint. Any route not included in the prefixes listed above should be considered a frontend route and resolve to `/index.html`.

### Headers and Query Parameters

All headers, including custom headers, must be forwarded on the non-frontend routes. All query parameters must be forwarded on all routes.

{% hint style="warning" %}
**Warning:** We may add routes and parameters between versions. We always recommend  deploying new software versions to a staging environment and testing behind your firewall configuration before promoting new versions to production.
{% endhint %}

---
description: Javascript for sessions across multiple domains or sub-domains.
---

# Cross-domain session support

### Cross-domain sessions

In some cases, several of your domains or sub-domains need to be visited in a single session.

To enable this, add the following javascript snippet within our [web SDK](../../../sdk-installation/web.md) to all required pages.

{% hint style="info" %}
Trusted origins must be served over _HTTPS_ and include the full domain or sub-domain, with no additional url path or parameters. \
\
Cobrowse will add a query string parameter to any links to the trusted origins. Query string parameters should be preserved across redirects.&#x20;
{% endhint %}

```javascript
CobrowseIO.trustedOrigins = [
  'https://www.myexample.com',          // origin to trust
  'https://my-other-website.net',       // another origin to trust
  'https://intranet.myexample.com:8443' // an origin with a port to trust
];
CobrowseIO.start(); // before the start call
```

This provides Cobrowse for Web with a list of _trusted origins_ to allow your session to continue on. All trusted origins must be listed.

Example code: [https://github.com/cobrowseio/cobrowse-sdk-js-examples#cross-domain-sessions](https://github.com/cobrowseio/cobrowse-sdk-js-examples#cross-domain-sessions).

#### Sub-domains

If your sessions are across sub-domains of the same subdomain we recommend adding the top-level domain and any required subdomains. Glob matching is also supported here.

```javascript
CobrowseIO.trustedOrigins = [
  'https://myexample.com',         // top-level domain
  'https://*.myexample.com'        // glob-matching can be used to match all domains
  'https://login.myexample.com',   // or specific sub-domains can also be specified
  'https://support.myexample.com'
];
CobrowseIO.start(); // before the start call
```

When the top-level domain is configured as a trusted origin Cobrowse will use cookies support session continuation across sub-domains of that top-level domain.

### Supported browsers

* Google Chrome
* Mozilla Firefox
* Safari

Cross-domain support will fail in a browser's private or "incognito" mode.

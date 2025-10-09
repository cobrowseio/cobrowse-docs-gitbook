---
description: Javascript for sessions across multiple domains or sub-domains.
---

# Cross-domain session support

### Cross-domain sessions

In some cases, several of your domains or sub-domains need to be visited in a single session.

To enable this, add the following javascript snippet within our [web SDK](../../../sdk-installation/web.md) to all required pages.&#x20;

{% hint style="info" %}
Trusted origins must be served over _HTTPS_ and include the full domain or sub-domain, with no additional url path or parameters.
{% endhint %}

```javascript
CobrowseIO.trustedOrigins = [
  'https://myexample.com', // origin to trust
  'https://my-other-website.net', // another origin to trust
  'https://intranet.myexample.com:8443' // an origin with a port to trust
];
CobrowseIO.start(); // before the start call
```

This provides Cobrowse for Web with a list of _trusted origins_ to allow your session to continue on. All trusted origins must be listed.&#x20;

Example code: [https://github.com/cobrowseio/cobrowse-sdk-js-examples#cross-domain-sessions](https://github.com/cobrowseio/cobrowse-sdk-js-examples#cross-domain-sessions).

### Supported browsers

* Google Chrome
* Mozilla Firefox
* Safari

Cross-domain support will fail in a browser's private or "incognito" mode.

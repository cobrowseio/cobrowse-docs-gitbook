---
description: >-
  Web SDK pinning. For customers that prefer to pin the version of the SDK to
  one shipped and tested with their Cobrowse Enterprise deployment.
---

# Pin web SDK version

Our Web SDK receives regular updates, these are always published to the hosted version of SDK. Some customers may prefer to pin the version of the SDK to one shipped and tested with their Cobrowse Enterprise deployment for stability and reproducibility.

#### Pin the Web SDK version

The Web SDK is hosted on your deployment at `/sdk-js/CobrowseIO.js`

You can use our snippet found on the [Web SDK](../../sdk-installation/web.md#installation) installation page, but replace the `src` that is set to `https://js.cobrowse.io/CobrowseIO.js` with your own deployment URL. For example if your deployment is running at `https://cobrowse.example.com`, the SDK will be hosted at `https://cobrowse.example.com/sdk-js/CobrowseIO.js`.

{% hint style="info" %}
Ensure to subscribe to release notifications to keep up to date with critical fixes and security patches. This can be done on the Web SDK repo: [https://github.com/cobrowseio/cobrowse-sdk-js-release](https://github.com/cobrowseio/cobrowse-sdk-js-release)
{% endhint %}


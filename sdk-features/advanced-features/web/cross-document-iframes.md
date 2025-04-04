---
description: >-
  To use Cobrowse for Web with cross document IFrames you will need to add a
  javascript snippet to the page being embedded.
---

# IFrames support

### Cross-document IFrames

{% hint style="info" %}
_IFrames that are not cross document are supported out-of-box without adding the SDK to the embedded page. This guide is for supporting cross document IFrames._
{% endhint %}

To use Cobrowse for Web with **cross document** IFrames you will need to add our javascript snippet to the page being embedded. You will then need to add a list of trusted origins to the embedded page and the parent page:

```javascript
CobrowseIO.trustedOrigins = [
    'https://myexample.com', // parent origin to trust
    'https://my-other-website.net' // another parent origin to trust
];
CobrowseIO.start();
```

It is **not** necessary for the embedded page to contain the Cobrowse license key, if provided it will be ignored.

#### Code example

See our JS examples repo for the parent and child SDK configuration: [https://github.com/cobrowseio/cobrowse-sdk-js-examples#cross-document-iframes](https://github.com/cobrowseio/cobrowse-sdk-js-examples#cross-document-iframes).

### Redaction within cross-document IFrames

Elements within the cross-document iframe can also be redacted by using the URL of the iframe as the key to the array of Selectors to be redacted just for that iframe.

Full details can be seen at the link below.

{% content-ref url="../../redact-sensitive-data.md" %}
[redact-sensitive-data.md](../../redact-sensitive-data.md)
{% endcontent-ref %}

### Running only in an IFrame

A few customers want to run Cobrowse.io only within an IFrame, and not any containing or parent page. This is supported, but requires passing an extra configuration option when starting Cobrowse.&#x20;

```javascript
CobrowseIO.start({allowIFrameStart:true});
```


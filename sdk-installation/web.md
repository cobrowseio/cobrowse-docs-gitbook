---
description: Javascript SDK for Web
---

# Web

## Installation

The Cobrowse SDK for web is available either using a `<script>` tag via CDN, or can be included in your build via NPM.

{% tabs %}
{% tab title="Script" %}
Add this javascript snippet to the top of the `<head>` section of your website.

```markup
<script>
    (function(w,t,c,p,s,e){p=new Promise(function(r){w[c]={client:function(){if(!s){
    s=document.createElement(t);s.src='https://js.cobrowse.io/CobrowseIO.js';s.async=1;
    e=document.getElementsByTagName(t)[0];e.parentNode.insertBefore(s,e);s.onload=function()
    {r(w[c]);};}return p;}};});})(window,'script','CobrowseIO');

    CobrowseIO.license = "put your license key here";    
    CobrowseIO.client().then(function(){
        CobrowseIO.start();
    });
</script>
```
{% endtab %}

{% tab title="NPM" %}
First install the Cobrowse SDK for web via NPM:

```bash
> npm install cobrowse-sdk-js --save
```

Then you can initialise the SDK:

```javascript
import CobrowseIO from 'cobrowse-sdk-js'

CobrowseIO.license = 'your license key here'
CobrowseIO.start()
```
{% endtab %}
{% endtabs %}

### Add your license key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your website with your Cobrowse.io account.

## Try it out

Once you have your Javascript snippet and license key set up, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

**Note: Cobrowse needs to be able to access your website, so localhost addresses are not recommenced for testing. To test on a local development version, we recommend using** [**ngrok**](https://ngrok.com)**.**

## Minimum browser requirements

Cobrowse.io for Web supports all major browsers including Chrome, Firefox, Safari, Opera, Edge, and IE11.&#x20;

Our Web SDK has an optional "full-device" mode which enables the user to share their entire desktop without any installation. Due to browser limitations, this feature is not available on IE11, or in the mobile browsers such as Mobile Chrome and Mobile Safari. Please see our native [Android](android.md) and [iOS](ios.md) SDKs for [full device capabilities](../sdk-features/full-device-capabilities.md) on mobile.&#x20;

The specific browser versions supported are Chrome 16+, Firefox 11+, Safari 7+, Opera 12.1+, Edge 12+, and IE11.&#x20;

## **Advanced configuration**

{% content-ref url="../sdk-features/advanced-features/web/cross-document-iframes.md" %}
[cross-document-iframes.md](../sdk-features/advanced-features/web/cross-document-iframes.md)
{% endcontent-ref %}

{% content-ref url="../sdk-features/advanced-features/web/ie-11-polyfills.md" %}
[ie-11-polyfills.md](../sdk-features/advanced-features/web/ie-11-polyfills.md)
{% endcontent-ref %}



{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

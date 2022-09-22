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

**Note: Cobrowse needs to be able to access your website, so localhost addresses are not recommenced for testing. To test on a local development version, we recommend using** [**ngrok**](https://ngrok.com/)**.**

## Minimum browser requirements

Cobrowse.io for Web supports all major browsers including Chrome, Firefox, Safari, Opera, Edge, and IE11.&#x20;

Our Web SDK has an optional "full-device" mode which enables the user to share their entire desktop without any installation. Due to browser limitations, this feature is not available on IE11, or in the mobile browsers such as Mobile Chrome and Mobile Safari. Please see our native [Android](android.md) and [iOS](ios.md) SDKs for [full device capabilities](../sdk-features/full-device-capabilities/) on mobile.&#x20;

The specific browser versions supported are Chrome 16+, Firefox 11+, Safari 7+, Opera 12.1+, Edge 12+, and IE11.&#x20;

## **IFrames support**

{% content-ref url="../sdk-features/advanced-features/web/cross-document-iframes.md" %}
[cross-document-iframes.md](../sdk-features/advanced-features/web/cross-document-iframes.md)
{% endcontent-ref %}

## IE 11 support

{% content-ref url="../sdk-features/advanced-features/web/ie-11-polyfills.md" %}
[ie-11-polyfills.md](../sdk-features/advanced-features/web/ie-11-polyfills.md)
{% endcontent-ref %}

## Cross-domain support

{% content-ref url="../sdk-features/advanced-features/web/cross-domain-session-support.md" %}
[cross-domain-session-support.md](../sdk-features/advanced-features/web/cross-domain-session-support.md)
{% endcontent-ref %}

## Something not working?

Please use the following steps to troubleshoot issues on web:

* check that all steps of these docs and requirements have been followed
* check whether the issues persist when using [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) directly
* inspect the developer console on the agent and client-side to see any error messages
* check the below sections to see if they match the error messages you can see
* send a description, screenshots, and optionally a video of the above to [hello@cobrowse.io](mailto:hello@cobrowse.io)!

## Non-public resources (e.g. CSS)

Certain resources (e.g. CSS) may not be exposed to the public internet (e.g. pre-production environments such as UAT) and so these elements of your webpage will not appear to agents.&#x20;

You will see error messages in the developer console relating to `https://cobrowse.io/proxy/1/assets/<hex encoded asset>` . You can check which assets are encoded by pasting the hex code into [https://cryptii.com/pipes/hex-decoder](https://cryptii.com/pipes/hex-decoder).

Make sure these assets are accessible to our server by making them public, or by whitelisting requests from `*.cobrowse.io`!

{% hint style="warning" %}
If you self-host your instance, then requests from`*.<your instance domain>` should be whitelisted instead of `*.cobrowse.io`.
{% endhint %}

## Content Security Policies **(CSPs)**

If you have CSPs on your website then they may block the functionality of Cobrowse.io. When a CSP is blocking Cobrowse.io, then there will be an error in the javascript console stating:

`Refused to connect to https://cobrowse.io/... because it violates the document's Content Security Policy.`

To solve this, you will need to enable the following in your CSP:

connect src of: **`connect-src cobrowse.io *.cobrowse.io wss://*.cobrowse.io;`**

{% hint style="warning" %}
If you self-host your instance, then replace`cobrowse.io` with your `<your instance domain>` in each case.
{% endhint %}

script-src of: **`script-src 'unsafe-inline' js.cobrowse.io;`**

{% hint style="warning" %}
If you use the self-hosted version of the web SDK instead of `js.cobrowse.io`, e.g. [following this](../enterprise-self-hosting/advanced/web-sdk-pinning.md), then you should use that full URL instead, e.g. `https://<your instance domain>/sdk-js/CobrowseIO.js`.
{% endhint %}

You should be able to replace the unsafe-inline with the hash of your snippet if you wish. This will be available in the javascript console.

## Incognito/private browser windows

If the same user has an incognito window (or multiple) running at the same time as their usual browser window, then the incognito windows will be separate devices in the device listing.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

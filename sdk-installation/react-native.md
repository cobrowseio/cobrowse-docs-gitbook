---
description: React Native SDK
---

# React Native

## Installation

```bash
npm install --save cobrowse-sdk-react-native
react-native link
```

**Note:** For `react-native link` to work out of the box on iOS, you need to be using Pods to manage dependencies. Please also remember to run `pod install` after the link step. Depending on your project, you may also need `use_frameworks!` in your Podfile. If using Pods causes a problem for your project, you may use Carthage to manage your Cobrowse.io dependencies, or you may also add the frameworks to your project by hand. More info at [iOS SDK Installation](ios.md).&#x20;

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

```javascript
import CobrowseIO from 'cobrowse-sdk-react-native';

CobrowseIO.license = "put your license key here";
CobrowseIO.start();
```

## Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* iOS 9.0, Android API 19 or above.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

## **Firewalls**

If your agents work behind a firewall (e.g. a corporate firewall), then the **agent-side** API routes will need to be whitelisted as specified here: [https://docs.cobrowse.io/enterprise-self-hosting/advanced/firewalls#agent-side-required-apis](https://docs.cobrowse.io/enterprise-self-hosting/advanced/firewalls#agent-side-required-apis).

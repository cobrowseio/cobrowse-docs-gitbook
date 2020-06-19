---
description: React Native SDK
---

# React Native

## Installation

```bash
npm install --save cobrowse-sdk-react-native
react-native link
```

**Note:** For iOS you need to be using Pods to manage dependencies for `react-native link` to work out of the box \(and also remember to run `pod install` after the link step\).

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

```javascript
import CobrowseIO from 'cobrowse-sdk-react-native';

CobrowseIO.license = "<your license key here>";
CobrowseIO.start();
```

## Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Full device capabilities <a id="full-device-capabilities"></a>

Cobrowse.io optionally supports full device capabilities on Android and iOS.

{% page-ref page="../sdk-features/advanced-features/react-native/full-device-capabilities.md" %}

## Requirements

* iOS 9.0, Android API 19 or above.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}


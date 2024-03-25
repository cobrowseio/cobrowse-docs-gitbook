---
description: React Native SDK
---

# React Native

## Installation

```bash
npm install --save cobrowse-sdk-react-native
(cd ios && pod install)
```

**Note:** For older versions of React Native, you may need to run `react-native link` before running `pod install`. Depending on your project, you may also need `use_frameworks!` in your Podfile. If using Pods causes a problem for your project, you may use Carthage to manage your Cobrowse.io dependencies, or you may also add the frameworks to your project by hand. More info at [iOS SDK Installation](ios.md).

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

* iOS 11.0, Android API 19 or above.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

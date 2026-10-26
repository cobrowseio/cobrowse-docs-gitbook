---
description: React Native SDK
---

# React Native

## Installation

```bash
npm install --save cobrowse-sdk-react-native
(cd ios && npx react-native spm)
```

On iOS, the SDK is linked with Swift Package Manager (SPM) through React Native's SPM integration, which requires React Native 0.87 or later.

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

* iOS 15.0, Android API 23 or above.
* React Native 0.87 or later for version 4.0 and above.

## Stay up to date

There are a few ways to find out about new releases of the React Native SDK:

* Run `npm outdated cobrowse-sdk-react-native` to check for a newer version.
* Enable Dependabot or Renovate version updates on your repository to get a pull request for each new version.
* Subscribe to [cobrowse-sdk-react-native on libraries.io](https://libraries.io/npm/cobrowse-sdk-react-native) to get an email for each new version.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

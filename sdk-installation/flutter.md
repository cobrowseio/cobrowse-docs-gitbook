---
description: Flutter iOS and Android SDK
---

# Flutter

## Installation

We recommend installing the Cobrowse.io SDK using [pub.dev](https://pub.dev/packages/cobrowse_sdk_flutter):

[![Pub Version](https://img.shields.io/pub/v/cobrowse_sdk_flutter?color=blueviolet)](https://pub.dev/packages/cobrowse_sdk_flutter)

Add the following package to your `pubspec.yaml`:

```yaml
...
dependencies:
  ...
  cobrowse_sdk_flutter: ^1.0.0
...
```

Add the following lines to your code which will register this device with the Cobrowse servers so you can connect to it. You could choose to do this on app startup, or when your users visits a support page in your application, or any other time.

```dart
import 'package:cobrowse_sdk_flutter/cobrowseio.dart';

CobrowseIO.instance.setLicense('put your license key here');
CobrowseIO.instance.start();
```

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

### Flutter sample app

We provide [a sample application](https://github.com/cobrowseio/cobrowse-sdk-flutter/tree/master/example) showing how to use the Cobrowse.io SDK in a Flutter app.

## Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* iOS 11.0 or later
* Android API version 24 (7.0 Nougat) or later

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

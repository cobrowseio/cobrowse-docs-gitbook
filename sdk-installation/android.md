---
description: Android Native SDK, with support for Compose and native webviews
---

# Android

## Installation

{% code title="build.gradle" %}
```java
dependencies {
    // ... other dependencies ...
    implementation 'io.cobrowse:cobrowse-sdk-android:3.+'
}
```
{% endcode %}

Add the following lines to your code which will register this device with the Cobrowse servers so you can connect to it. You could choose to do this on app startup, or when your users visits a support page in your application, or any other time.

```java
import io.cobrowse.CobrowseIO;
...

CobrowseIO.instance().license("put your license key here");
CobrowseIO.instance().start();
```

You may also start Cobrowse in your MainActivity or other Activity if necessary. In that case, the SDK will continue to function even as new Activities are being created and destroyed.

### Add your license key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse.io account.

## Try it out

Once you have your app running in the Android emulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* API version 19 (4.4 KitKat) or later

{% hint style="warning" %}
Support for Android 4.4 and Android 5.0 will be ended in upcoming versions
{% endhint %}

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

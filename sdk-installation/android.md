---
description: Android Native SDK, with support for Compose and native webviews
---

# Android

## Installation

{% code title="build.gradle" %}
```java
dependencies {
    // ... other dependencies ...
    implementation 'io.cobrowse:cobrowse-sdk-android:2.+'
}
```
{% endcode %}

Add the following lines to your code which will register this device with the Cobrowse servers so you can connect to it. You could choose to do this on app startup, or when your users visits a support page in your application, or any other time.

```java
import io.cobrowse.CobrowseIO;
...

CobrowseIO.instance().license("put your license key here");
CobrowseIO.instance().start(this);
```

#### Registering on startup

If you would like your devices to register as soon as your app starts, we recommend doing this in your custom Application subclass `onCreate` method. For example:

```java
package com.example;

import android.app.Application;
import io.cobrowse.CobrowseIO;

public class MainApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        CobrowseIO.instance().license("put your license key here");
        CobrowseIO.instance().start(this);
    }
}
```

If you do not have a custom Application subclass in your app yet, you will also need to modify your application manifest:

{% code title="AndroidManifest.xml" %}
```xml
<application
    android:name=".MainApplication"
    <!-- ... -->
</application>
```
{% endcode %}

You may also start Cobrowse in your MainActivity or other Activity if necessary. In that case, the SDK will continue to function even as new Activities are being created and destroyed.

### Add your license key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse.io account.

## Try it out

Once you have your app running in the Android emulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* API version 19 (4.4 KitKat) or later

{% hint style="warning" %}
Due to OkHttp dropping support for Android 4.4 after its v3.12.x releases on December 31, 2021, support for Android 4.4 will likely be supported in 2.x releases of the Cobrowse.io Android SDK only.&#x20;
{% endhint %}

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

## Flutter Support

There is a community-built Flutter plugin: [https://github.com/robodigital/cobrowseio\_flutter](https://github.com/robodigital/cobrowseio\_flutter).

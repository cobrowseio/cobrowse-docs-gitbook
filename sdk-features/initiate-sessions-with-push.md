---
description: For use with Android, iOS, React Native, Flutter, and .NET Mobile SDKs only.
---

# Initiate sessions with push

By default, Cobrowse Native SDKs for Android and iOS open a socket to handle incoming session requests.

You may optionally send new session requests over the native push channel. Cobrowse supports this configuration out of box using Firebase Cloud Messaging (FCM).

{% hint style="info" %}
For sessions to start by push the user needs to have the client application open in the foreground. This method will not send a visible push notification to the user.

If you wish to use visible push, this is possible using your own existing push channel and can be fully customized.
{% endhint %}

Setup your Firebase account:

1. Create an account for [Firebase](https://firebase.google.com/).
2. Create a new project from your [Firebase console](https://console.firebase.google.com/).
3. In your Project settings, add entries for your Android and iOS apps.
4. For iOS, you will need generate an APNs Authentication Key (recommended) or APNs Certificate from [https://developer.apple.com](https://developer.apple.com/). You may then upload it under Project Settings -> Cloud Messaging.
5. Please generate a Firebase Private Key file from Project Settings -> Service Accounts, download it and enter its contents into your [Firebase settings](https://cobrowse.io/dashboard/settings/firebase).

A few changes to native code as described below.

{% tabs %}
{% tab title="iOS" %}
You first need to `registerForRemoteNotifications()` at a location that is right for your application.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    application.registerForRemoteNotifications()
}
```

Once requested the `didRegisterForRemoteNotificationsWithDeviceToken` delegate method will be called. You must then pass this token to Cobrowse using the `CobrowseIO.setDeviceToken` method.

This device will now be associated with the push token within the Cobrowse dashboard.

```swift
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    CobrowseIO.setDeviceToken(deviceToken)
}
```
{% endtab %}

{% tab title="Android" %}
You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/android/client](https://firebase.google.com/docs/cloud-messaging/android/client).

FCM identifies devices by their Firebase installation ID. Enable it in your `AndroidManifest.xml`:

```xml
<application>
    <meta-data
        android:name="firebase_messaging_installation_id_enabled"
        android:value="true" />
</application>
```

Register with FCM every time your app starts, for example in `Application.onCreate()`:

```kotlin
FirebaseMessaging.getInstance().register()
```

Then pass the installation ID and push notifications from FCM to the Cobrowse.io SDK, for example:

```kotlin
package com.example

import com.google.firebase.messaging.FirebaseMessagingService
import com.google.firebase.messaging.RemoteMessage

import io.cobrowse.CobrowseIO

class MyFirebaseMessagingService : FirebaseMessagingService() {

    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        CobrowseIO.instance().onPushNotification(remoteMessage.data)
    }

    override fun onRegistered(installationId: String) {
        CobrowseIO.instance().setDeviceToken(installationId)
    }
}
```
{% endtab %}

{% tab title="React Native" %}
You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/android/client](https://firebase.google.com/docs/cloud-messaging/android/client).

FCM identifies devices by their Firebase installation ID. Enable it in your `firebase.json`:

```json
{
  "react-native": {
    "messaging_installation_id_enabled": true
  }
}
```

Next, on Android, register with FCM every time your app starts and pass the installation ID to the Cobrowse.io SDK, for example:

```javascript
import { Platform } from 'react-native';
import { getMessaging, onRegistered, register } from '@react-native-firebase/messaging';
import CobrowseIO from 'cobrowse-sdk-react-native';

if (Platform.OS === 'android') {
    onRegistered(getMessaging(), (installationId) => {
        CobrowseIO.deviceToken = installationId;
    });
    register(getMessaging());
}
```
{% endtab %}

{% tab title="Flutter" %}
You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/android/client](https://firebase.google.com/docs/cloud-messaging/android/client).

FCM identifies devices by their Firebase installation ID. Enable it in your `android/app/src/main/AndroidManifest.xml`:

```xml
<application>
    <meta-data
        android:name="firebase_messaging_installation_id_enabled"
        android:value="true" />
</application>
```

Next, register with FCM every time your app starts and pass the installation ID to the Cobrowse.io SDK, for example:

```dart
import 'package:cobrowse_sdk_flutter/cobrowseio.dart';
import 'package:firebase_messaging/firebase_messaging.dart';

if (Platform.isAndroid) {
    // Only required on Android, iOS does this automatically
    FirebaseMessaging.instance.onRegistered.listen((installationId) {
        CobrowseIO.instance.setDeviceToken(installationId);
    });
    FirebaseMessaging.instance.register();
}
```
{% endtab %}

{% tab title=".NET Mobile" %}
**.NET iOS implementation**

If you are already using push notifications in your app, there is nothing further required on the native side.

If you are not already using push notifications in your app, please enable them under Capabilities in the iOS app project and request push permission from the user whenever is appropriate:

```csharp
[Export("applicationDidBecomeActive:")]
public void OnActivated(UIApplication application)
{
    application.RegisterUserNotificationSettings(
        UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Badge | UIUserNotificationType.Sound | UIUserNotificationType.Alert,
            categories: null));
    application.RegisterForRemoteNotifications();
}
```

**.NET Android implementation**

You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://docs.microsoft.com/en-us/xamarin/android/data-cloud/google-messaging/firebase-cloud-messaging](https://docs.microsoft.com/en-us/xamarin/android/data-cloud/google-messaging/firebase-cloud-messaging).

FCM identifies devices by their Firebase installation ID. Enable it in your `AndroidManifest.xml`:

```xml
<application>
    <meta-data
        android:name="firebase_messaging_installation_id_enabled"
        android:value="true" />
</application>
```

Register with FCM every time your app starts, for example in `MainApplication.OnCreate()`:

```csharp
Firebase.Messaging.FirebaseMessaging.Instance.Register();
```

Then pass the installation ID and push notifications from FCM to the Cobrowse.io SDK, for example:

```csharp
using System;
using Android.App;
using Android.Runtime;
using Firebase.Messaging;
using Cobrowse.IO.Android;

namespace YourAppNamespace
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        public MyFirebaseMessagingService()
        {
        }

        protected MyFirebaseMessagingService(IntPtr javaReference, JniHandleOwnership transfer) : base(javaReference, transfer)
        {
        }

        public override void OnMessageReceived(RemoteMessage remoteMessage)
        {
            CobrowseIO.Instance.OnPushNotification(remoteMessage.Data);
        }

        public override void OnRegistered(string installationId)
        {
            CobrowseIO.Instance.SetDeviceToken(Application, installationId);
        }
    }
}
```
{% endtab %}
{% endtabs %}

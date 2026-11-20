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
You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/ios/client](https://firebase.google.com/docs/cloud-messaging/ios/client).

FCM identifies devices by their Firebase installation ID. Enable it in your `Info.plist`:

```xml
<key>FirebaseMessagingInstallationIdEnabled</key>
<true/>
```

You then need to `registerForRemoteNotifications()` at a location that is right for your application. Firebase maps the APNs token to the installation ID for you, unless you have disabled its method swizzling, in which case pass the token to `Messaging.messaging().apnsToken` yourself.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    FirebaseApp.configure()
    Messaging.messaging().delegate = self
    application.registerForRemoteNotifications()
    return true
}
```

Firebase Messaging registers with FCM every time your app starts and hands the installation ID to its delegate. Pass it and push notifications from FCM to the Cobrowse.io SDK, for example:

```swift
extension AppDelegate: MessagingDelegate {
    func messaging(_ messaging: Messaging, didReceiveRegistration installationId: String?) {
        guard let installationId else { return }
        CobrowseIO.setDeviceToken(.firebaseInstallationId(installationId))
    }
}

func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) async -> UIBackgroundFetchResult {
    CobrowseIO.onPushNotification(userInfo)
    return .noData
}
```

This device will now be associated with the push token within the Cobrowse dashboard.
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
import io.cobrowse.PushToken

class MyFirebaseMessagingService : FirebaseMessagingService() {

    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        CobrowseIO.instance().onPushNotification(remoteMessage.data)
    }

    override fun onRegistered(installationId: String) {
        CobrowseIO.instance().setDeviceToken(PushToken.firebaseInstallationId(installationId))
    }
}
```
{% endtab %}

{% tab title="React Native" %}
You must first add Firebase Cloud Messaging (FCM) to your app with [React Native Firebase](https://rnfirebase.io/messaging/usage).

FCM identifies devices by their Firebase installation ID. Enable it in your `firebase.json`:

```json
{
  "react-native": {
    "messaging_installation_id_enabled": true
  }
}
```

Register with FCM every time your app starts, then pass the installation ID and push notifications from FCM to the Cobrowse.io SDK, for example:

```javascript
import { getMessaging, onMessage, onRegistered, register } from '@react-native-firebase/messaging';
import CobrowseIO, { PushToken } from 'cobrowse-sdk-react-native';

onRegistered(getMessaging(), (installationId) => {
  CobrowseIO.deviceToken = PushToken.firebaseInstallationId(installationId);
});
onMessage(getMessaging(), (message) => {
  CobrowseIO.onPushNotification(message.data);
});
register(getMessaging());
```
{% endtab %}

{% tab title="Flutter" %}
You must first add Firebase Cloud Messaging (FCM) to your app with [FlutterFire](https://firebase.flutter.dev/docs/messaging/overview).

FCM identifies devices by their Firebase installation ID. Enable it in your `android/app/src/main/AndroidManifest.xml`:

```xml
<application>
    <meta-data
        android:name="firebase_messaging_installation_id_enabled"
        android:value="true" />
</application>
```

and in your `ios/Runner/Info.plist`:

```xml
<key>FirebaseMessagingInstallationIdEnabled</key>
<true/>
```

Register with FCM every time your app starts, then pass the installation ID and push notifications from FCM to the Cobrowse.io SDK, for example:

```dart
import 'package:cobrowse_sdk_flutter/cobrowseio.dart';
import 'package:firebase_messaging/firebase_messaging.dart';

FirebaseMessaging.instance.onRegistered.listen((installationId) {
  CobrowseIO.instance.setPushToken(PushToken.firebaseInstallationId(installationId));
});
FirebaseMessaging.onMessage.listen((message) {
  CobrowseIO.instance.onPushNotification(message.data.cast<String, String>());
});
FirebaseMessaging.instance.register();
```
{% endtab %}

{% tab title=".NET Mobile" %}
**.NET iOS implementation**

You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/ios/client](https://firebase.google.com/docs/cloud-messaging/ios/client).

FCM identifies devices by their Firebase installation ID. Enable it in your `Info.plist`:

```xml
<key>FirebaseMessagingInstallationIdEnabled</key>
<true/>
```

Register for remote notifications, then pass the installation ID from your Firebase Messaging delegate and push notifications from your `AppDelegate` to the Cobrowse.io SDK, for example:

```csharp
using Cobrowse.IO.iOS;

public void DidReceiveRegistration(Messaging messaging, string installationId)
{
    CobrowseIO.SetPushToken(PushToken.FirebaseInstallationId(installationId));
}

public override void DidReceiveRemoteNotification(UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    CobrowseIO.OnPushNotification(userInfo);
    completionHandler(UIBackgroundFetchResult.NoData);
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
            CobrowseIO.Instance.SetDeviceToken(PushToken.FirebaseInstallationId(installationId));
        }
    }
}
```

The cross-platform `Cobrowse.IO.CobrowseIO.Instance` offers the same `SetDeviceToken` and `OnPushNotification` if you prefer to call them from shared code.
{% endtab %}
{% endtabs %}

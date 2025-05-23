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
You first need to `registerForRemoteNotificataions()`at a location that is right for your application.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    application.registerForRemoteNotifications()
}
```

Once requested the `didRegisterForRemoteNotificationsWithDeviceToken`delegate method will be called. You must then pass this token to Cobrowse using the `CobrowseIO.setDeviceToken` method.

This device will now be associated associate with the push token winth the Cobrowse dashboard.

```swift
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    CobrowseIO.setDeviceToken(deviceToken)
}
```
{% endtab %}

{% tab title="Android" %}
You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/android/client](https://firebase.google.com/docs/cloud-messaging/android/client).

Next, whenever your device receives a registration token or push notification from FCM, pass that to the Cobrowse.io SDK, for example:

```java
package com.example;

import com.google.firebase.messaging.FirebaseMessagingService;
import com.google.firebase.messaging.RemoteMessage;

import io.cobrowse.CobrowseIO;

public class FirebaseMessaging extends FirebaseMessagingService {

    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        CobrowseIO.instance().onPushNotification(remoteMessage.getData());
    }

    @Override
    public void onNewToken(String token) {
        CobrowseIO.instance().setDeviceToken(getApplication(), token);
    }

}
```
{% endtab %}

{% tab title="React Native" %}
You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/android/client](https://firebase.google.com/docs/cloud-messaging/android/client).

Next, whenever your device receives a registration token from FCM, pass that to the Cobrowse.io SDK, for example:

```javascript
import CobrowseIO from 'cobrowse-sdk-react-native';

CobrowseIO.deviceToken = "<your FCM token>";
```
{% endtab %}

{% tab title="Flutter" %}
You must first add Firebase Cloud Messaging (FCM) to your app. Please see FCM documentation at [https://firebase.google.com/docs/cloud-messaging/android/client](https://firebase.google.com/docs/cloud-messaging/android/client).

Next, whenever your device receives a registration token from FCM, pass that to the Cobrowse.io SDK, for example:

```dart
import 'package:cobrowse_sdk_flutter/cobrowseio.dart';

if (Platform.isAndroid) {
    // Only required on Android, iOS does this automatically
    CobrowseIO.instance.setDeviceToken('<your FCM token>');
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

Next, whenever your device receives a registration token or push notification from FCM, pass that to the Cobrowse.io SDK, for example:

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
    public class FirebaseMessaging : FirebaseMessagingService
    {
        public FirebaseMessaging()
        {
        }

        protected FirebaseMessaging(IntPtr javaReference, JniHandleOwnership transfer) : base(javaReference, transfer)
        {
        }

        public override void OnMessageReceived(RemoteMessage remoteMessage)
        {
            CobrowseIO.Instance.OnPushNotification(remoteMessage.Data);
        }

        public override void OnNewToken(string token)
        {
            CobrowseIO.Instance.SetDeviceToken(Application, token);
        }
    }
}
```
{% endtab %}
{% endtabs %}

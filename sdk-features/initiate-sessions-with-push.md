---
description: 'For use with Android, iOS, React Native, and Xamarin SDKs only.'
---

# Initiate sessions with push

By default, Cobrowse Native SDKs for Android and iOS open a socket to handle incoming session requests.

You may optionally send new session requests over the native push channel. Cobrowse supports this configuration out of box using Firebase Cloud Messaging \(FCM\).

Setup your Firebase account:

1. Create an account for [Firebase](https://firebase.google.com/).
2. Create a new project from your [Firebase console](https://console.firebase.google.com/).
3. In your Project settings, add entries for your Android and iOS apps.
4. For iOS, you will need generate an APNs Authentication Key \(recommended\) or APNs Certificate from [https://developer.apple.com](https://developer.apple.com/). You may then upload it under Project Settings -&gt; Cloud Messaging.
5. Please generate a Firebase Server Key from Project Settings -&gt; Cloud Messaging and enter it into your [Firebase settings](https://cobrowse.io/dashboard/settings/firebase).

A few changes to native code as described below.




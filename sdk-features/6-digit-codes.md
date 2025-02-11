---
description: >-
  Cobrowse does not require any visible UI presented to the user but there is
  the option to present a UI that generates a 6-digit code. Learn more.
---

# Use 6-digit codes

By default, Cobrowse does not require any visible UI presented to the user. It will exist in the background of your app, and only activate when agents initiate a new session.

You may optionally present a UI in your app that enables users to generate 6-digit codes. Users may then read a code over the phone or in chat, and agents can use the 6-digit code to initiate the Cobrowse session.

{% hint style="info" %}
**Important**: 6-digit codes expire after approximately 20 minutes, so it's best practice to generate a code only when a user wants to start a session.
{% endhint %}

To generate a 6-digit code in your integration you can use the following APIs. Once you have generated the code you can display it to the user in your own UI. You should only generate a code when a user needs it as they expire shortly after creation.

{% tabs %}
{% tab title="Web" %}
```javascript
// ensure Cobrowse is loaded
CobrowseIO.client().then(function() {
    // create a code a display it to the user using your own UI
    // ONLY GENERATE CODE WHEN NEEDED. DO NOT GENERATE CODE ON PAGE LOAD.
    CobrowseIO.createSessionCode().then(function(code) {
       console.log('your code is', code);
    });
});
```
{% endtab %}

{% tab title="iOS / MacOS" %}
**Swift**

```swift
CobrowseIO.instance().createSession { error, session in
    guard error == nil, let code = session?.code()
        else { return }
    
    print(code)
}
```

**Objective-C**

```objectivec
[CobrowseIO.instance createSession:^(NSError * err, CBIOSession * session) {
    if (err) NSLog(@"Failed to create code")
    else NSLog(@"%@", session.code);
}];
```


{% endtab %}

{% tab title="Android" %}
```java
CobrowseIO.instance().createSession((err, session) -> {
    if (err != null) Log.w("App", "Failed to create code");
    else if (session != null) Log.i("App", "Session code " + session.code());
});
```
{% endtab %}

{% tab title="React Native" %}
```javascript
const session = await CobrowseIO.createSession()
console.log('Your 6 digit code is', session.code)
```
{% endtab %}

{% tab title="Flutter" %}
```dart
Session session = await CobrowseIO.instance.createSession();
log('Your session code: ${session.code}');
```
{% endtab %}

{% tab title=".NET Mobile" %}
```csharp
CobrowseIO.Instance.CreateSession((Exception err, ISession session) => {
    if (err != null) Debug.WriteLine("Failed to create a code");
    else if (session != null) Debug.WriteLine("Session code " + session.Code);
});
```
{% endtab %}

{% tab title="Windows" %}
```csharp
Session session = await CobrowseIO.Instance.CreateSession();
Console.WriteLine("Code: {0}", session.Code);
```
{% endtab %}
{% endtabs %}

If you are **only** using 6 digit codes to start Cobrowse sessions in your implementation, you can prevent the SDK registering in your Cobrowse account until a screen sharing session is required. See the documentation on preventing automatic registration.

{% content-ref url="advanced-features/starting-and-stopping-the-sdk.md" %}
[starting-and-stopping-the-sdk.md](advanced-features/starting-and-stopping-the-sdk.md)
{% endcontent-ref %}

You can monitor changes in the state of the session using the Cobrowse delegate methods to listen for events:

{% content-ref url="listening-for-events.md" %}
[listening-for-events.md](listening-for-events.md)
{% endcontent-ref %}

To see some code samples for how to build a custom UI for your platform see:

{% content-ref url="customize-the-interface/customize-6-digit-code-screen.md" %}
[customize-6-digit-code-screen.md](customize-the-interface/customize-6-digit-code-screen.md)
{% endcontent-ref %}

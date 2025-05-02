---
description: >-
  Learn how to promote a session to full device mode using the SDK, as per your
  requirements.
---

# Managing full device mode

### Managing full device state from the SDKs

In some situations it is useful to be able to switch a Cobrowse session into full device mode using the SDK. For example, if you would like sessions to default to full device mode, or enforce full device mode is always (or never) used. We provide an API for doing this:

{% tabs %}
{% tab title="Web" %}
```javascript
await session.setFullDevice('requested')
```
{% endtab %}

{% tab title="iOS / MacOS" %}
**Swift**

```swift
session.setFullDevice(.requested)
```

**Objective-C**

```objectivec
[session setFullDevice: CBIOFullDeviceStateRequested callback:nil];
```
{% endtab %}

{% tab title="Android" %}
```java
session.setFullDevice(Session.FullDeviceState.Requested, (err, arg) -> {
    // handle error
});
```
{% endtab %}

{% tab title="React Native" %}
```javascript
await session.setFullDevice(true)
```
{% endtab %}

{% tab title="Flutter" %}
```dart
Session? session = await CobrowseIO.instance.currentSession();
if (session != null) {
    try {
        await session.setFullDevice(FullDeviceState.on);
    } on PlatformException catch (e) {
        // E.g. a network error
        log('Cannot update the full device state: ${e.message}');
    }
}
```
{% endtab %}

{% tab title=".NET Mobile" %}
```cs
Session session = CobrowseIO.Instance.CurrentSession;
if (session != null)
{
    session.SetFullDevice(FullDeviceState.On, (err, session) =>
    {
        if (err != null)
        {
            // E.g. a network error
        }
        else
        {
            // Full device mode is activated
        }
    });
}
```
{% endtab %}
{% endtabs %}

See this in action: [https://github.com/cobrowseio/cobrowse-sdk-js-examples#full-device-mode-by-default](https://github.com/cobrowseio/cobrowse-sdk-js-examples#full-device-mode-by-default).

These APIs can be used in combination with our various [delegate APIs](../listening-for-events.md) to switch the session in and out of full device mode as your use case requires.

For example, to request full device is used by default, you can set the full device state when the session is first loaded by the SDK:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.on('session.loaded', session => session.setFullDevice('requested')) 
```
{% endtab %}

{% tab title="iOS / MacOS" %}
**Swift**

```swift
// You must have implmented the CobrowseIODelegate
func cobrowseSessionDidLoad(_ session: CBIOSession) {
    session.setFullDevice(.requested)
}
```

**Objective-C**

```objectivec
// You must have implmented the CobrowseIODelegate
- (void)cobrowseSessionDidLoad:(CBIOSession *)session {
    [session setFullDevice: CBIOFullDeviceStateRequested callback:nil];
}
```
{% endtab %}

{% tab title="Android" %}
```java
// note: you must have implmented CobrowseIO.SessionLoadDelegate
@Override
public void sessionDidLoad(@NonNull Session session) {
    session.setFullDevice(Session.FullDeviceState.Requested, null);
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.addListener('session.loaded', session => {
    console.log('A session was loaded', session)
    await session.setFullDevice(true)
})
```
{% endtab %}

{% tab title="Flutter" %}
```dart
CobrowseIO.instance.sessionDidLoad.listen((session) {
    try {
        await session.setFullDevice(FullDeviceState.on);
    } on PlatformException catch (e) {
        // E.g. a network error
        log('Cannot update the full device state: ${e.message}');
    }
});
```
{% endtab %}

{% tab title=".NET Mobile" %}
```dart
CobrowseIO.Instance.SessionDidLoad += (sender, session) =>
{
    session.SetFullDevice(FullDeviceState.On, (err, session) =>
    {
        if (err != null)
        {
            // E.g. a network error
        }
        else
        {
            // Full device mode is activated
        }
    });
};
```
{% endtab %}
{% endtabs %}

### Controlling full device state from the Agent SDK

You may also set the full device state using the Agent SDK. See the Agent SDK [API Reference](../../agent-side-integrations/agent-sdk/api-reference.md) for more details.

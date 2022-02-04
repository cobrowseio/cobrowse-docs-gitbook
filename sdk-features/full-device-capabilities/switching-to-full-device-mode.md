# Switching to full device mode

### Controlling full device state from the agent UI

To switch to full device mode, the easiest way is to use the agent UI provided when in a Cobrowse session.&#x20;

![Toggle the "Full Device" switch to request access.](<../../.gitbook/assets/Screenshot 2022-02-03 at 10.41.47.png>)

### Controlling full device state from the SDKs

In some situations it is useful to be able to switch a Cobrowse session into full device mode using the SDK. For example, if you would like sessions to default to full device mode, or enforce full device mode is always (or never) used. We provide an API for doing this:

{% tabs %}
{% tab title="Web" %}
```javascript
await session.setFullDevice(true)
```
{% endtab %}

{% tab title="iOS" %}
```objectivec
[session setFullDevice:YES callback:^(NSError* err, CBIOSession* session) {
    // catch errors
}];
```
{% endtab %}

{% tab title="Android" %}
```java
session.setFullDevice(true, (err, arg) -> {
    // handle error
});
```
{% endtab %}

{% tab title="ReactNative" %}
```javascript
await session.setFullDevice(true)
```
{% endtab %}
{% endtabs %}

These APIs can be used in combination with our various [delegate APIs](../listening-for-events.md) to switch the session in and out of full device mode as your use case requires.

For example, to request full device is used by default, you can set the full device state when the session is first loaded by the SDK:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.on('session.loaded', session => session.setFullDevice(true)) 
```
{% endtab %}

{% tab title="iOS" %}
```objectivec
// note: you must have implmented the CobrowseIODelegate
- (void)cobrowseSessionDidLoad:(CBIOSession *)session {
    // ensure the screen share session is always in full device
    [session setFullDevice:YES callback: null];
}
```
{% endtab %}

{% tab title="Android" %}
```java
// note: you must have implmented CobrowseIO.SessionLoadDelegate
@Override
public void sessionDidLoad(@NonNull Session session) {
    session.setFullDevice(true, null);
}
```
{% endtab %}
{% endtabs %}

### Controlling full device state from the Agent SDK

You may also set the full device state using the Agent SDK. See the Agent SDK [API Reference](../../agent-side-integrations/agent-sdk/api-reference.md) for more details.&#x20;

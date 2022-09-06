# Full device consent dialog

Before a session is upgraded to [full device](../full-device-capabilities/), some platforms require a consent from the user. Here is how to customize these consent prompts where possible:

{% tabs %}
{% tab title="Web" %}
### Customizing the full device UI on Web

```javascript
CobrowseIO.confirmFullDevice = function() {
    return new Promise(function(resolve, reject) {
        // your logic here
        // 
        // call CobrowseIO.currentSession.fullDevice() 
        // to get current full device state
        // 
        // you must show a button or UI as part of the DOM 
        // for the user to click before resolving the promise
        // 
        // call resolve(true) to start full device, or reject() 
        // to turn off full device
    });
}
```

If your custom confirmation prompt is not working in some browsers, please ensure you resolve the promise in response to a user action, such as a button click. This is required by both Safari and Firefox browsers when calling the browser's getDisplayMedia() API, which is used for full device screen share on Web.  The call to getDisplayMedia() must be made from code which is running in response to a user action, such as in an event handler.
{% endtab %}

{% tab title="iOS" %}
To override the the full device consent prompt, you should implement the `cobrowseHandleFullDeviceRequest:` method of `CobrowseIODelegate`.

When you override this method you are responsible for presenting a `RPSystemBroadcastPickerView` to the user. See the [Apple docs ](https://developer.apple.com/documentation/replaykit/rpsystembroadcastpickerview?language=objc)for further information on customizing this required view. This view is required by the iOS system in order to launch the broadcast extension.

```objectivec
-(void) cobrowseHandleFullDeviceRequest:(CBIOSession*) session {
    // You must show a RPSystemBroadcastPickerView which is provided by the iOS platform.
    // Call [session setFullDeviceState:kCBIOFullDeviceStateRejected callback:nil] if the user
    // dismisses the prompt without allowing the full device screen share.
}
```
{% endtab %}

{% tab title="Android" %}
{% hint style="info" %}
**Note:** you cannot replace the system prompt for switching to full device mode. This methods allows you to add logic before the system prompt is shown, or cancel the prompt entirely.
{% endhint %}

To override the the default remote control consent prompt, you should implement the `CobrowseIO.FullDeviceRequestDelegate` interface on your `CobrowseIO.Delegate`.

```java
@Override
public void handleFullDeviceRequest(@NonNull Activity activity, @NonNull Session session) {
    // show your own UI here
    // call session.setFullDeviceState(Session.FullDeviceState.On, null) to accept
    // or session.setFullDeviceState(Session.FullDeviceState.Rejected, null) to reject
    session.setFullDeviceState(Session.FullDeviceState.On, null);
}
```
{% endtab %}

{% tab title="macOS" %}
To override the the full device consent prompt, you should implement the `cobrowseHandleFullDeviceRequest:` method of `CobrowseIODelegate`.

```objectivec
-(void) cobrowseHandleFullDeviceRequest:(CBIOSession*) session {
    // Call [session setFullDeviceState:kCBIOFullDeviceStateRejected callback:nil] if the user
    // dismisses the prompt without allowing the full device screen share.
    // Or [session setFullDeviceState:kCBIOFullDeviceStateOn callback:nil] to allow the switch
    // full device mode.
}
```
{% endtab %}

{% tab title="Windows" %}
Full device by default, use the [user consent dialog](user-consent-dialog.md).
{% endtab %}
{% endtabs %}

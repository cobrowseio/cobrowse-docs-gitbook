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

{% tab title="React Native" %}
{% hint style="info" %}
Please follow the iOS and Android documentation to implement full device capabilities on React Native by following the steps outlined on [Full device screen sharing](../full-device-capabilities/full-device-screen-sharing.md)
{% endhint %}

To override the full device consent prompt, you should overwrite the `handleFullDeviceRequest` method on the sdk. For most scenarios, setting this as an empty function should be enough.

```javascript
CobrowseIO.handleFullDeviceRequest = (session) => {
    // your code
}
```

Whenever a full device request is received, the callback you set above will be called with the current session object and you're expected to handle the prompt within the application. Please check the examples below.

**iOS**

On iOS, you need to present a `RPSystemBroadcastPickerView` to the user. To achieve this, we expose a Native UI component that you must render on your component, `CBIOBroadcastPickerView`. **Since this is component represent a native view, please remember to set the width and height accordingly**. You can also control the color used to render the button by passing in an HEX color to the `buttonColor` prop.

```javascript
// FullDevicePrompt.ios.js
import React from 'react'
import {
  Text,
  View,
  Modal,
  Button
} from 'react-native'
import CobrowseIO, { CBIOBroadcastPickerView } from 'cobrowse-sdk-react-native'

CobrowseIO.handleFullDeviceRequest = () => {}

export function FullDevicePrompt () {
  const session = useSession();
  const isVisible = session?.full_device_state === 'requested'

  const reject = () => session?.setFullDevice('off')

  return (
    <Modal animationType='slide' visible={isVisible} onRequestClose={reject}>
      <View>
        <Text>Tap the record icon to manage full device screen sharing.</Text>
        <CBIOBroadcastPickerView
          style={{height: 50, width: 50}}
          buttonColor="#007AFF"
        />
      </View>
    </Modal>
  )
}
```

{% hint style="info" %}
Please note that **`RPSystemBroadcastPickerView` is an iOS only component and, as a result, the following code will not work on Android**. You must **ensure that this is only included for iOS** through the use of platform specific files or platform conditionals.
{% endhint %}

**Android**

On Android, we can't overwrite the system prompt but you can choose to present a prompt that explains what is happening to your users. To do this, you can follow the following example:

```javascript
// FullDevicePrompt.android.js
import React from 'react'
import {
  Text,
  View,
  Modal,
  Button
} from 'react-native'
import CobrowseIO, { CBIOBroadcastPickerView } from 'cobrowse-sdk-react-native'

CobrowseIO.handleFullDeviceRequest = () => {}

export function FullDevicePrompt () {
  const session = useSession();
  const isVisible = session?.full_device_state === 'requested'

  const reject = () => session?.setFullDevice('off')
  const displaySystemPrompt = () => session?.setFullDevice('on')

  return (
    <Modal animationType='slide' visible={isVisible} onRequestClose={reject}>
      <View>
         <Text>
            An agent has requested to screen share your full device.
          </Text>

          <Text>
            If you wish to accept please press "Continue" and "Accept" the
            system prompt. Otherwise press "Cancel".
          </Text>

          <Button title='Continue' onPress={displaySystemPrompt} />
          <Button title='Cancel' onPress={reject} />
      </View>
    </Modal>
  )
}
```

{% hint style="info" %}
Note that you must call `session?.setFullDevice('on')` on your prompt for the require Android prompt to be displayed so the user can accept it.
{% endhint %}

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

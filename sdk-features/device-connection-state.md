---
description: >-
  Know whether agents can currently start sessions with a device, and be told
  when that changes.
---

# Device connection state

When the SDK starts it registers the device with your account and opens a connection that agents use to start sessions with it. The SDK reports the state of that connection, so your app can show whether the device is reachable or log when the connection drops and recovers.

Sessions started from a [6-digit code](6-digit-codes.md) do not use this connection, so they work in any state.

### States

**disconnected** - The SDK is not trying to connect: it has not been started, `stop()` was called, or [automatic registration](advanced-features/starting-and-stopping-the-sdk.md#automatic-registration) is turned off.

**connecting** - The SDK has started and is connecting for the first time. A device that is reachable only by push notification, because your account has device sockets switched off, is given no connection to open and stays here.

**connected** - The device is connected. Agents can start sessions with it.

**reconnecting** - The connection was lost after it had been made. The SDK reconnects by itself.

**disabled** - Your account's configuration does not let agents start sessions with this device: agent-initiated sessions are switched off, device sockets are switched off, or the platform is not licensed. The state stays here until the device's registration says it is connectable again.

The typical transitions are `disconnected` -> `connecting` -> `connected`, then `reconnecting` -> `connected` whenever the network drops, and back to `disconnected` when the SDK is stopped.

{% hint style="info" %}
Further states may be added in future, so handle values you don't recognise.
{% endhint %}

### Reading the current state

The device exposed by the SDK always reports the state it is in now.

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.device.connectionState
```
{% endtab %}

{% tab title="iOS / MacOS" %}
**Swift**

```swift
CobrowseIO.instance().device.connectionState
```

**Objective-C**

```objectivec
CobrowseIO.instance.device.connectionState;
```
{% endtab %}

{% tab title="Android" %}
```kotlin
CobrowseIO.instance().device().connectionState()
```
{% endtab %}
{% endtabs %}

### Listening for changes

Every change is reported with two devices: the device as it is now, and a copy fixed at the state it was in before. On iOS, macOS and Android the [delegate](listening-for-events.md#delegate-implementation) is called on the main thread.

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.on('device.connection.updated', (device, previousDevice) => {
    switch (device.connectionState) {
        case 'connected':
            console.log('Agents can start sessions with this device')
            break
        default:
            console.log('Device connection state', device.connectionState)
    }
})
```
{% endtab %}

{% tab title="iOS / MacOS" %}
**Swift**

```swift
func cobrowseDeviceConnectionStateDidChange(_ device: CBIODevice, previousDevice: CBIODevice) {
    switch device.connectionState {
    case .connected:
        print("Agents can start sessions with this device")
    case .disconnected, .connecting, .reconnecting, .disabled:
        print("Agents cannot start sessions with this device")
    @unknown default:
        break
    }
}
```

**Objective-C**

```objectivec
-(void) cobrowseDeviceConnectionStateDidChange: (CBIODevice*) device previousDevice: (CBIODevice*) previousDevice {
    switch (device.connectionState) {
        case CBIODeviceConnectionStateConnected:
            NSLog(@"Agents can start sessions with this device");
            break;
        default:
            NSLog(@"Device connection state %ld", (long) device.connectionState);
            break;
    }
}
```
{% endtab %}

{% tab title="Android" %}
```kotlin
// Must implement CobrowseIO.DeviceConnectionStateDelegate
override fun deviceConnectionStateDidChange(device: Device, previousDevice: Device) {
    when (device.connectionState()) {
        CobrowseIO.DeviceConnectionState.Connected -> Log.i("App", "Agents can start sessions with this device")
        else -> Log.i("App", "Device connection state ${device.connectionState()}")
    }
}
```
{% endtab %}
{% endtabs %}

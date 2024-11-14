---
description: >-
  Effortlessly identify, search, and filter devices on your dashboard by
  specifying custom key/value pairs as metadata. Learn more.
---

# Identify your devices

To help you identify, search, and filter devices in your Cobrowse dashboard, it's helpful to specify any meaningful metadata.

You may add any custom key/value pairs you'd like, and they will all be searchable and filterable in your online dashboard. We've added a few placeholders for convenience only - all fields are optional.

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.customData = {
    user_id: "<your_user_id>",
    user_name: "<your_user_name>",
    user_email: "<your_user_email>",
    device_id: "<your_device_id>",
    device_name: "<your_device_name>"
};
```
{% endtab %}

{% tab title="iOS / MacOS" %}
**Swift**

```swift
CobrowseIO.instance().customData = [
    kCBIOUserIdKey: "<your_user_id>" as NSObject,
    kCBIOUserNameKey: "<your_user_name>" as NSObject,
    kCBIOUserEmailKey: "<your_user_email>" as NSObject,
    kCBIODeviceIdKey: "<your_device_id>" as NSObject,
    kCBIODeviceNameKey: "<your_device_name>" as NSObject
]
```

**Objective-C**

```objectivec
CobrowseIO.instance.customData = @{
    kCBIOUserIdKey: @"<your_user_id>",
    kCBIOUserNameKey: @"<your_user_name>",
    kCBIOUserEmailKey: @"<your_user_email>",
    kCBIODeviceIdKey: @"<your_device_id>",
    kCBIODeviceNameKey: @"<your_device_name>"
};
```
{% endtab %}

{% tab title="Android" %}
```java
HashMap<String, Object> customData = new HashMap<>();
customData.put("user_id", "<your_user_id>");
customData.put("user_name", "<your_user_name>");
customData.put("user_email", "<your_user_email>");
customData.put("device_id", "<your_device_id>");
customData.put("device_name", "<your_device_name>");
CobrowseIO.instance().customData(customData);
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.customData = {
    user_id: "<your_user_id>",
    user_name: "<your_user_name>",
    user_email: "<your_user_email>",
    device_id: "<your_device_id>",
    device_name: "<your_device_name>"
};
```
{% endtab %}

{% tab title="Flutter" %}
```dart
CobrowseIO.instance.setCustomData(<String, dynamic>{
    'user_id': '<your_user_id>',
    'user_name': '<your_user_name>',
    'user_email': '<your_user_email>',
    'device_id': '<your_device_id>',
    'device_name': '<your_device_name>'
});
```
{% endtab %}

{% tab title=".NET Mobile" %}
```csharp
CobrowseIO.Instance.CustomData = new Dictionary<string, object>
{
    { CobrowseIO.UserIdKey, "<your_user_id>" },
    { CobrowseIO.UserNameKey, "<your_user_name>" },
    { CobrowseIO.UserEmailKey, "<your_user_email>" },
    { CobrowseIO.DeviceIdKey, "<your_device_id>" },
    { CobrowseIO.DeviceNameKey, "<your_device_name>" }
};
```
{% endtab %}

{% tab title="Windows" %}
```csharp
CobrowseIO.Instance.CustomData = new Dictionary<string, object>()
{
    { CobrowseIO.UserIdKey, "<your_user_id>" },
    { CobrowseIO.UserNameKey, "<your_user_name>" },
    { CobrowseIO.UserEmailKey, "<your_user_email>" },
    { CobrowseIO.DeviceIdKey, "<your_device_id>" },
    { CobrowseIO.DeviceNameKey, "<your_device_name>" }
};
```
{% endtab %}
{% endtabs %}

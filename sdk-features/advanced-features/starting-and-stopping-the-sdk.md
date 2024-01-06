---
description: Have greater control over when the SDK is running.
---

# Starting and stopping the SDK

Starting the Cobrowse SDK is often done early in your application. However, you have the ability to delay the start of the SDK or programmatically stop the SDK so that it runs only when needed.

### Starting the SDK

{% tabs %}
{% tab title="Web" %}
```javascript
await CobrowseIO.client();
await CobrowseIO.start();
```

#### Advanced usage

Sometimes it is required to run Cobrowse.io **only** within an IFrame, and not any containing or parent page. This is supported, but requires passing an extra configuration option when starting Cobrowse. **Most implementations should not need to use this.** Please contact us if you are unsure.

```javascript
await CobrowseIO.start({ allowIFrameStart: true });
```
{% endtab %}

{% tab title="iOS" %}
**Swift**

```swift
CobrowseIO.instance().start()
```

**Objective-C**

```objectivec
[CobrowseIO.instance start];
```
{% endtab %}

{% tab title="Android" %}
```java
CobrowseIO.instance().start(this);
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.start();
```
{% endtab %}

{% tab title="Xamarin / .NET Mobile" %}
```csharp
CobrowseIO.Instance.Start();
```
{% endtab %}

{% tab title="Windows" %}
```csharp
await CobrowseIO.Instance.Start();
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you do decide to delay or lazy load the SDK it is important to check if there is a currently active session and if so load and start the SDK right away. Without this check sessions would not continue between navigation or page refreshes as the SDK won't be loaded.

```
// On page load check if we should start the Cobrowse SDK right away
if (CobrowseIO.currrentSession) {
  await CobrowseIO.client();
  await CobrowseIO.start();
}
```
{% endhint %}

### Automatic registration

By default, when the SDK starts it will register the device to your account and share its connectivity state. This provides the dashboard with a list of devices which are online and ready to connect.

If you don't need to see a list of devices in your dashboard, e.g. your sessions start only using [6-digit codes](../customize-the-interface/customize-6-digit-code-screen.md), then you can stop the SDK from registering the device and its connectivity status by setting the registration option with a value of `false`.

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.registration = false;
```
{% endtab %}

{% tab title="iOS" %}
```objc
CobrowseIO.instance.registration = NO;
```
{% endtab %}

{% tab title="Android" %}
```java
CobrowseIO.instance().registration(false);
```
{% endtab %}

{% tab title="Windows" %}
```csharp
CobrowseIO.Instance.Registration = false;
```
{% endtab %}
{% endtabs %}

### Stopping the SDK

{% hint style="info" %}
Calling `stop()` will stop Cobrowse completely and you won't be able to Cobrowse again until you call `start()`. If you only wish to end a session please use `end()` method on the `Session` object.
{% endhint %}

{% tabs %}
{% tab title="Web" %}
```javascript
await CobrowseIO.stop();
```
{% endtab %}

{% tab title="iOS" %}
**Swift**

```swift
CobrowseIO.instance().stop()
```

**Objective-C**

```objectivec
[CobrowseIO.instance stop];
```
{% endtab %}

{% tab title="Android" %}
```java
CobrowseIO.instance().stop();
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.stop();
```
{% endtab %}

{% tab title="Xamarin / .NET Mobile" %}
```csharp
CobrowseIO.Instance.Stop();
```
{% endtab %}

{% tab title="Windows" %}
```csharp
CobrowseIO.Instance.Stop();
```
{% endtab %}
{% endtabs %}

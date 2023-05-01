---
description: Have greater control over when the SDK is running.
---

# Starting and stopping the SDK

Starting the Cobrowse SDK is often done early in your application. However, you have the ability to delay the start of the SDK or programmatically stop the SDK so that it runs only when needed.&#x20;

### Starting the SDK

{% tabs %}
{% tab title="Web" %}
```javascript
await CobrowseIO.client();
await CobrowseIO.start();
```

####

#### Starting options

The `start()` method accepts some options to allow customization of the start up behaviour.



**register**

By default, when the SDK starts it will register the device to your account and share its connectivity state. This provides the dashboard with a list of devices which are online and ready to connect.&#x20;

If you don't need to see a list of devices in your dashboard, e.g. your sessions start only using [6-digit codes](../customize-the-interface/customize-6-digit-code-screen.md), then you can stop the SDK from registering the device and its status by passing the `register` option with a value of `false`.



```javascript
await CobrowseIO.start({ register: false });
```

****

**allowIFrameStart**

A few customers want to run Cobrowse.io only within an IFrame, and not any containing or parent page. This is supported, but requires passing an extra configuration option when starting Cobrowse.&#x20;



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

#### Automatic registration

By default, when the SDK starts it will register the device to your account and share its connectivity state. This provides the dashboard with a list of devices which are online and ready to connect.

If you don't need to see a list of devices in your dashboard, e.g. your sessions start only using [6-digit codes](../customize-the-interface/customize-6-digit-code-screen.md), then you can stop the SDK from registering the device and its status by setting the `registration` option with a value of `NO`.

```objc
CobrowseIO.instance.registration = NO;
```
{% endtab %}

{% tab title="Android" %}
```java
CobrowseIO.instance().start(this);
```

#### Automatic registration

By default, when the SDK starts it will register the device to your account and share its connectivity state. This provides the dashboard with a list of devices which are online and ready to connect.

If you don't need to see a list of devices in your dashboard, e.g. your sessions start only using [6-digit codes](../customize-the-interface/customize-6-digit-code-screen.md), then you can stop the SDK from registering the device and its status by passing the `registration` option with a value of `false`.

```java
CobrowseIO.instance().registration(false);
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.start();
```
{% endtab %}

{% tab title="Xamarin" %}
```csharp
CobrowseIO.Instance.Start();
```
{% endtab %}

{% tab title="Windows" %}
```csharp
await CobrowseIO.Instance.Start();
```

#### Automatic registration

By default, when the SDK starts it will register the device to your account and share its connectivity state. This provides the dashboard with a list of devices which are online and ready to connect.

If you don't need to see a list of devices in your dashboard, e.g. your sessions start only using [6-digit codes](../customize-the-interface/customize-6-digit-code-screen.md), then you can stop the SDK from registering the device and its status by setting the `Registration` option with a value of `false`.

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

{% tab title="Xamarin" %}
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

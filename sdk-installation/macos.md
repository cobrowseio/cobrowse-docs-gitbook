---
description: MacOS SDK
---

# MacOS

## Installation

The Cobrowse SDK for MacOS is available for installation via several dependency managers, or as a framework to integrate directly into your project:

{% tabs %}
{% tab title="Pods" %}
```ruby
pod 'CobrowseIO', '~>2'
```
{% endtab %}

{% tab title="Carthage" %}
Carthage support is currently unavailable for our MacOS SDK as Carthage does not yet support XCFrameworks.
{% endtab %}

{% tab title="Manual" %}
Add CobrowseIO._**xc**framework_  (_**not**_ CobrowseIO.framework) to your project to use the SDK for MacOS:

```
https://github.com/cobrowseio/cobrowse-sdk-ios-binary/releases
```
{% endtab %}
{% endtabs %}

_Don't forget to run `pod repo update` then `pod install` after you've edited your Podfile._

{% tabs %}
{% tab title="MacOS (Swift)" %}
```swift
import CobrowseIO

func applicationDidFinishLaunching(_ aNotification: Notification)
{
    CobrowseIO.instance().license = "put your license key here"
    CobrowseIO.instance().start()
}
```
{% endtab %}

{% tab title="MacOS (Objective-C)" %}
```objectivec
@import CobrowseIO;

- (void)applicationDidFinishLaunching:(NSNotification *)notification
{
    CobrowseIO.instance.license = @"put your license key here";
    [CobrowseIO.instance start];
}
```
{% endtab %}
{% endtabs %}

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

## Try it out

Once you have your app running, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* MacOS 10.13 or later

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

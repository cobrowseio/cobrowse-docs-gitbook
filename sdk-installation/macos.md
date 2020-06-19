---
description: MacOS SDK
---

# MacOS

### Installation

We recommend installing the Cobrowse.io SDK using Cocoapods. Add this to your Podfile:

```ruby
pod 'CobrowseIO', '~>2'
```

_Don't forget to run `pod repo update` then `pod install` after you've edited your Podfile._

{% tabs %}
{% tab title="MacOS \(Swift\)" %}
```swift
import CobrowseIO

func applicationDidFinishLaunching(_ aNotification: Notification)
{
    CobrowseIO.instance().license = "<your license key here>"
    CobrowseIO.instance().start()
}
```
{% endtab %}

{% tab title="MacOS \(Objective-C\)" %}
```objectivec
@import CobrowseIO;

- (void)applicationDidFinishLaunching:(NSNotification *)notification
{
    CobrowseIO.instance.license = @"<your license key here>";
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

* MacOS 10.10 or later

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

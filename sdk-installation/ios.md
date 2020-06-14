---
description: iOS Native SDK
---

# iOS

Cobrowse.io for iOS supports iOS 9.0+.

## Installation

We recommend installing the Cobrowse.io SDK using Cocoapods. Add this to your Podfile:

```ruby
pod 'CobrowseIO', '~>2'
```

_Don't forget to run `pod repo update` then `pod install` after you've edited your Podfile._

{% tabs %}
{% tab title="iOS \(Swift\)" %}
```swift
import CobrowseIO

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
{
    CobrowseIO.instance().license = "<your license key here>"
    CobrowseIO.instance().start()
    return true
}
```
{% endtab %}

{% tab title="iOS \(Objective-C\)" %}
```objectivec
@import CobrowseIO;

- (BOOL)application:(UIApplication*) application didFinishLaunchingWithOptions:(NSDictionary*) launchOptions
{
    CobrowseIO.instance.license = @"<your license key here>";
    [CobrowseIO.instance start];
    return YES;
}
```
{% endtab %}
{% endtabs %}

_Important: Do this in your `application:didFinishLaunchingWithOptions:` implementation to make sure your device shows up in your dashboard right away._

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

## Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Questions?

Any questions at all? Please email us directly at [hello@cobrowse.io](mailto:hello@cobrowse.io).

## Requirements

* iOS 9.0 or later


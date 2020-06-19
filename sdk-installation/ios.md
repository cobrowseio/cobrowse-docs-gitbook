---
description: iOS Native SDK
---

# iOS

### Installation

The Cobrowse SDK for iOS is available for installation via several dependency manager, or as frameworks to integrate into your project manually:

{% tabs %}
{% tab title="Pods" %}
```ruby
pod 'CobrowseIO', '~>2'
```
{% endtab %}

{% tab title="Carthage" %}
```
github "cobrowseio/cobrowse-sdk-ios-binary" ~> 2.0
```
{% endtab %}

{% tab title="Manual" %}
Frameworks are available for manual integration into your Xcode projects from:

```
https://github.com/cobrowseio/cobrowse-sdk-ios-binary/releases
```
{% endtab %}
{% endtabs %}

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

## Requirements

* iOS 9.0 or later

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}


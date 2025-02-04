---
description: iOS Native SDK
---

# iOS

## Installation

The Cobrowse SDK for iOS is available for installation via several dependency managers, or as frameworks to integrate directly into your project:

{% tabs %}
{% tab title="SPM" %}
```
https://github.com/cobrowseio/cobrowse-sdk-ios-binary.git
```

Add the `CobrowseIO` package dependency to **your app target**.
{% endtab %}

{% tab title="Pods" %}
```ruby
pod 'CobrowseIO', '~>2'
```

_Don't forget to run `pod repo update` then `pod install` after you've edited your Podfile._

_Make sure you are on the latest stable version of Pods. Run `pod --version` to check!_

**Note:** Depending on your project, you may also need `use_frameworks!` in your Podfile.
{% endtab %}

{% tab title="Carthage" %}
```
binary "https://raw.githubusercontent.com/cobrowseio/cobrowse-sdk-ios-binary/master/cobrowse-sdk-ios-binary.json" ~> 2.0
```

{% hint style="info" %}
Remember to run `carthage update` after modifying your Cartfile.
{% endhint %}

_More information about Carthage at_ [_https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos_](https://github.com/Carthage/Carthage#if-youre-building-for-ios-tvos-or-watchos)_._

Link the `CobrowseIO.framework` to your main app target that can be found at `./Carthage/Build/iOS`.
{% endtab %}

{% tab title="Manual" %}
Frameworks are available for manual integration into your Xcode projects from:

```
https://github.com/cobrowseio/cobrowse-sdk-ios-binary/releases
```
{% endtab %}
{% endtabs %}

Add the following lines to your code which will register this device with the Cobrowse servers so you can connect to it. You could choose to do this on app startup, or when your users visits a support page in your application, or any other time.

{% tabs %}
{% tab title="Swift" %}
```swift
import CobrowseIO
...

CobrowseIO.instance().license = "put your license key here"
CobrowseIO.instance().start()
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
@import CobrowseIO;
...

CobrowseIO.instance.license = @"put your license key here";
[CobrowseIO.instance start];
```
{% endtab %}
{% endtabs %}

#### Registering on startup

If you would like your devices to register as soon as your app starts, we recommend doing this in your app delegate `application:didFinishLaunchingWithOptions:` method. For example:

{% tabs %}
{% tab title="Swift" %}
```swift
import CobrowseIO

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
{
    CobrowseIO.instance().license = "put your license key here"
    CobrowseIO.instance().start()
    return true
}
```
{% endtab %}

{% tab title="Objective-C" %}
```objectivec
@import CobrowseIO;

- (BOOL)application:(UIApplication*) application didFinishLaunchingWithOptions:(NSDictionary*) launchOptions
{
    CobrowseIO.instance.license = @"put your license key here";
    [CobrowseIO.instance start];
    return YES;
}
```
{% endtab %}
{% endtabs %}

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

## Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* iOS 11.0 or later

## Problems rendering certain views?

Try our alternative rendering method below:

{% content-ref url="../sdk-features/advanced-features/ios/alternate-render-method.md" %}
[alternate-render-method.md](../sdk-features/advanced-features/ios/alternate-render-method.md)
{% endcontent-ref %}

## Adding custom touch handlers

{% content-ref url="../sdk-features/advanced-features/ios/custom-touch-handling.md" %}
[custom-touch-handling.md](../sdk-features/advanced-features/ios/custom-touch-handling.md)
{% endcontent-ref %}

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

---
description: MacOS SDK for native desktop applications
---

# macOS

## Installation

The Cobrowse SDK for macOS is available for installation via several dependency managers, or as a framework to integrate directly into your project:

{% tabs %}
{% tab title="SPM" %}
```
https://github.com/cobrowseio/cobrowse-sdk-ios-binary.git
```

Add the `CobrowseSDK` package dependency to **your app target**.
{% endtab %}

{% tab title="Pods" %}
```ruby
pod 'CobrowseIO', '~>3'
```

_Don't forget to run `pod repo update` then `pod install` after you've edited your Podfile._
{% endtab %}

{% tab title="Carthage" %}
<pre><code><strong>binary "https://raw.githubusercontent.com/cobrowseio/cobrowse-sdk-ios-binary/master/cobrowse-sdk-ios-binary.json" ~> 3.0
</strong></code></pre>

{% hint style="info" %}
Remember to run `carthage update --use-xcframeworks` after modifying your Cartfile.
{% endhint %}

Link the `CobrowseSDK.xcframework` to your main app target that can be found at `./Carthage/Build`.
{% endtab %}

{% tab title="Manual" %}
Add `CobrowseSDK.xcframework`, _**not**_ `CobrowseSDK.framework`, to your project to use the SDK for MacOS:

```
https://github.com/cobrowseio/cobrowse-sdk-ios-binary/releases
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="MacOS (Swift)" %}
```swift
import CobrowseSDK
...

CobrowseIO.instance().license = "put your license key here"
CobrowseIO.instance().start()
```
{% endtab %}

{% tab title="MacOS (Objective-C)" %}
```objectivec
@import CobrowseSDK;
...

CobrowseIO.instance.license = @"put your license key here";
[CobrowseIO.instance start];
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

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
{% hint style="warning" %}
[CocoaPods will no longer accept pod updates after 2nd December 2026](https://blog.cocoapods.org/CocoaPods-Specs-Repo/) so Cobrowse cannot publish updates through that platform after this date. We recommend migrating to the Swift Package Manager (SPM) instead.
{% endhint %}

<pre class="language-ruby"><code class="lang-ruby"><strong>pod 'CobrowseIO', '~>3'
</strong></code></pre>

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

* MacOS 13.0 or later

## Stay up to date

There are a few ways to find out about new releases of the macOS SDK:

* Watch the [cobrowse-sdk-ios-binary](https://github.com/cobrowseio/cobrowse-sdk-ios-binary) repository on GitHub and choose **Custom → Releases** to get an email for each new release. If you use GitHub Enterprise and can't watch repositories outside your organization, subscribe to `https://github.com/cobrowseio/cobrowse-sdk-ios-binary/releases.atom` in your RSS reader instead.
* If you use SPM, choose **File → Packages → Update to Latest Package Versions** in Xcode.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

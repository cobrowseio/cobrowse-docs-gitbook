---
description: iOS Native SDK
---

# iOS

Cobrowse.io for iOS supports iOS 9.0+.

### Installation

We recommend installing the Cobrowse.io SDK using Cocoapods. Add this to your Podfile:

```ruby
pod 'CobrowseIO', '~>2'
```

_Don't forget to run `pod repo update` then `pod install` after you've edited your Podfile._

**Swift**

```swift
import CobrowseIO

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
{
    CobrowseIO.instance().license = "<your license key here>"
    CobrowseIO.instance().start()
    return true
}
```

**Objective C**

```text
@import CobrowseIO;

- (BOOL)application:(UIApplication*) application didFinishLaunchingWithOptions:(NSDictionary*) launchOptions
{
    CobrowseIO.instance.license = @"<your license key here>";
    [CobrowseIO.instance start];
    return YES;
}
```

_Important: Do this in your `application:didFinishLaunchingWithOptions:` implementation to make sure your device shows up in your dashboard right away._

#### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

#### Add device metadata

To help you identify, search, and filter devices in your Cobrowse dashboard, it's helpful to specify any meaningful metadata. We recommend specifying the end-user's email if available.

You may add any custom key/value pairs you'd like, and they will all be searchable and filterable in your online dashboard. We've added a few placeholders for convenience only - all fields are optional.

### Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

### Questions?

Any questions at all? Please email us directly at [hello@cobrowse.io](mailto:hello@cobrowse.io).

### Requirements

* iOS 9.0 or later


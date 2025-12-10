---
description: >-
  Some iOS apps with custom controls, lots of animation, or complex visual
  effects, can benefit from alternate render methods used by the Cobrowse iOS
  SDK.
---

# Alternate render method

#### Liquid Glass

With the introduction of [Liquid Glass](https://www.apple.com/uk/newsroom/2025/06/apple-introduces-a-delightful-and-elegant-new-software-design/) some components may render incorrectly to the agent.

We recommend testing an alternate render method by adding a plist entry to your `Info.plist` as follows:

```xml
<key>CBIORenderMethod</key>
<integer>4</integer>
```

Please note this render method requires the `CobrowseSDK` version [3.11.0](https://github.com/cobrowseio/cobrowse-sdk-ios-binary/blob/master/CHANGELOG.md#3110-2025-09-18) or higher.

#### Other render methods

There may be a small performance cost for some apps, particularly noticeable in apps with heavy scrolling.&#x20;

You may test this alternate render method by adding a plist entry to your `Info.plist` as follows:

```markup
<key>CBIORenderMethod</key>
<integer>2</integer>
```

If you've been able to test this alternate render method in your iOS app, we would love to hear your feedback. Please drop us an email at [hello@cobrowse.io](mailto:hello@cobrowse.io).

# Alternate render method

A small number of iOS apps, usually with highly customized controls, lots of animation, and/or complex visual effects, can benefit from an alternate render method used by the Cobrowse.io iOS SDK. There may be a small performance cost for some apps, particularly noticeable in apps with heavy scrolling.&#x20;

You may test this alternate render method by adding a plist entry to your Info.plist as follows:

```markup
<key>CBIORenderMethod</key>
<integer>2</integer>
```

If you've been able to test this alternate render method in your iOS app, we would love to hear your feedback. Please drop us an email at [hello@cobrowse.io](mailto:hello@cobrowse.io).

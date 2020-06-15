# Full device screenshare

### Overview

By default, the Cobrowse.io SDKs for iOS will capture the user activity inside your app only. You can enable the capture of the full device, i.e. screens outside your app, including home screen, device settings, and everything else. Follow this guide to add the App Extension required for capturing full device frames.

### Implementation

**Add a Broadcast Extension target**

1. Open your Xcode project
2. Navigate to File &gt; New &gt; Target...
3. Pick "Broadcast Upload Extension"
4. Enter a name for the target
5. Uncheck "Include UI Extension"
6. Create the target, noting its bundle ID
7. Change the target SDK of your Broadcast Extension target to at least iOS 10.0

**Set up Keychain Sharing**

Your app and the app extension you created above need to share some secrets via the iOS Keychain. They do this using their own Keychain group so they are isolated from the rest of your apps Keychain.

In **both** your **app target** and your **extension target** add a Keychain Sharing entitlement for the "io.cobrowse" keychain group.

**Add the bundle ID to your plist**

Take the bundle ID of the **extension** you created above, and add the following entry in your apps `Info.plist` \(_Note:_ **not** in the extensions `Info.plist`\), replacing the bundle ID below with your own:

```markup
<key>CBIOBroadcastExtension</key>
<string>your.app.extension.bundle.ID.here</string>
```

**Add the new target to your Podfile**

The app extension needs a dependency on the CobrowseIO app extension framework. Add the following to your Podfile, replacing the target name with you own extensions target name:

```ruby
target 'YourExtensionTargetName' do
    pod 'CobrowseIO/Extension', '~>2'
end
```

_Make sure to run `pod install` after updating your Podfile_

**Implement the extension**

Xcode will have added `SampleHandler.m` and `SampleHandler.h` \(or `SampleHander.swift`\) files as part of the target you created earlier. Replace the content of the files with the following:

{% tabs %}
{% tab title="iOS \(Swift\)" %}
```swift
import CobrowseIOAppExtension

class SampleHandler: CobrowseIOReplayKitExtension {

}
```
{% endtab %}

{% tab title="iOS \(Objective-C\)" %}
```objectivec
// SampleHandler.h

@import CobrowseIOAppExtension;

@interface SampleHandler : CobrowseIOReplayKitExtension

@end
```

```objectivec
// SampleHandler.m

#import "SampleHandler.h"

@implementation SampleHandler

@end
```
{% endtab %}
{% endtabs %}



**Build and run your app**

You're now ready to build and run your app. The full device capability is only available on physical devices, it will not work in the simulator.


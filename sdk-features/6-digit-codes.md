---
description: >-
  Cobrowse does not require any visible UI presented to the user but there is
  the option to present a UI that generates a 6-digit code. Learn more.
---

# Use 6-digit codes

By default, Cobrowse does not require any visible UI presented to the user. It will exist in the background of your app, and only activate when agents initiate a new session.

You may optionally present a UI in your app that enables users to generate 6-digit codes. Users may then read a code over the phone or in chat, and agents can use the 6-digit code to initiate the Cobrowse session.

{% hint style="info" %}
**Important**: 6-digit codes expire after approximately 20 minutes, so it's best practice to generate a code only when a user wants to start a session.&#x20;
{% endhint %}

To generate a 6-digit code in your integration you can use the following APIs. Once you have generated the code you can display it to the user in your own UI. You should only generate a code when a user needs it as they expire shortly after creation.

{% tabs %}
{% tab title="Web" %}
```javascript
// ensure Cobrowse is loaded
CobrowseIO.client().then(function() {
    // create a code a display it to the user using your own UI
    // ONLY GENERATE CODE WHEN NEEDED. DO NOT GENERATE CODE ON PAGE LOAD.
    CobrowseIO.createSessionCode().then(function(code) {
       console.log('your code is', code);
    });
});
```
{% endtab %}

{% tab title="iOS / MacOS" %}
```objectivec
[CobrowseIO.instance createSession:^(NSError * err, CBIOSession * session) {
    if (err) NSLog(@"Failed to create code")
    else NSLog(@"%@", session.code);
}];
```
{% endtab %}

{% tab title="Android" %}
```java
CobrowseIO.instance().createSession((err, session) -> {
    if (err != null) Log.w("App", "Failed to create code");
    else if (session != null) Log.i("App", "Session code " + session.code());
});
```
{% endtab %}

{% tab title="React Native" %}
```javascript
const session = await CobrowseIO.createSession()
console.log('Your 6 digit code is', session.code)
```
{% endtab %}

{% tab title="Flutter" %}
```dart
Session session = await CobrowseIO.instance.createSession();
log('Your session code: ${session.code}');
```
{% endtab %}

{% tab title=".NET Mobile" %}
```csharp
CobrowseIO.Instance.CreateSession((Exception err, ISession session) => {
    if (err != null) Debug.WriteLine("Failed to create a code");
    else if (session != null) Debug.WriteLine("Session code " + session.Code);
});
```
{% endtab %}

{% tab title="Windows" %}
```csharp
Session session = await CobrowseIO.Instance.CreateSession();
Console.WriteLine("Code: {0}", session.Code);
```
{% endtab %}
{% endtabs %}

If you are **only** using 6 digit codes to start Cobrowse sessions in your implementation, you can prevent the SDK registering in your Cobrowse account until a screen sharing session is required. See the documentation on preventing automatic registration.

{% content-ref url="advanced-features/starting-and-stopping-the-sdk.md" %}
[starting-and-stopping-the-sdk.md](advanced-features/starting-and-stopping-the-sdk.md)
{% endcontent-ref %}

You can monitor changes in the state of the session using the Cobrowse delegate methods to listen for events:

{% content-ref url="listening-for-events.md" %}
[listening-for-events.md](listening-for-events.md)
{% endcontent-ref %}

### Default 6-digit code UI

We provide a very simple default plug-and-play UI that provides the 6-digit code display functionality. We also provide a set of public methods if you prefer to build a custom UI (see above). Learn more at [Customize the interface.](customize-the-interface/)

{% tabs %}
{% tab title="Web" %}
The Cobrowse.io SDK for web does not provide a default UI for generating 6 digit codes.
{% endtab %}

{% tab title="iOS" %}
1. Add the appropriate code below into a view controller in your app.
2. Hook up a trigger for the action (or call it programatically if you prefer).

#### Swift

```swift
import CobrowseIO

class ExampleViewController: UIViewController {
    var sessionController: CBIOViewController!

    @IBAction func startCobrowse(sender: UIBarButtonItem) {
        self.sessionController = CBIOViewController()
        self.navigationController?.pushViewController(self.sessionController, animated: true)
    }

    // ... the rest of your view controller
}
```

For a full example written in Swift, see our Listr sample app at: [https://github.com/cobrowseio/Listr](https://github.com/cobrowseio/Listr)

#### Objective C

```objectivec
@import CobrowseIO;

@implementation ExampleViewController {
    CBIOViewController* sessionController;
}

-(IBAction) startCobrowse:(id)sender {
    sessionController = [[CBIOViewController alloc] init];
    [self.navigationController pushViewController:sessionController animated:YES];
}

// ... the rest of your view controller

@end
```

Make sure you've hooked up a trigger for the `startCobrowse` IBAction that we've just added. Then head to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) and enter the 6 digit code that will be generated by your app when you trigger the action!
{% endtab %}

{% tab title="Android" %}
```java
import io.cobrowse.ui.CobrowseActivity;

public class YourActivity extends Activity {

    // the rest of your Activity

    public void startCobrowse(View view) {
        Intent intent = new Intent(this, CobrowseActivity.class);
        startActivity(intent);
    }
}
```

Make sure you've hooked up a trigger for the `startCobrowse` method that we've just added. Then head to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) and enter the 6 digit code that will be generated by your app when you trigger the action!
{% endtab %}

{% tab title="React Native" %}
We've provided a view that will do all the session creation and management for you. All you need to do is include this somewhere in your react native view hierarchy. It's not a requirement to use this UI, you can easily build your own if you like!

```javascript
import { CobrowseView } from 'cobrowse-sdk-react-native';

export default class App extends Component {
    render() {
        return (
            <View>
                <CobrowseView />
            </View>
        );
    }
}
```
{% endtab %}

{% tab title="Flutter" %}
The Cobrowse.io SDK for Flutter does not provide a default UI for generating 6 digit codes, but you can show the default UI shipped in the native SDKs using [Flutter platform channel](https://docs.flutter.dev/platform-integration/platform-channels). It's not a requirement to use this UI, you can easily build your own if you like!

#### iOS implementation

```objc
@import CobrowseIO;

UIViewController *rootViewController = [UIApplication sharedApplication].keyWindow.rootViewController;
CBIOViewController *sessionController = [[CBIOViewController alloc] init];
[rootViewController presentViewController:sessionController animated:YES completion:nil];
```

#### Android implementation

```java
import io.cobrowse.ui.CobrowseActivity;

Intent intent = new Intent(activity, CobrowseActivity.class);
activity.startActivity(intent);
```

{% endtab %}

{% tab title=".NET Mobile" %}

```csharp
using Cobrowse.IO;

public partial class MainPage : Microsoft.Maui.Controls.ContentPage
{
    public void StartCobrowse()
    {
        CobrowseIO.Instance.OpenCobrowseUI();
    }
}
```
{% endtab %}

{% tab title="MacOS" %}
The Cobrowse.io SDK for MacOS does not provide a default UI for generating 6 digit codes.
{% endtab %}

{% tab title="Windows" %}
The Cobrowse.io SDK for Windows does not provide a default UI for generating 6 digit codes, instead you can generate a code for your UI by using the `CreateSession` API.

A complete example of a WPF integration is available [here](https://github.com/cobrowseio/cobrowse-sdk-windows-examples).
{% endtab %}
{% endtabs %}

To see some code samples for how to build a custom UI for your platform see:

{% content-ref url="customize-the-interface/customize-6-digit-code-screen.md" %}
[customize-6-digit-code-screen.md](customize-the-interface/customize-6-digit-code-screen.md)
{% endcontent-ref %}

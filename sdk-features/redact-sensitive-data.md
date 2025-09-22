---
description: >-
  Ensure secure remote screen viewing using the redaction API to automatically
  block sensitive data such as credit card details, social security numbers and
  more.
---

# Redact sensitive data

When remotely viewing a user's screen, there may be certain sensitive data that should not be viewable by the agent.

For this purpose, we provide a redaction API that automatically blocks out on device all sensitive data sources such as credit cards, social security numbers, etc. When certain data is redacted, it will never leave the user's device.

Cobrowse provides two methods for redacting sensitive data in your applications:

### **1. Define the redacted views in your app source code (recommended)**

This is the recommended method as it will make sure your redactions are tied to application or websites code version.

{% tabs %}
{% tab title="Web" %}
Redactions are defined as CSS selectors, passed as an array to the Cobrowse SDK. We recommend using a simple css class to signify redaction where possible, although more complex selectors will also work.

```javascript
CobrowseIO.redactedViews = ['.redacted', ...some other selectors...]
```

Selectors can also be scoped by URL or [glob pattern](https://www.npmjs.com/package/glob-to-regexp#usage) by providing an `Object` where the key is the URL or glob pattern and the value is the array of `String` CSS Selectors to redact when viewing that page.

The example below will always redact any element with the `redacted` class but will only redact all input elements when viewing any example.com page.

```
CobrowseIO.redactedViews = [ 
    '.redacted',
    { 'example.com*' : [ 'input' ] }
]
```

#### **Unredaction aka** Private by Default

Our web SDK also supports an un-redaction mechanism, where by you can define sub-elements inside of a redacted element that should be visible to the agent. You can specify un-redaction selectors like this:

```javascript
CobrowseIO.unredactedViews = ['.unredacted', ...some other selectors...]
```
{% endtab %}

{% tab title="iOS" %}
To redact a single view you can call `cobrowseRedacted` on any `UIView` or SwiftUI `View`.

```swift
// UIKit
let uiView = UIView()
uiView.cobrowseRedacted()

// SwiftUI
let swiftUIView = ExampleView()
swiftUIView.cobrowseRedacted()
```

Any children of the view will also be redacted allowing you to redact container views like `VStack` or a `UIView` that contains subviews.

#### Redact within UIViewController via CobrowseIORedacted

You may wish to return all the views that should be redacted at once per view controller. By conforming your `UIViewController` to `CobrowseIORedacted` you can proved the reference to each `UIView` that should be redacted.

```swift
class ExampleViewController: UIViewController, CobrowseIORedacted {
    let imageView: UIImageView
    @IBOutlet weak var label: UILabel!
    
    ...
    func redactedViews() -> [UIView] {
        [imageView, label]
    }
}
```

#### Redact within custom delegate via CobrowseIODelegate

If you can't make changes to the `UIViewController` or you wish to centralize your redaction logic. You will need an object that conforms to `CobrowseIODelegate` and implement the `cobrowseRedactedViews(for vc: UIViewController)` within it.

This method provides you with the `UIViewController` whereby you return the views within that view controller you wish to redact.

```swift
class ExampleDelegate: NSObject, CobrowseIODelegate {
    func cobrowseRedactedViews(for vc: UIViewController) -> [UIView] {
        // Return an array of UIViews to redact
        // For example only the subviews that have the tag of 1
        vc.view.subviews.filter { $0.tag == 1 }
    }
}
```

Be sure to set the `delegate` property to your custom delegate before calling `start()`.

```swift
let delegate = ExampleDelegate()
let cobrowse = CobrowseIO.instance()

cobrowse.delegate = delegate
cobrowse.start()
```

#### **Unredaction aka** Private by Default

Sometimes you may want to redact everything on the screen, then selectively "unredact" only the parts your support agents should be able to see. This is particularly useful on applications that require a higher privacy standard or where only specific sections of the App should be visible to the agent.

To achieve this, you should return all of your application's root views via the `cobrowseRedactedViews(for vc: UIViewController)` delegate method.

_Please note the example below may not redact all your root views as this can be application dependent._

```swift
@main
class AppDelegate: UIResponder, UIApplicationDelegate, CobrowseIODelegate {
    
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        let cobrowse = CobrowseIO.instance()
        cobrowse.delegate = self
        cobrowse.start()
        
        return true
    }
    
    func cobrowseRedactedViews(for vc: UIViewController) -> [UIView] {
        guard let view = window?.rootViewController?.view
            else { return [] }
        
        return [view]
    }
}
```

Once you've done this, your application root views will be redacted by default and you'll be able to unredact child components to make them visible to the agents by implementing `cobrowseUnredactedViews(for vc: UIViewController)` in your `CobrowseIODelegate` class:

```swift
class ExampleDelegate: NSObject, CobrowseIODelegate {
    func cobrowseUnredactedViews(for vc: UIViewController) -> [UIView] {
        // Return an array of UIViews to unredact
        // For example only the subviews that have the tag of 1
        vc.view.subviews.filter { $0.tag == 1 }
    }
}
```

Alternatively, you can implement `CobrowseIOUnredacted` protocol in your `UIViewController` subclasses:

```swift
class ExampleViewController: UIViewController, CobrowseIOUnredacted {
    let imageView: UIImageView
    @IBOutlet weak var label: UILabel!
    
    ...
    func unredactedViews() -> [UIView] {
        [imageView, label]
    }
}
```

#### **Redaction of WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `webviewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```swift
let cobrowse = CobrowseIO.instance()
cobrowse.webviewRedactedViews = [ ".redacted", ...some other selectors... ]
```
{% endtab %}

{% tab title="Android" %}
Implement the `CobrowseIO.Redacted` interface on any Activity that contains sensitive views. This interface contains one method:

```java
// From this method you should return a list of the views you want
// Cobrowse to redact, for example:
public List<View> redactedViews() {
    List<View> redacted = new ArrayList<>();
    redacted.add(findViewById(R.id.redact_me));
    return redacted;
}
```

If making changes to your `Activity` classes isn't an option, we also support a delegate style method to allow you to supply this information in one place. Implement `CobrowseIO.RedactionDelegate` interface in your `CobrowseIO.Delegate` class, then you can pass redacted views for a specific `Activity` in a single method:

```java
@Override
public List<View> redactedViews(@NonNull Activity activity) {
    List<View> redacted = new ArrayList<>();
    // Return a list of redacted views for a provided activity
    return redacted;
}
```

**Redaction of Jetpack Compose UI**

Redaction for Jetpack Compose UI is shipped in a separate library on Maven Central:

```
dependencies {
    // ... other dependencies ...
    implementation 'io.cobrowse:cobrowse-sdk-android:2.+'
    implementation 'io.cobrowse:cobrowse-sdk-android-compose-ui:2.+'
}
```

{% hint style="info" %}
You are required to use the same version of the Cobrowse.io SDK and Compose UI redaction artifacts. Using different versions of Cobrowse.io SDK artifacts is not supported.
{% endhint %}

Apply `Modifier.redacted()` to your composable to be redacted, like so:

```kotlin
import io.cobrowse.redacted

Text("Redacted label",
     modifier = Modifier
         .background(Color.Red)
         // Other modifiers...
         .redacted())
```

**Redaction by default**

Sometimes you may want to redact everything on the screen, then selectively "unredact" only the parts your support agents should be able to see. This is particularly useful on applications that require a higher privacy standard or where only specific sections of the App should be visible to the agent.

To achieve this, you'll need to follow the delegate implementation and ensure you pass the all your applications root views to the Cobrowse redaction delegate method:

```java
@Override
public List<View> redactedViews(@NonNull Activity activity) {
    return new ArrayList<View>() {{
        add(activity.getWindow().getDecorView());
    }};
}
```

Once you've done this, your application root views will be redacted by default and you'll be able to un-redact child components to make them visible to the agents by implementing `CobrowseIO.UnredactionDelegate` in your `CobrowseIO.Delegate` class:

```java
@Override
public List<View> unredactedViews(@NonNull Activity activity) {
    return new ArrayList<View>(){{
        if (findViewById(R.id.view_to_be_unredacted) != null)
            add(findViewById(R.id.view_to_be_unredacted));
    }};
}
```

Alternatively, you can implement `CobrowseIO.Unredacted` interface in your `Activity` subclasses:

```java
@Override
public List<View> unredactedViews() {
    return new ArrayList<View>(){{
        if (findViewById(R.id.view_to_be_unredacted) != null)
            add(findViewById(R.id.view_to_be_unredacted));
    }};
}
```

**Redaction of WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `webviewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```java
CobrowseIO.instance().webviewRedactedViews(new String[] { ".redacted",  ...some other selectors... });
```
{% endtab %}

{% tab title="React Native" %}
To redact an element in your RN application you can wrap it in a tag provided by the Cobrowse module:

```javascript
import { Redacted } from 'cobrowse-sdk-react-native';

// ...
<View style={styles.container}>
    <Redacted>
        <Text style={styles.instructions}>This text should be secret</Text>
    </Redacted>
</View>
```

**Redaction by default**

Sometimes you may want to redact everything on the screen, then selectively "unredact" only the parts your support agents should be able to see. This is particularly useful on applications that require a higher privacy standard or where only specific sections of the App should be visible to the agent.

To achieve this, you'll need to follow the [delegate implementation](listening-for-events.md#session-updated-events) and ensure you pass the all your applications root views to the Cobrowse redaction delegate methods for iOS and Android.

{% content-ref url="listening-for-events.md" %}
[listening-for-events.md](listening-for-events.md)
{% endcontent-ref %}

Once you've done this, your application root views will be redacted by default and you'll be able to un-redact child components to make them visible to the agents:

```javascript
import { Unredacted } from 'cobrowse-sdk-react-native';
// ...

<View style={styles.container}>
    <Text>This text should be secret due to the redaction by default implementation</Text>
    <Unredacted>
        <Text>This text will be visible to the agents because it's wrapped with an Unredacted component</Text>
        <Redacted>
            <Text>You can also nest redacted elements inside unredacted ones. This text will be hidden to the agents.</Text>
        </Redacted>
    </Unredacted>
</View>
```

**Redaction of WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `webviewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```javascript
CobrowseIO.webviewRedactedViews = [ '.redacted', ...some other selectors... ];
```

**Redacting views outside React Native**

Finally, within React Native some packages will render in new Windows/Root Views. You can follow the same [delegate implementation](listening-for-events.md#session-updated-events) to identify this views and redact or unredact them by default as required. You can check the provided examples for [iOS](https://github.com/cobrowseio/cobrowse-sdk-react-native/blob/master/Example/ios/Example/AppDelegate.m) and [Android](https://github.com/cobrowseio/cobrowse-sdk-react-native/blob/master/Example/android/app/src/main/java/com/example/MainApplication.java) which redact by default the React Native Dev menu keeping one of the options unredacted to illustrate the technique.
{% endtab %}

{% tab title="Flutter" %}
To redact an element in your Flutter application you can wrap it in a widget provided by the SDK:

```dart
Redacted(child: TextField(/* TextField properties */))
```

{% hint style="info" %}
The widget will be redacted for both client and agent during an active screensharing session. Both client and agent will not be able to interact with it during a session.
{% endhint %}
{% endtab %}

{% tab title=".NET Mobile" %}
**iOS implementation**

Implement the `ICobrowseIORedacted` interface on any `UIViewController` that contains sensitive views. This interface contains one `RedactedViews` property:

```csharp
public partial class ViewController : UIViewController, ICobrowseIORedacted
{
    // From this property you should return a list of the views you want Cobrowse to redact, for example:
    public UIView[] RedactedViews
        => new[] { redactedTextView };
}
```

**Android implementation**

Implement the `CobrowseIO.IRedacted` interface on any `Activity` that contains sensitive views. This interface contains one method:

```csharp
[Activity]
public class MainActivity : AppCompatActivity, CobrowseIO.IRedacted
{
    // From this method you should return a list of the views you want Cobrowse to redact, for example:
    public IList<View> RedactedViews()
    {
        return new[]
        {
            FindViewById(Resource.Id.redact_me)
        }
    }
}
```

**MAUI implementation**

Create a new [`Effect`](https://learn.microsoft.com/en-us/dotnet/maui/migration/effects?view=net-maui-8.0) which then will be used to redact certain MAUI views:

```csharp
public class CobrowseRedactedViewEffect : RoutingEffect
{
    public bool IsRedacted { get; set; } = true;
}
```

Create the following _iOS-specific_ classes:

```csharp
public class CobrowseRedactionDelegate
    : Cobrowse.IO.CobrowseDelegateImplementation
{
    public override UIView[] RedactedViewsForViewController(UIViewController vc)
        => PlatformCobrowseRedactedViewEffect.RedactedViews;
}

public class PlatformCobrowseRedactedViewEffect : PlatformEffect
{
    private static readonly List<UIView> sRedacted = new List<UIView>();

    public static UIView[] RedactedViews => sRedacted.ToArray();

    public PlatformCobrowseRedactedViewEffect()
    {
    }

    protected override void OnAttached()
    {
        AddToRedacted(Container);
    }

    protected override void OnDetached()
    {
        RemoveFromRedacted(Container);
    }

    private static void AddToRedacted(UIView view)
    {
        if (view == null)
        {
            return;
        }
        sRedacted.Add(view);
    }

    private static void RemoveFromRedacted(UIView view)
    {
        if (view == null)
        {
            return;
        }
        if (sRedacted.Contains(view))
        {
            sRedacted.Remove(view);
        }
    }
}
```

_Android-specific_ classes:

```csharp
public class CobrowseRedactionDelegate
    : Cobrowse.IO.CobrowseDelegateImplementation,
    Cobrowse.IO.Android.CobrowseIO.IRedactionDelegate
{
    public IList<AView>? RedactedViews(Activity activity)
        => PlatformCobrowseRedactedViewEffect.RedactedViews;
}

public class PlatformCobrowseRedactedViewEffect : PlatformEffect
{
    public PlatformCobrowseRedactedViewEffect()
    {
    }
    private static readonly List<AView> sRedacted = new List<AView>();

    public static IList<AView> RedactedViews => sRedacted;

    protected override void OnAttached()
    {
        AddToRedacted(Control ?? Container);
    }

    protected override void OnDetached()
    {
        RemoveFromRedacted(Control ?? Container);
    }

    private static void AddToRedacted(AView view)
    {
        if (view == null)
        {
            return;
        }
        sRedacted.Add(view);
    }

    private static void RemoveFromRedacted(AView view)
    {
        if (view == null)
        {
            return;
        }
        if (sRedacted.Contains(view))
        {
            sRedacted.Remove(view);
        }
    }
}
```

Make sure the delegates you just created are passed into the Cobrowse.io SDK after the SDK is started:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        CobrowseIO.Instance.License = "<your license key>";
        CobrowseIO.Instance.Start();

#if ANDROID
        Cobrowse.IO.Android.CobrowseIO.Instance.SetDelegate(new YourNamespace.Platforms.Android.CobrowseRedactionDelegate());
#elif IOS
        Cobrowse.IO.iOS.CobrowseIO.Instance.SetDelegate(new YourNamespace.Platforms.iOS.CobrowseRedactionDelegate());
#endif
    }
```

Last, utilize the created effect like this:

```xml
<ContentPage xmlns:local="clr-namespace:YourNamespace">
...
    <Label Text="This label is redacted">
        <Label.Effects>
            <local:CobrowseRedactedViewEffect />
        </Label.Effects>
    </Label>
...
</ContentPage>
```

**Redaction of WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `WebViewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```csharp
CobrowseIO.Instance.WebViewRedactedViews = new string[] { ".redacted", ...some other selectors... };
```
{% endtab %}
{% endtabs %}

### **2. Selector based redaction**

You can use CSS like selectors to identify what elements / views should be redacted. These selectors can be defined within your application using our SDK or via the Cobrowse dashboard.

Adding selectors via the dashboard can be useful if your app is already in production and you need to redact a field retrospectively, either due to a missed redaction entry in the app build or changing requirements.

{% tabs %}
{% tab title="Web" %}
**Via SDK**

```
CobrowseIO.redactedViews = ['.redacted', ...some other selectors...]
```

{% hint style="info" %}
The full CSS selector standard is supported with nesting and complex comparators.
{% endhint %}

**Via dashboard**

Visit [https://cobrowse.io/dashboard/settings/redaction](https://cobrowse.io/dashboard/settings/redaction) and enter your selectors into the Web redaction / unredaction configuration.
{% endtab %}

{% tab title="iOS" %}
**Via SDK**

For UIKit you can use the class name of any `UIView`, the view's integer tag value or any property on the `UIView` that is string representable with no additional configuration.

```swift
let cobrowse = CobrowseIO.instance()
cobrowse.redactedViews = [ 
    "UIButton",
    "MyCustomView UILabel#2[text=Hello]",
    "[accessibilityIdentifier=\"HELLO MESSAGE\"]"
]
```

{% hint style="info" %}
* Nested selectors are supported
* Only the `=` comparator is supported
{% endhint %}

For SwiftUI you will need to use the `cobrowseSelector(tag:, id:, classes:, attributes:)` view modifier filling our the values manually.

```swift
Text("Hello, Frank")
    .accessibilityIdentifier("HELLO MESSAGE")
    .cobrowseSelector(tag: "Text", attributes: ["accessibilityIdentifier" : "HELLO MESSAGE"]) 
```

This view can now be referenced using the selector of:

`Text[accessibilityIdentifier="HELLO MESSAGE"]`

**Via dashboard**

Visit [https://cobrowse.io/dashboard/settings/redaction](https://cobrowse.io/dashboard/settings/redaction) and enter your selectors into the iOS redaction configuration.
{% endtab %}

{% tab title="Android" %}
**Via SDK**

For Android Views you can use the [simple name](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getSimpleName--) of any view class, the id of the view.

The `#id` must be able to be used with system `android.view.View#findViewById()` and `android.app.Activity#findViewById()` methods.

```java
CobrowseIO.instance().redactedViews(new String[] {
    "Button"
    "Label#123[contentDescription=Hello]",
    "[testTag=\"Hello Message\"]"
});
```

{% hint style="info" %}
* Nested selectors are **not** supported
* Only the `=` comparator is supported
{% endhint %}

For JetPack Compose UIs you will need to use the `cobrowseSelector(tag, id, attributes)` modifier filling our the values manually.

```kotlin
Text("Hello, Frank", modifier = Modifier
            .semantics { testTag = "HELLO MESSAGE" }
            .cobrowseSelector(
                tag = "Text",
                attributes = mapOf("testTag" to "HELLO MESSAGE")))
```

This view can now be referenced using the selector of:

`Text[testTag="HELLO MESSAGE"]`

**Via dashboard**

Visit [https://cobrowse.io/dashboard/settings/redaction](https://cobrowse.io/dashboard/settings/redaction) and enter your selectors into the iOS redaction configuration.
{% endtab %}
{% endtabs %}

#### **Redaction Playground**

Configuring redaction can often require deep technical involvement and coordination across multiple teams. This can make it harder to manage consistently and to adjust quickly as requirements change.&#x20;

The Redaction Playground provides a centralized environment to define, preview, and apply redaction rules, so configuration can be managed in one place without code changes or application releases.

{% embed url="https://player.vimeo.com/video/1114896755?amp;app_id=58479&amp;autopause=0&amp;badge=0&amp;player_id=0&h=eee519bb12" %}

Once you have your updated redaction configuration please copy them over to your [redaction settings](https://cobrowse.io/dashboard/settings/redaction) of your Cobrowse account.

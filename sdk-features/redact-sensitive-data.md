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

**1. Define the redacted views in your app source code (recommended)**

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

#### Unredaction

Our web SDK also supports an un-redaction mechanism, where by you can define sub-elements inside of a redacted element that should be visible to the agent. You can specify un-redaction selectors like this:

```javascript
CobrowseIO.unredactedViews = ['.unredacted', ...some other selectors...]
```
{% endtab %}

{% tab title="iOS" %}
Implement the `CobrowseIORedacted` protocol on any `UIViewController` that contains sensitive views. This protocol contains one method:

```objectivec
// From this method you should return a list of the views you want
// Cobrowse to redact, for example:
- (NSArray*) redactedViews {
    return @[ redactedTextView ];
}
```

If making changes to your `UIViewController` subclasses isn't an option, we also support a [delegate style](listening-for-events.md) method to allow you to supply this information in one place. Implement `cobrowseRedactedViewsForViewController` in your `CobrowseIODelegate` class, then you can pass redacted views for a specific `UIViewController` in a single method:

```objc
-(NSArray<UIView*>*) cobrowseRedactedViewsForViewController:(UIViewController*) vc {
    NSMutableArray<UIView*>* redacted = [[NSMutableArray alloc] init];
    // Return a list of redacted views for a provided UIViewController
    return redacted;
}
```

**Redaction of SwiftUI views**

To redact any SwiftUI view please use the `.redacted()` view modifier supplied by the Cobrowse iOS SDK v2.30.0 +.

The example below will redact the `Text` view that displays the email address.

```swift
struct AccountView: View {
    
    let name: String
    let email: String
    
    var body: some View {
        VStack {
            Text(name)
                .font(.largeTitle)
            
            Text(verbatim: email)
                .font(.title2)
                .redacted()
        }
    }
}
```

You can add the `.redacted()` view modifier to views that contain child views like `VStack` and the entire stack will be redacted.

```swift
struct AccountView: View {
    
    let name: String
    let email: String
    
    var body: some View {
        VStack {
            Text(name)
                .font(.largeTitle)
            
            Text(verbatim: email)
                .font(.title2)
        }
        .redacted()
    }
}
```

**Redaction by default**

Sometimes you may want to redact everything on the screen, then selectively "unredact" only the parts your support agents should be able to see. This is particularly useful on applications that require a higher privacy standard or where only specific sections of the App should be visible to the agent.

To achieve this, you'll need to follow the delegate implementation and ensure you pass the all your applications root views to the Cobrowse redaction delegate method:

```objc
-(NSArray<UIView*>*) cobrowseRedactedViewsForViewController:(UIViewController*) vc {
    return @[self.window.rootViewController.view];
}
```

Once you've done this, your application root views will be redacted by default and you'll be able to un-redact child components to make them visible to the agents by implementing `cobrowseUnredactedViewsForViewController` in your `CobrowseIODelegate` class:

```objc
-(NSArray<UIView*>*) cobrowseUnredactedViewsForViewController:(UIViewController *) vc {
    UIView *unredacted = [self findViewByAccessibilityLabel:self.window:@"viewToBeUnredacted"];
    if (unredacted) {
        return @[unredacted];
    }
    return @[];
}
```

Alternatively, you can implement `CobrowseIOUnredacted` protocol in your `UIViewController` subclasses:

```objc
-(NSArray*) unredactedViews {
    return @[viewToBeUnredacted];
}
```

**Redaction of WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `webviewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```objc
CobrowseIO.instance.webviewRedactedViews = @[ @".redacted", ...some other selectors... ];
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

**2. Use the Cobrowse web dashboard to define redacted views**

{% hint style="warning" %}
This mechanism is provided as a fallback, use the SDK APIs when possible for maximum resiliency and efficiency.
{% endhint %}

You can also define redactions using a selector entered into the web dashboard. This can be useful if your app is already in production and you need to redact a field retrospectively, either due to a missed redaction entry in the app build or changing requirements. Visit the [dashboard settings](https://cobrowse.io/dashboard/settings/redaction) to enter redaction selectors.

{% tabs %}
{% tab title="Web" %}
Enter your css selectors, e.g. `.redacted-class` or `#redacted-id`.
{% endtab %}

{% tab title="iOS" %}
Enter your objective-C class name and/or view id, e.g. `ClassName#id` or just `#id`.

`ClassName` is the name of the UI class, and `id` is the integer tag of the view object.
{% endtab %}

{% tab title="Android" %}
Enter your tag of the view class and/or the identifier name for the view, e.g. `tag#id` or just `#id`.

`tag` is the [simple name](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getSimpleName--) of the view class, `#id` can usually be found in the XML layout like so `android:id="@+id/here_is_the_id`. The `#id` must be able to be used with system `android.view.View#findViewById()` and `android.app.Activity#findViewById()` methods.
{% endtab %}
{% endtabs %}

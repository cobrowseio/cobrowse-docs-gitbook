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



**Redacting views outside React Native**

Finally, within React Native some packages will render in new Windows/Root Views. You can follow the same [delegate implementation](listening-for-events.md#session-updated-events) to identify this views and redact or unredact them by default as required. You can check the provided examples for [iOS](https://github.com/cobrowseio/cobrowse-sdk-react-native/blob/master/Example/ios/Example/AppDelegate.m) and [Android](https://github.com/cobrowseio/cobrowse-sdk-react-native/blob/master/Example/android/app/src/main/java/com/example/MainApplication.java) which redact by default the React Native Dev menu keeping one of the options unredacted to illustrate the technique.
{% endtab %}

{% tab title="Xamarin" %}
**Xamarin.iOS implementation**

Implement the `ICobrowseIORedacted` interface on any `UIViewController` that contains sensitive views. This interface contains one `RedactedViews` property:

```csharp
public partial class ViewController : UIViewController, ICobrowseIORedacted
{
    // From this property you should return a list of the views you want Cobrowse to redact, for example:
    public UIView[] RedactedViews
        => new[] { redactedTextView };
}
```

**Xamarin.Android implementation**

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

**Xamarin.Forms implementation**

While it is not possible to access platform-specific UI code directly from a cross-platform project, you can easily achieve it using [Effects](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/effects/introduction) and [Custom Renderers](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/custom-renderer/).

In your **cross-platform** project declare a new `Effect` for marking Xamarin.Forms UI elements as _redacted_:

```csharp
using Xamarin.Forms;

namespace YourAppNamespace.Forms
{
    /// <summary>
    /// A Xamarin.Forms effect that helps to redact Xamarin.Forms view in Cobrowse.io.
    /// </summary>
    public class CobrowseRedactedViewEffect : RoutingEffect
    {
        public bool IsRedacted { get; set; } = true;

        public CobrowseRedactedViewEffect()
            : base("YourAppName" + "." + nameof(CobrowseRedactedViewEffect))
        {
        }
    }
}
```

**iOS-specific** platform Effect would look like this:

```csharp
using System.Collections.Generic;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ResolutionGroupName("YourAppName")]
[assembly: ExportEffect(typeof(YourAppNamespace.iOS.PlatformCobrowseRedactedViewEffect), "CobrowseRedactedViewEffect")]
namespace YourAppNamespace.iOS
{
    public class PlatformCobrowseRedactedViewEffect : PlatformEffect
    {
        private static readonly List<UIView> sRedacted = new List<UIView>();

        public static UIView[] RedactedViews => sRedacted.ToArray();

        public PlatformCobrowseRedactedViewEffect()
        {
        }

        protected override void OnAttached()
        {
            // We have to always use 'Container' and never 'Control'
            // because 'Control' is null in 'OnDetached', at least in Xamarin.Forms 4.5.0.356
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
}
```

Then create a new default `Xamarin.Forms.Page` renderer which would implement `ICobrowseIORedacted` interface:

```csharp
using UIKit;
using Xamarin.CobrowseIO;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourAppNamespace.iOS.CobrowseRedactedPageRenderer))]
namespace YourAppNamespace.iOS
{
    public class CobrowseRedactedPageRenderer : PageRenderer, ICobrowseIORedacted
    {
        public UIView[] RedactedViews => PlatformCobrowseRedactedViewEffect.RedactedViews;
    }
}
```

**Android-specific** platform Effect would look like this:

```csharp
using System.Collections.Generic;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;
using AView = Android.Views.View;

[assembly: ResolutionGroupName("YourAppName")]
[assembly: ExportEffect(typeof(YourAppNamespace.Android.PlatformCobrowseRedactedViewEffect), "CobrowseRedactedViewEffect")]
namespace YourAppNamespace.Android
{
    public class PlatformCobrowseRedactedViewEffect : PlatformEffect
    {
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
}
```

Then implement `CobrowseIO.IRedacted` interface in your Forms activity:

```csharp
using System.Collections.Generic;
using Android.App;
using Xamarin.CobrowseIO;
using AView = Android.Views.View;

namespace YourAppNamespace.Android
{
    [Activity]
    public class MainActivity
        : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity,
        CobrowseIO.IRedacted
    {
        public IList<AView> RedactedViews()
        {
            return PlatformCobrowseRedactedViewEffect.RedactedViews;
        }
    }
}
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


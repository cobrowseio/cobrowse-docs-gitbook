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

If making changes to your `UIViewController` subclasses isn't an option, we also support a [delegate style](listening-for-events.md) method to allow you to supply this information in one place. Find out more about this by emailing us at [hello@cobrowse.io](https://github.com/cobrowseio/cobrowse-docs-gitbook/tree/91f22fdcf9e3124fc8f6f9877feb64cda4b8af39/sdk-features/hello@cobrowse.io).
{% endtab %}

{% tab title="Android" %}
Implement the CobrowseIO.Redacted interface on any Activity that contains sensitive views. This interface contains one method:

```java
// From this method you should return a list of the views you want
// Cobrowse to redact, for example:
public List<View> redactedViews() {
    List<View> redacted = new ArrayList<>();
    redacted.add(findViewById(R.id.redact_me));
    return redacted;
}
```

If making changes to your Activity classes isn't an option, we also support a delegate style method to allow you to supply this information in one place. Find out more about this by emailing us at [hello@cobrowse.io](https://github.com/cobrowseio/cobrowse-docs-gitbook/tree/91f22fdcf9e3124fc8f6f9877feb64cda4b8af39/sdk-features/hello@cobrowse.io).
{% endtab %}

{% tab title="React Native" %}
To redact an element in your React native application you can wrap it in a tag provided by the Cobrowse module:

```javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';
import { Redacted } from 'cobrowse-sdk-react-native';

export default class MyComponent extends Component {
    render() {
        return (
            <View style={styles.container}>
                <Redacted>
                    <Text style={styles.instructions}>This text should be secret</Text>
                </Redacted>
            </View>
        );
    }
}

// ... stylesheets etc...
```
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

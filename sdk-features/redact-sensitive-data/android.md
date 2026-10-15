# Android

We provide two methods to redact views.

### **1. Delegate based redaction**

Redacting views via your source code ensures your redaction configuration are tied directly to the application structure.

#### **Redact per-view**

To redact a single view you can call `cobrowseRedacted()` on any `View`, or apply `Modifier.cobrowseRedacted()` to a composable. Any children of the view are redacted too, so this also works for containers such as a `LinearLayout` or a `Column`.

{% tabs %}
{% tab title="Android Views" %}
```kotlin
import io.cobrowse.cobrowseRedacted

findViewById<View>(R.id.card_number).cobrowseRedacted()
```

The extension function is Kotlin only and requires SDK 3.18.0 or later.
{% endtab %}

{% tab title="Jetpack Compose" %}
Redaction for Jetpack Compose UI is shipped in a separate library on Maven Central:

```
dependencies {
    // ... other dependencies ...
    implementation 'io.cobrowse:cobrowse-sdk-android:3.+'
    implementation 'io.cobrowse:cobrowse-sdk-android-compose-ui:3.+'
}
```

{% hint style="info" %}
You are required to use the same version of the Cobrowse.io SDK and Compose UI redaction artifacts. Using different versions of Cobrowse.io SDK artifacts is not supported.
{% endhint %}

Apply `Modifier.cobrowseRedacted()` to your composable to be redacted, like so:

```kotlin
import io.cobrowse.cobrowseRedacted

Text("Redacted label",
     modifier = Modifier
         .background(Color.Red)
         // Other modifiers...
         .cobrowseRedacted())
```
{% endtab %}
{% endtabs %}

#### **Redact views within Activity via** `CobrowseIO.Redacted`

Implement the `CobrowseIO.Redacted` interface on any Activity that contains sensitive views. This interface contains one method:

```kotlin
// From this method you should return a list of the views you want
// Cobrowse to redact, for example:
override fun redactedViews(): List<View> {
    val redacted = mutableListOf<View>()
    redacted.add(findViewById(R.id.redact_me))
    return redacted
}
```

#### **Redact views within custom delegate via** `CobrowseIO.RedactionDelegate`

If making changes to your `Activity` classes isn't an option, we also support a delegate style method to allow you to supply this information in one place. Implement `CobrowseIO.RedactionDelegate` interface in your `CobrowseIO.Delegate` class, then you can return the redacted views for the whole app in a single method:

```kotlin
import androidx.core.view.allViews
import io.cobrowse.rootViews

override fun redactedViews(app: Application): List<View> {
    val redactedIds = setOf(R.id.redact_me, R.id.card_number)
    return app.rootViews()
        .asSequence()
        .flatMap { it.allViews }
        .filter { it.id in redactedIds }
        .toList()
}
```

Or individually per activity:

```kotlin
override fun redactedViews(activity: Activity): List<View> {
    val redacted = mutableListOf<View>()
    // Return a list of redacted views for a provided activity
    return redacted
}
```

#### **Redact WebView content**

Your app may show web content that contains elements that you wish to redact. Pass `webviewRedactedViews` an array of CSS selectors that identify the elements to be redacted. These selectors apply to every page loaded in any `WebView` in your app:

```kotlin
CobrowseIO.instance().webviewRedactedViews(arrayOf(".redacted", /* ...some other selectors... */))
```

To apply selectors only to pages whose URL matches a glob pattern, pass the pattern as the first argument:

```kotlin
CobrowseIO.instance().webviewRedactedViews("*/checkout.html", arrayOf(".card-number"))
```

Elements inside a redacted region can be made visible again with `webviewUnredactedViews`, which takes the same arguments:

```kotlin
CobrowseIO.instance().webviewUnredactedViews(arrayOf(".order-total"))
```

All of these must be called before `CobrowseIO.instance().start()`.

### **2. Selector based redaction**

You can use CSS-like selectors to identify which views should be redacted. These selectors can be defined within your application using our SDK or via the Cobrowse dashboard.

#### **Via SDK**

{% tabs %}
{% tab title="Android Views" %}
You can use the [simple name](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getSimpleName--) of any view class, the resource name of the view's id, or one of the supported view attributes: `id`, `contentDescription`, `tag`, `text`, `hint`, `enabled`, `checked`, `clickable`, `inputType` and `error`.

The class name is that of the view's runtime class, which may differ from the tag in your layout XML. Under an AppCompat theme a `<Button>` is inflated as `AppCompatButton`, and under a Material theme as `MaterialButton`, so those are the names to use in a selector.

The `#id` is the resource entry name, so a view with `android:id="@+id/card_number"` is matched by `#card_number`. Ids assigned in code with `setId()` or `View.generateViewId()` have no resource name and cannot be matched.

```kotlin
CobrowseIO.instance().redactedViews(arrayOf(
    "Button",
    "TextView#card_number[contentDescription=Hello]",
    "[tag=\"Hello Message\"]",
    "PaymentCardView TextView"
))
```
{% endtab %}

{% tab title="Jetpack Compose" %}
You will need to use the `cobrowseSelector(tag, id, attributes)` modifier filling our the values manually.

```kotlin
Text("Hello, Frank", modifier = Modifier
            .semantics { testTag = "HELLO MESSAGE" }
            .cobrowseSelector(
                tag = "Text",
                attributes = mapOf("testTag" to "HELLO MESSAGE")))
```

This view can now be referenced using the selector of:

`Text[testTag="HELLO MESSAGE"]`
{% endtab %}
{% endtabs %}

{% hint style="info" %}
* Nested selectors are supported
* Only the `=` comparator is supported
{% endhint %}

#### **Via dashboard**

Visit [https://cobrowse.io/dashboard/settings/redaction](https://cobrowse.io/dashboard/settings/redaction) and enter your selectors into the Android redaction configuration.

### **Private by Default**

Sometimes you may want to redact everything on the screen, then selectively "unredact" only the parts your support agents should be able to see. This is particularly useful on applications that require a higher privacy standard or where only specific sections of the App should be visible to the agent.

To achieve this, you'll need to follow the delegate implementation and ensure you pass the all your applications root views to the `CobrowseIO.RedactionDelegate` delegate method:

```kotlin
import io.cobrowse.rootViews

override fun redactedViews(app: Application): List<View> {
    return app.rootViews()
}
```

Once you've done this, your application root views will be redacted by default and you'll be able to un-redact child components to make them visible to the agents by implementing `CobrowseIO.UnredactionDelegate` in your `CobrowseIO.Delegate` class:

```kotlin
import androidx.core.view.allViews
import io.cobrowse.rootViews

override fun unredactedViews(app: Application): List<View> {
    val unredactedIds = setOf(R.id.view_to_be_unredacted)
    return app.rootViews()
        .asSequence()
        .flatMap { it.allViews }
        .filter { it.id in unredactedIds }
        .toList()
}
```

Or individually per activity:

```kotlin
override fun unredactedViews(activity: Activity): List<View> {
    return listOfNotNull(
        activity.findViewById<View>(R.id.view_to_be_unredacted)
    )
}
```

You can also find and unredact views of certain types, such as web views:

```kotlin
import androidx.core.view.allViews

override fun unredactedViews(app: Application): List<View> {
    val unredactedClasses = listOf(
        androidx.compose.ui.platform.ComposeView::class,
        io.flutter.embedding.android.FlutterView::class,
        android.webkit.WebView::class)
    return app.rootViews()
        .asSequence()
        .flatMap { it.allViews }
        .filter { view ->
            unredactedClasses.any { it.isInstance(view) }
        }
        .toList()
}
```

From Kotlin, you can also call `cobrowseUnredacted()` on the view itself. The view and its ancestors become visible to the agent while the other children of those ancestors stay redacted:

```kotlin
import io.cobrowse.cobrowseUnredacted

findViewById<View>(R.id.view_to_be_unredacted).cobrowseUnredacted()
```

Alternatively, you can implement `CobrowseIO.Unredacted` interface in your `Activity` subclasses:

```kotlin
override fun unredactedViews(): List<View> {
    return listOfNotNull(
        findViewById<View>(R.id.view_to_be_unredacted)
    )
}
```

### Redaction Playground

To explore and modify redaction in your apps you can use the Redaction Playground.

{% content-ref url="redaction-playground.md" %}
[redaction-playground.md](redaction-playground.md)
{% endcontent-ref %}

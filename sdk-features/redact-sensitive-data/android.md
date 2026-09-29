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

The extension function is Kotlin only and requires SDK 3.18.0 or later. From Java, use one of the interfaces below instead.
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

```java
// From this method you should return a list of the views you want
// Cobrowse to redact, for example:
public List<View> redactedViews() {
    List<View> redacted = new ArrayList<>();
    redacted.add(findViewById(R.id.redact_me));
    return redacted;
}
```

#### **Redact views within custom delegate via** `CobrowseIO.RedactionDelegate`

If making changes to your `Activity` classes isn't an option, we also support a delegate style method to allow you to supply this information in one place. Implement `CobrowseIO.RedactionDelegate` interface in your `CobrowseIO.Delegate` class, then you can pass redacted views for a specific `Activity` in a single method:

```java
@Override
public List<View> redactedViews(@NonNull Activity activity) {
    List<View> redacted = new ArrayList<>();
    // Return a list of redacted views for a provided activity
    return redacted;
}
```

#### **Redact WebView content**

Your app may show web content that contains elements that you wish to redact. Pass `webviewRedactedViews` an array of CSS selectors that identify the elements to be redacted. These selectors apply to every page loaded in any `WebView` in your app:

```java
CobrowseIO.instance().webviewRedactedViews(new String[] { ".redacted", ...some other selectors... });
```

To apply selectors only to pages whose URL matches a glob pattern, pass the pattern as the first argument:

```java
CobrowseIO.instance().webviewRedactedViews("*/checkout.html", new String[] { ".card-number" });
```

Elements inside a redacted region can be made visible again with `webviewUnredactedViews`, which takes the same arguments:

```java
CobrowseIO.instance().webviewUnredactedViews(new String[] { ".order-total" });
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

```java
CobrowseIO.instance().redactedViews(new String[] {
    "Button",
    "TextView#card_number[contentDescription=Hello]",
    "[tag=\"Hello Message\"]",
    "PaymentCardView TextView"
});
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

From Kotlin, you can also call `cobrowseUnredacted()` on the view itself. The view and its ancestors become visible to the agent while the other children of those ancestors stay redacted:

```kotlin
import io.cobrowse.cobrowseUnredacted

findViewById<View>(R.id.view_to_be_unredacted).cobrowseUnredacted()
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

### Redaction Playground

To explore and modify redaction in your apps you can use the Redaction Playground.

{% content-ref url="redaction-playground.md" %}
[redaction-playground.md](redaction-playground.md)
{% endcontent-ref %}

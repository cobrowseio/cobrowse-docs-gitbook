# Android

We provide two methods to redact views.

### **1. Delegate based redaction**

Redacting views via your source code ensures your redaction configuration are tied directly to the application structure.

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

#### **Redact Jetpack Compose UI**

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

#### **Redact WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `webviewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```java
CobrowseIO.instance().webviewRedactedViews(new String[] { ".redacted",  ...some other selectors... });
```

### **2. Selector based redaction**

You can use CSS-like selectors to identify which views should be redacted. These selectors can be defined within your application using our SDK or via the Cobrowse dashboard.

#### **Via SDK**

{% tabs %}
{% tab title="Android Views" %}
You can use the [simple name](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getSimpleName--) of any view class, the id of the view.

The `#id` must be able to be used with system `android.view.View#findViewById()` and `android.app.Activity#findViewById()` methods.

```java
CobrowseIO.instance().redactedViews(new String[] {
    "Button"
    "Label#123[contentDescription=Hello]",
    "[testTag=\"Hello Message\"]"
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
* Nested selectors are **not** supported
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

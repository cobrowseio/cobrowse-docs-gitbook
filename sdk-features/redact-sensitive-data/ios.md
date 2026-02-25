# iOS

We provide two methods to redact views.

### **1. Delegate based redaction**

Redacting views via your source code ensures your redaction configuration are tied directly to the application structure.

#### Redact per-view

To redact a single view you can call `cobrowseRedacted` on any `UIView` or SwiftUI `View`.

{% tabs %}
{% tab title="UIKit" %}
```swift
let uiView = UIView()
uiView.cobrowseRedacted()
```
{% endtab %}

{% tab title="SwiftUI" %}
```swift
let swiftUIView = ExampleView()
swiftUIView.cobrowseRedacted()
```
{% endtab %}
{% endtabs %}

Any children of the view will also be redacted allowing you to redact container views like `VStack` or a `UIView` that contains subviews.

#### **Redact views within UIViewController via CobrowseIORedacted**

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

#### **Redact views within custom delegate via CobrowseIODelegate**

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

#### **Redact WebView content**

Your app may show web content that contains elements that you wish to redact. This can be achieved by setting the `webviewRedactedViews` property to an array of CSS selectors that identify the elements to be redacted.

```swift
let cobrowse = CobrowseIO.instance()
cobrowse.webviewRedactedViews = [ ".redacted", ...some other selectors... ]
```

### **2. Selector based redaction**

You can use CSS-like selectors to identify which views should be redacted. These selectors can be defined within your application using our SDK or via the Cobrowse dashboard.

#### **Via SDK**

Adding selectors via code ensures that list is tied to that version of the application.

{% tabs %}
{% tab title="UIKit" %}
You can use the class name of any `UIView`, the view's integer tag value or any property on the `UIView` that is string representable with no additional configuration.

```swift
let cobrowse = CobrowseIO.instance()
cobrowse.redactedViews = [ 
    "UIButton",
    "MyCustomView UILabel#2[text=Hello]",
    "[accessibilityIdentifier=\"HELLO MESSAGE\"]"
]
```
{% endtab %}

{% tab title="SwiftUI" %}
You will need to use the `cobrowseSelector(tag:, id:, classes:, attributes:)` view modifier filling our the values manually.

```swift
Text("Hello, Frank")
    .accessibilityIdentifier("HELLO MESSAGE")
    .cobrowseSelector(tag: "Text", attributes: ["accessibilityIdentifier" : "HELLO MESSAGE"]) 
```

This view can now be referenced using the selector of:

`Text[accessibilityIdentifier="HELLO MESSAGE"]`
{% endtab %}
{% endtabs %}

{% hint style="info" %}
* Nested selectors are supported
* Only the `=` comparator is supported
{% endhint %}

#### **Via dashboard**

Visit [https://cobrowse.io/dashboard/settings/redaction](https://cobrowse.io/dashboard/settings/redaction) and enter your selectors into the iOS redaction configuration.

Adding selectors via the dashboard can be useful if your app is already in production and you need to redact a field retrospectively, either due to a missed redaction entry in the app build or changing requirements.

### **Private by Default**

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

### Redaction Playground

To explore and modify redaction in your apps you can use the Redaction Playground.

{% content-ref url="redaction-playground.md" %}
[redaction-playground.md](redaction-playground.md)
{% endcontent-ref %}

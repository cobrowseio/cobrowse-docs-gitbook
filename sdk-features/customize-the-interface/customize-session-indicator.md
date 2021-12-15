# Active session indicator

By default, the Cobrowse SDKs will show a minimal visual indicator to the end-user that a session is active. Our default UI provides an easy way for the end-user to end the session at any time.

You can fully customize the interface for a Cobrowse session.

{% tabs %}
{% tab title="Web" %}
The SDK provides hooks for you to render your own interface:

```javascript
CobrowseIO.showSessionControls = function() {
    // your code, i.e. $('#cobrowse-control').show() or similar
}

CobrowseIO.hideSessionControls = function() {
    // your code, i.e. $('#cobrowse-control').hide() or similar
}
```

When you override these functions, we will not show any default UI for the end-user to end their session. You can then display your own button or other UI to allow your end-user to end their session.

When implementing your own UI, you can use the following javascript code to programatically end the Cobrowse session.

```javascript
if (CobrowseIO.currentSession) {
    CobrowseIO.currentSession.end();
}
```

Here's a simple example that re-create the default UI, but with a blue button and some different text:

```markup
<script>
CobrowseIO.client().then(function() {
    var button = document.createElement('div');
    button.className = '__cbio_ignored';
    button.textContent = 'End';
    button.style.fontFamily = 'sans-serif';
    button.style.padding = '10px 13px';
    button.style.fontSize = '13px';
    button.style.color = 'white';
    button.style.boxShadow = '0px 2px 5px #33333344';
    button.style.cursor = 'pointer';
    button.style.borderRadius = '30px';
    button.style.background = 'blue';
    button.style.position = 'fixed';
    button.style.zIndex = '2147483647';
    button.style.bottom = '20px';
    button.style.left = '50%';
    button.style.transform = 'translateX(-50%)';
    button.addEventListener('click', function() {
        if (CobrowseIO.currentSession) CobrowseIO.currentSession.end();
    });

    CobrowseIO.showSessionControls = function() {
        document.body.appendChild(button);
    }

    CobrowseIO.hideSessionControls = function() {
        if (button.parentNode) button.parentNode.removeChild(button);
    }
});
</script>
```
{% endtab %}

{% tab title="iOS" %}
The SDK provides hooks via `CobrowseIODelegate` for you to render your own interface:

### **Swift**

```swift
import CobrowseIO

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate, CobrowseIODelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        CobrowseIO.instance().delegate = self
        // ... the rest of your app setup
        return true
    }

    func cobrowseShowSessionControls(_ session: CBIOSession) {
        // You can render controls however you like here.
        // One option is to add our sample end session UI defined below.
        if (indicatorInstance == nil) {
            indicatorInstance = self.defaultSessionIndicator(container: UIApplication.shared.keyWindow!)
        }
        indicatorInstance?.isHidden = false
    }

    func cobrowseHideSessionControls(_ session: CBIOSession) {
        indicatorInstance?.isHidden = true
    }

    // sample end session UIView, constructor, and tap gesture recognizer implementation
    var indicatorInstance : UIView? = nil
    func defaultSessionIndicator(container: UIView) -> UIView {
        let indicator : UILabel = UILabel()
        indicator.backgroundColor = UIColor(red: 1.0, green: 0.0, blue: 0.0, alpha: 0.7)
        indicator.text = "End Session"
        indicator.isUserInteractionEnabled = true
        indicator.textAlignment = .center
        indicator.font.withSize(UIFont.smallSystemFontSize)
        indicator.textColor = .white
        indicator.layer.cornerRadius = 10
        indicator.clipsToBounds = true
        indicator.translatesAutoresizingMaskIntoConstraints = false
        container.addSubview(indicator)

        indicator.widthAnchor.constraint(equalToConstant: 200).isActive = true
        indicator.heightAnchor.constraint(equalToConstant: 40).isActive = true
        indicator.centerXAnchor.constraint(equalTo: container.centerXAnchor).isActive = true
        indicator.bottomAnchor.constraint(equalTo: container.bottomAnchor, constant: -20).isActive = true

        let tapRecognizer : UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(indicatorTapped(_:)))
        tapRecognizer.numberOfTapsRequired = 1
        indicator.addGestureRecognizer(tapRecognizer)
        return indicator
    }
    @objc
    func indicatorTapped(_ sender: UITapGestureRecognizer) {
        CobrowseIO.instance().currentSession()?.end(nil)
    }

}
```

### **Objective C**

```objectivec
@implementation CBAppDelegate // should implement CobrowseIODelegate

- (BOOL)application:(UIApplication*) application didFinishLaunchingWithOptions:(NSDictionary*) launchOptions {
    CobrowseIO.instance.delegate = self;
    // ... the rest of your app setup
    return YES;
}

- (void)cobrowseShowSessionControls:(CBIOSession*) session {
    // You can render controls however you like here. One option is to add a floating
    // control visible over all other controls by adding it as a subview of the keyWindow
    [UIApplication.sharedApplication.keyWindow addSubview: self.myCustomCobrowseView];
}

- (void)cobrowseHideSessionControls:(CBIOSession*) session {
    [self.myCustomCobrowseView removeFromSuperview];
}

@end
```
{% endtab %}

{% tab title="Android" %}
The SDK provides hooks via `CobrowseIO.SessionControlsDelegate` for you to render your own interface:

```java
package com.example;

import io.cobrowse.CobrowseIO;

public class MainApplication extends Application implements CobrowseIO.SessionControlsDelegate {

    @Override
    public void onCreate() {
        super.onCreate();
        CobrowseIO.instance().setDelegate(this);

        // and the rest of cobrowse setup ...
    }

    @Override
    public void showSessionControls(final Activity activity, final Session session) {
        // optionally show your own controls here
    }

    @Override
    public void hideSessionControls(final Activity activity, final Session session) {
        // hide controls created by the method above here
    }

    //...

}
```
{% endtab %}

{% tab title="React Native" %}
The SDK provides hooks for you to render your own interface:

```javascript
import { SessionControl } from 'cobrowse-sdk-react-native';

export default class App extends Component {
    render() {
        return (
            <View>
                <SessionControl>
                    <Text>This will only show when a session is active!</Text>
                </SessionControl>
            </View>
        );
    }
}
```

When implementing your own UI, you can use the following javascript code to programatically end the Cobrowse session.

```javascript
// Get a reference to the current session if you don't have one
const session = await CobrowseIO.currentSession();
// if there's an ongoing session, end it
if (session) await session.end()
```


{% endtab %}

{% tab title="Xamarin" %}
The SDK provides hooks via `CobrowseIODelegate` for you to render your own interface:

### Xamarin.iOS implementation

```csharp
using Xamarin.CobrowseIO;

[Register("AppDelegate")]
public class AppDelegate : UIResponder, IUIApplicationDelegate
{
    [Export("application:didFinishLaunchingWithOptions:")]
    public bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        CobrowseIO.Instance.SetDelegate(new CustomCobrowseDelegate());
        // ... the rest of your app setup
        return true;
    }
}

public class CustomCobrowseDelegate : CobrowseIODelegate
{
    // Sample end session UIView, constructor, and tap gesture recognizer implementation
    private UIView _indicatorInstance;

    public override void CobrowseShowSessionControls(Session session)
    {
        // You can render controls however you like here.
        // One option is to add our sample end session UI defined below.
        if (_indicatorInstance == null)
        {
            _indicatorInstance = GetDefaultSessionIndicator(container: UIApplication.SharedApplication.KeyWindow);
        }
        _indicatorInstance.Hidden = false;
    }

    public override void CobrowseHideSessionControls(Session session)
    {
        if (_indicatorInstance != null)
            _indicatorInstance.Hidden = true;
    }

    private UIView GetDefaultSessionIndicator(UIView container)
    {
        var indicator = new UILabel();
        indicator.BackgroundColor = new UIColor(red: 1.0f, green: 0.0f, blue: 0.0f, alpha: 0.7f);
        indicator.Text = "End Session";
        indicator.UserInteractionEnabled = true;
        indicator.TextAlignment = UITextAlignment.Center;
        indicator.Font.WithSize(UIFont.SmallSystemFontSize);
        indicator.TextColor = UIColor.White;
        indicator.Layer.CornerRadius = 10;
        indicator.ClipsToBounds = true;
        indicator.TranslatesAutoresizingMaskIntoConstraints = false;
        container.AddSubview(indicator);

        indicator.WidthAnchor.ConstraintEqualTo(200f).Active = true;
        indicator.HeightAnchor.ConstraintEqualTo(40f).Active = true;
        indicator.CenterXAnchor.ConstraintEqualTo(container.CenterXAnchor).Active = true;
        indicator.BottomAnchor.ConstraintEqualTo(container.BottomAnchor, constant: -20f).Active = true;

        var tapRecognizer = new UITapGestureRecognizer(() =>
        {
            CobrowseIO.Instance().CurrentSession?.End(null);
        });
        tapRecognizer.NumberOfTapsRequired = 1;
        indicator.AddGestureRecognizer(tapRecognizer);
        return indicator;
    }

    public override void SessionDidUpdate(Session session)
    {
    }

    public override void SessionDidEnd(Session session)
    {
    }
}
```

### Xamarin.Android implementation

You can fully customize the interface for a Cobrowse session. The SDK provides hooks via `CobrowseIO.ISessionControlsDelegate` for you to render your own interface:

```csharp
using Xamarin.CobrowseIO;

[Application]
public class MainApplication : Application, CobrowseIO.ISessionControlsDelegate
{
    public override void OnCreate()
    {
        base.OnCreate();
        CobrowseIO.Instance.SetDelegate(this);
        // and the rest of cobrowse setup ...
    }

    public void ShowSessionControls(Activity activity, Session session)
    {
        // optionally show your own controls here
    }

    public void HideSessionControls(Activity activity, Session session)
    {
        // hide controls created by the method above here
    }

    //...
}
```

### Xamarin.Forms implementation

Even though Cobrowse.io works with native views, there is nothing that would prevent you from using `Xamarin.Forms.VisualElement` as a session indicator.

First, create an indicator view using Xamarin.Forms (`CobrowseCustomView.xaml`):

```markup
<?xml version="1.0" encoding="UTF-8"?>
<ContentView
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="SampleApp.Forms.CobrowseCustomView"
    HeightRequest="42"
    WidthRequest="130">
    <Button
        BackgroundColor="Red"
        TextColor="White"
        Text="End Session"
        CornerRadius="4"
        Clicked="EndSessionButton_Clicked" />
</ContentView>
```

Then, in the **iOS project**:

```csharp
using System;
using Foundation;
using UIKit;
using Xamarin.CobrowseIO;
using Xamarin.Forms.Platform.iOS;

[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        CobrowseIO.Instance.SetDelegate(new CustomOverlayCobrowseDelegate());
    }
}

public class CustomOverlayCobrowseDelegate : CobrowseDelegateImplementation
{
    private UIView _indicatorInstance;

    public override void CobrowseShowSessionControls(Session session)
    {
        if (_indicatorInstance == null)
        {
            _indicatorInstance = GetDefaultSessionIndicator(container: UIApplication.SharedApplication.KeyWindow);
        }
        _indicatorInstance.Hidden = false;
    }

    public override void CobrowseHideSessionControls(Session session)
    {
        if (_indicatorInstance != null)
            _indicatorInstance.Hidden = true;
    }

    private UIView GetDefaultSessionIndicator(UIView container)
    {
        var indicator = new CobrowseCustomView();
        var renderer = Platform.CreateRenderer(indicator);
        renderer.Element.Layout(new Xamarin.Forms.Rectangle(0, 0, indicator.WidthRequest, indicator.HeightRequest));
        var nativeIndicator = renderer.NativeView;
        nativeIndicator.TranslatesAutoresizingMaskIntoConstraints = false;

        container.AddSubview(nativeIndicator);

        nativeIndicator.WidthAnchor.ConstraintEqualTo((float)indicator.WidthRequest).Active = true;
        nativeIndicator.HeightAnchor.ConstraintEqualTo((float)indicator.HeightRequest).Active = true;
        nativeIndicator.CenterYAnchor.ConstraintEqualTo(container.CenterYAnchor).Active = true;
        nativeIndicator.RightAnchor.ConstraintEqualTo(container.RightAnchor, constant: -20f).Active = true;

        return nativeIndicator;
    }
}
```

And in the **Android project**:

```csharp
using Xamarin.CobrowseIO;
using Xamarin.Forms.Platform.Android;

[Application]
public class MainApplication : Application
{
    public override void OnCreate()
    {
        CobrowseIO.Instance.SetDelegate(new CustomOverlayCobrowseDelegate());
    }
}

public class CustomOverlayCobrowseDelegate : CobrowseDelegateImplementation, CobrowseIO.ISessionControlsDelegate
{
    private View _overlayIndicator;

    public void ShowSessionControls(Activity activity, Session session)
    {
        if (_overlayIndicator != null)
        {
            return;
        }
        if (!(activity is FormsAppCompatActivity))
        {
            return;
        }
        var indicator = new CobrowseCustomView();
        var renderer = Platform.CreateRendererWithContext(indicator, activity);
        renderer.Element.Layout(new Xamarin.Forms.Rectangle(0, 0, indicator.WidthRequest, indicator.HeightRequest));
        var nativeIndicator = renderer.View;

        var modal = new RelativeLayout(activity);
        var layoutParams = new RelativeLayout.LayoutParams(
            (int)TypedValue.ApplyDimension(ComplexUnitType.Dip, (float)indicator.WidthRequest, activity.Resources.DisplayMetrics),
            (int)TypedValue.ApplyDimension(ComplexUnitType.Dip, (float)indicator.HeightRequest, activity.Resources.DisplayMetrics))
        {
            MarginEnd = (int)TypedValue.ApplyDimension(ComplexUnitType.Dip, 4f, activity.Resources.DisplayMetrics)
        };
        layoutParams.AddRule(LayoutRules.CenterVertical);
        layoutParams.AddRule(LayoutRules.AlignParentEnd);
        modal.AddView(nativeIndicator, layoutParams);

        var rootFrameLayout = (ViewGroup)activity.Window.PeekDecorView();
        rootFrameLayout.AddView(modal, new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MatchParent, ViewGroup.LayoutParams.MatchParent));
        rootFrameLayout.Invalidate();

        _overlayIndicator = modal;
    }

    public void HideSessionControls(Activity activity, Session session)
    {
        if (_overlayIndicator == null)
        {
            return;
        }
        if (!(activity is FormsAppCompatActivity))
        {
            return;
        }
        var rootFrameLayout = (ViewGroup)activity.Window.PeekDecorView();
        rootFrameLayout.RemoveView(_overlayIndicator);
        _overlayIndicator = null;
    }
}
```
{% endtab %}

{% tab title="MacOS" %}
The SDK provides hooks via `CobrowseIODelegate` for you to render your own interface:

```objectivec
@implementation CBAppDelegate // should implement CobrowseIODelegate

- (void)applicationDidFinishLaunching:(NSNotification *)notification
    CobrowseIO.instance.delegate = self;
    // ... the rest of your app setup
}

- (void)cobrowseShowSessionControls:(CBIOSession*) session {
    // You can render controls however you like here.
}

- (void)cobrowseHideSessionControls:(CBIOSession*) session {
    // hide your custom control here
}

@end
```
{% endtab %}

{% tab title="Windows" %}
You may override the default session and active display indicator by handling `CobrowseIO.Instance.SessionControlsUpdated`:

```csharp
CobrowseIO.Instance.SessionControlsUpdated += OnSessionControlsUpdated;
```

Callback will be called when:

* Session starts. `FrameInfo` will contian information about the display currently captured by the screenshare session.
* Active display is switched. `FrameInfo` will contain information about the display which is being switched to.
* Session ends. `FrameInfo` param will be `null`. This means that session UI should be hidden.

**Warning:** Be aware that callback is called from non-UI thread.
{% endtab %}
{% endtabs %}

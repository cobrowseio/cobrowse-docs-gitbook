# Customize the interface

Cobrowse.io provides some minimal default UI for things like user consent and to visually indicate when a session is active.

Many customers are happy to use our default UI which works out of box. But if you prefer, everything is easily customizable to meet your specific requirements.

{% tabs %}
{% tab title="Web" %}
You can fully customize the interface for a Cobrowse session. The SDK provides hooks for you to render your own interface:

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
CobrowseIO.currentSession.end();
```

### Example

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
### Customizing the Session Indicator

You can fully customize the interface for a Cobrowse session. The SDK provides hooks via `CobrowseIODelegate` for you to render your own interface:

**Swift**

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

**Objective C**

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

### Customizing the 6 Digit Code screen

You can build your own UI to completely replace the default UI we provide for generating 6 digit codes. You can generate a code for your UI by using the `createSession` API:

```objectivec
[CobrowseIO.instance createSession:^(NSError * _Nullable err, CBIOSession * _Nullable session) {
    if (err) NSLog(@"Error creating code %@", err);
    else NSLog(@"Created session code: %@", session.code);
}];
```

**Note:** the codes expire shortly after creation \(codes last less than 10 minutes\), so wait until your user is ready to share the code with an agent before generating it:

You can monitor changes in the state of the session you create using the Cobrowse delegate methods:

```objectivec
-(void) cobrowseSessionDidUpdate: (nonnull CBIOSession*) session;
-(void) cobrowseSessionDidEnd: (nonnull CBIOSession*) session;
```

You can get information about the state of the session using the following methods, which may adjust the UI you are showing:

```objectivec
[session isPending] // session has been created but is waiting for agent or user
[session isAuthorizing] // waiting for the user to confirm the session
[session isActive] // session running, frames are streaming to the agent
[session isEnded] // session is over and can no longer be used or edited
```
{% endtab %}

{% tab title="Android" %}
### Customize the Session Indicator

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

### Customize the 6 Digit Code screen

You can build your own UI to completely replace the default UI we provide for generating 6 digit codes. You can generate a code for your UI by using the `createSession` API:

```java
CobrowseIO.instance().createSession(new Callback<Error, Session>() {
    @Override
    public void call(@Nullable Error err, @Nullable Session session) {
        if (err != null) Log.e("CobrowseIO", "Error creating session " + err.getMessage());
        else if (session != null) Log.i("CobrowseIO", "Created session code " + session.code());
    }
});
```

**Note:** the codes expire shortly after creation \(codes last less than 10 minutes\), so wait until your user is ready to share the code with an agent before generating it:

You can monitor changes in the state of the session you create using the Cobrowse delegate methods:

```java
void sessionDidUpdate(final @NonNull Session session);
void sessionDidEnd(final @NonNull Session session);
```

You can get information about the state of the session using the following methods, which may adjust the UI you are showing:

```java
session.isPending() // session has been created but is waiting for agent or user
session.isAuthorizing() // waiting for the user to confirm the session
session.isActive() // session running, frames are streaming to the agent
session.isEnded() // session is over and can no longer be used or edited
```
{% endtab %}

{% tab title="React Native" %}

{% endtab %}

{% tab title="Xamarin" %}

{% endtab %}
{% endtabs %}


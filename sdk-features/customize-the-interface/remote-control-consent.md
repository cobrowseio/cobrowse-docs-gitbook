---
description: Learn how to customize the remote control consent prompt to your needs.
---

# Remote control consent dialog

By default, Cobrowse will show a remote control consent dialog when an agent requests remote control. You may modify and customize the remote control consent prompt as you wish, using the SDK hooks described below.

Admin users may also disable this remote control consent prompt from your account settings if you prefer to remove it entirely: [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings)

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.confirmRemoteControl = function() {
    return new Promise(function(resolve, reject) {
        // show your own UI here
        // call resolve(true) to allow
        // call reject() to reject
        resolve(true)
    });
};
```
{% endtab %}

{% tab title="iOS" %}
To override the the default remote control consent prompt, you should implement the `cobrowseHandleRemoteControlRequest:` method of `CobrowseIODelegate`.

```objectivec
-(void) cobrowseHandleRemoteControlRequest:(CBIOSession*) session {
    // show your own UI here
    // call [session setRemoteControl:CBIORemoteControlStateOn callback:nil] to allow
    // or [session setRemoteControl:CBIORemoteControlStateRejected callback:nil] to reject
    [session setRemoteControl:CBIORemoteControlStateOn callback:nil];
}
```
{% endtab %}

{% tab title="Android" %}
To override the default remote control consent prompt, you should implement the `CobrowseIO.RemoteControlRequestDelegate` interface on your `CobrowseIO.Delegate`.

```kotlin
override fun handleRemoteControlRequest(activity: Activity?, session: Session) {
    // show your own UI here
    // call session.setRemoteControl(Session.RemoteControlState.On, null) to accept
    // or session.setRemoteControl(Session.RemoteControlState.Rejected, null) to reject
    session.setRemoteControl(Session.RemoteControlState.On, null)
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.handleRemoteControlRequest = function(session) {
    // Show your own UI here
    // call session.setRemoteControl('on') to allow
    // or session.setRemoteControl('rejected') to reject
    session.setRemoteControl('on')
}
```
{% endtab %}

{% tab title="Flutter" %}
```dart
CobrowseIO.instance.handleRemoteControlRequest.listen((session) {
    // Show your own UI here
    // call session.setRemoteControl(RemoteControlState.on) to allow
    // or session.setRemoteControl(RemoteControlState.rejected) to reject
    session.setRemoteControl(RemoteControlState.on);
});
```
{% endtab %}

{% tab title=".NET Mobile" %}
```сsharp
CobrowseIO.Instance.RemoteControlRequest += (object sender, ISession session) =>
{
    // Show your own UI here
    // Use RemoteControlState.On to allow or RemoteControlState.Rejected to reject
    session.SetRemoteControl(RemoteControlState.On, (e, s) => { });
};
```
{% endtab %}
{% endtabs %}

### Example UIs

We've created a set of sample UIs that you may drop directly into your app or website as a starting point to customize the consent prompt.

{% tabs %}
{% tab title="Web" %}
```javascript
// A generic consent dialog class
function Consent() {
  var container = document.createElement('div');
  function content(title, description){
    return '\
      <div style="background: rgba(50, 50, 50, 0.4); position: fixed; z-index: 2147483647; bottom: 0; top: 0; left: 0; right: 0">\
        <div style="color: #333; font-family:sans-serif; line-height:140%; position:fixed; padding:25px; background:white; border-radius:15px; z-index:2147483647; top:50px; left:50%; width:75%; max-width:350px; transform:translateX(-50%); box-shadow:0px 0px 15px #33333322;">\
          <div style="text-align:center; margin-top:10px; margin-bottom:20px"><b>'+title+'</b></div>\
          <div>'+description+'</div>\
          <div style="float:right; margin-top:40px; color:rgb(0, 122, 255);">\
            <a class="cobrowse-deny" style="cursor:pointer; padding:10px;">Deny</a>\
            <a class="cobrowse-allow" style="cursor:pointer; padding:10px; font-weight: bold;">Allow</a>\
          </div>\
        </div>\
      </div>\
    ';
  }

  this.show = function(title, description) {
    return new Promise(function(resolve) {
      container.innerHTML = content(title, description);
      container.querySelector('.cobrowse-allow').addEventListener('click', function(){ resolve(true); this.hide() }.bind(this));
      container.querySelector('.cobrowse-deny').addEventListener('click', function(){ resolve(false); this.hide() }.bind(this));
      if (document.body) document.body.appendChild(container);
    }.bind(this));
  }.bind(this);

  this.hide = function() {
    if (container.parentNode) {
      container.parentNode.removeChild(container);
    }
  }.bind(this);
}

// Integration with Cobrowse
CobrowseIO.confirmRemoteControl = function() {
    return new Consent().show('Remote Control Request', 'Do you want to allow remote control by ' + CobrowseIO.currentSession.agent().name + '?');
}
```
{% endtab %}

{% tab title="iOS" %}
```objectivec
@interface CBAppDelegate : UIResponder <UIApplicationDelegate, CobrowseIODelegate>

@end


@implementation CBAppDelegate

// Custom consent dialog UI
UIAlertController *remoteControlPrompt;

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    CobrowseIO.instance.delegate = self;
    // insert your license key here
    CobrowseIO.instance.license = @"trial";
    [CobrowseIO.instance start];

    ...
}

// Implement cobrowseHandleRemoteControlRequest:(CBIOSession*) to show a custom prompt
- (void) cobrowseHandleRemoteControlRequest:(CBIOSession *)session {
    // replace the code below with your own confirmation prompt
    remoteControlPrompt = [UIAlertController alertControllerWithTitle:@"Allow remote control?"
                                             message:@"A support agent would like to control this app. Do you accept?"
                                             preferredStyle:UIAlertControllerStyleAlert];
    [remoteControlPrompt addAction: [UIAlertAction actionWithTitle:@"Accept"
                                                   style:UIAlertActionStyleDefault
                                                   handler:^(UIAlertAction * _Nonnull action) {
        [session setRemoteControl:CBIORemoteControlStateOn callback:nil];
        remoteControlPrompt = nil;
    }]];
    [remoteControlPrompt addAction: [UIAlertAction actionWithTitle:@"Decline"
                                                   style:UIAlertActionStyleCancel
                                                   handler:^(UIAlertAction * _Nonnull action) {
        [session setRemoteControl:CBIORemoteControlStateRejected callback:nil];
        remoteControlPrompt = nil;
    }]];

    UIViewController* appViewController = [self findPresentedViewController];
    [appViewController presentViewController:remoteControlPrompt animated:YES completion:nil];
}

- (void)cobrowseSessionDidEnd:(CBIOSession *)session {
    if (remoteControlPrompt) {
        [remoteControlPrompt dismissViewControllerAnimated:NO completion:^{
            remoteControlPrompt = nil;
        }];
    }
}

@end
```
{% endtab %}

{% tab title="Android" %}
```kotlin
class MainApplication : Application(),
    CobrowseIO.Delegate, CobrowseIO.RemoteControlRequestDelegate {

    // Custom consent dialog UI
    private var customRemoteControlConsent: CustomRemoteControlConsentDialogFragment? = null

    override fun onCreate() {
        super.onCreate()

        CobrowseIO.instance().setDelegate(this)
        // ...
    }

    /*
     * Implement handleRemoteControlRequest(Activity, Session) from CobrowseIO.RemoteControlRequestDelegate
     * to show a custom prompt
     */
    override fun handleRemoteControlRequest(activity: Activity?, session: Session) {
        // replace the code below with your own confirmation prompt
        val tag = "RemoteControlConsentDialog"
        val fragments = activity?.fragmentManager ?: return
        val transaction = fragments.beginTransaction()
        fragments.findFragmentByTag(tag)?.let { transaction.remove(it) }
        customRemoteControlConsent = CustomRemoteControlConsentDialogFragment().also {
            it.show(transaction, tag)
        }
    }

    override fun sessionDidEnd(session: Session) {
        customRemoteControlConsent?.takeIf { it.isAdded }?.dismiss()
        customRemoteControlConsent = null
    }
}
```

And an example Fragment for showing the UI:

```kotlin
class CustomRemoteControlConsentDialogFragment : DialogFragment() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        retainInstance = true
    }

    override fun onDestroyView() {
        if (retainInstance) {
            dialog?.setDismissMessage(null)
        }
        super.onDestroyView()
    }

    override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
        isCancelable = false
        return AlertDialog.Builder(activity)
            .setTitle("Allow remote control?")
            .setMessage("A support agent would like to control this app. Do you accept?")
            .setPositiveButton("Allow") { _, _ ->
                CobrowseIO.instance().currentSession()
                    ?.setRemoteControl(Session.RemoteControlState.On, null)
            }
            .setNegativeButton("Reject") { _, _ ->
                CobrowseIO.instance().currentSession()
                    ?.setRemoteControl(Session.RemoteControlState.Rejected, null)
            }
            .create()
    }
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.handleRemoteControlRequest = function(session) {
    // Replace this with your own logic
    Alert.alert(
      'Remote Control Request',
      'A support agent would like to take remote control of this app. Do you accept?',
      [{
        text: 'Reject',
        onPress: () => session.setRemoteControl('rejected'),
        style: 'cancel'
      }, {
        text: 'Accept',
        onPress: () => session.setRemoteControl('on')
      }], { cancelable: false })
  }
}
```
{% endtab %}

{% tab title="Flutter" %}
```dart
CobrowseIO.instance.handleRemoteControlRequest.listen((session) {
    showDialog(
        context: context,
        builder: (BuildContext context) {
            return AlertDialog(
                title: const Text('Remote Control Request'),
                content: const Text('A support agent would like to control this app. Do you accept?'),
                actions: <Widget>[
                    TextButton(
                        onPressed: () {
                            try {
                                session.setRemoteControl(RemoteControlState.on);
                                Navigator.of(context).pop();
                            } on PlatformException {
                                // E.g. a network error
                            }
                        },
                        child: const Text('Accept'),
                    ),
                    TextButton(
                        onPressed: () {
                            try {
                                session.setRemoteControl(RemoteControlState.off);
                                Navigator.of(context).pop();
                            } on PlatformException {
                                // E.g. a network error
                            }
                        },
                        child: const Text('Cancel'),
                    ),
                ],
            );
        },
    );
});
```
{% endtab %}

{% tab title=".NET Mobile" %}
```сsharp
CobrowseIO.Instance.RemoteControlRequest += async (object sender, ISession session) =>
{
    bool allowed = await RequireMainPage().DisplayAlert(
        title: "Cobrowse.io",
        message: "Allow remote control?",
        accept: "Allow",
        cancel: "Reject");
    if (allowed)
    {
        session.SetRemoteControl(RemoteControlState.On, (e, s) => { });
    }
    else
    {
        session.SetRemoteControl(RemoteControlState.Rejected, (e, s) => { });
    }
};
```
{% endtab %}
{% endtabs %}


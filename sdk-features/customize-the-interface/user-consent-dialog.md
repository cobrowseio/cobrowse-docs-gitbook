# User consent dialog

By default, Cobrowse will show a user consent dialog when a new session is incoming. You may modify and customize the consent prompt as you wish, using the SDK hooks described below.

Admin users may also disable this consent prompt from your account settings if you prefer to remove it entirely: [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings)

Here are the SDK hooks:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.confirmSession = function() {
    return new Promise(function(resolve, reject) {
        // replace the code below with your own confirmation prompt
        // calling resolve or reject as a appropriate
        // NOTE: do not use window.confirm in production!
        if (window.confirm('Replace this window.confirm with your consent dialog!'))
            resolve(true);
        else reject();
    });
}
```

### Sample UI

We've created a Sample UI that you may drop directly into your website to show customize the consent prompt, which inherits the styles and colors from your website.

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
CobrowseIO.confirmSession = function() {
    return new Consent().show('Session Request', 'Do you want to share your screen?');
}
```
{% endtab %}

{% tab title="iOS" %}
### Swift

_Not yet documented. Please contact us at_ [_hello@cobrowse.io_](mailto:hello@cobrowse.io)_._

### Objective C

```objectivec
@implementation CBAppDelegate // should implement CobrowseIODelegate

- (BOOL)application:(UIApplication*) application didFinishLaunchingWithOptions:(NSDictionary*) launchOptions {
    CobrowseIO.instance.delegate = self;
    // ... the rest of your app setup
    return YES;
}

-(void) cobrowseHandleSessionRequest:(CBIOSession*) session {
    // show your own UI here
    // call [session activate: <callback>] to accept and start the session
    // provide a callback to handle any errors during session initiation
    [session activate: nil];
}

@end
```
{% endtab %}

{% tab title="Android" %}
```java
public class MainApplication extends Application implements CobrowseIO.SessionRequestDelegate {

    @Override
    public void onCreate() {
        super.onCreate();
        CobrowseIO.instance().setDelegate(this);

        // and the rest of cobrowse setup ...
    }

    @Override
    public void handleSessionRequest(final Activity currentActivity, final Session session) {
        // Do something here, e.g. showing a permission request dialog
        // Make sure to call activate(<callback>) on the session object if
        // you want to start the session.
        // Provide a callback if you wish to handle errors during session
        // initiation.
        session.activate(null);
    }

    // ...
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.handleSessionRequest = function(session) {
    // Replace this with your own logic
    // Just be sure to call session.activate() to
    // accept the session.
    Alert.alert(
        'Session Requested',
        'A cobrowse session has been requested',
        [
            {
                text: 'Cancel',
                onPress: () => {},
                style: 'cancel',
            },
            {text: 'OK', onPress: () => session.activate()},
        ],
        {cancelable: true},
    );
}
```
{% endtab %}

{% tab title="Xamarin" %}
### Xamarin.iOS implementation

```csharp
using Xamarin.CobrowseIO;

[Register("AppDelegate")]
public class AppDelegate : UIResponder, IUIApplicationDelegate
{
    [Export("application:didFinishLaunchingWithOptions:")]
    public bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // ... the rest of your app setup
        CobrowseIO.Instance.SetDelegate(new CustomCobrowseDelegate());
        return true;
    }
}

public class CustomCobrowseDelegate : CobrowseIODelegate
{
    public override void CobrowseHandleSessionRequest(Session session)
    {
        // show your own UI here
        // call Activate(<callback>) to accept and start the session
        // provide a callback to handle any errors during session initiation
        session.Activate(callback: null);
    }

    // ...
}
```

## Xamarin.Android implementation

```csharp
using Xamarin.CobrowseIO;

[Application]
public class MainApplication : Application
{
    public MainApplication()
    {
    }

    protected MainApplication(IntPtr javaReference, JniHandleOwnership transfer)
        : base(javaReference, transfer)
    {
    }

    public override void OnCreate()
    {
        base.OnCreate();
        CobrowseIO.Instance.SetDelegate(new CustomCobrowseDelegate());
        // and the rest of cobrowse setup ...
    }
}

public class CustomCobrowseDelegate : Java.Lang.Object, CobrowseIO.ISessionRequestDelegate
{
    public CustomCobrowseDelegate()
    {
    }

    public CustomCobrowseDelegate(IntPtr handle, JniHandleOwnership transfer)
        : base(handle, transfer)
    {
    }

    public void HandleSessionRequest(Activity activity, Session session)
    {
        // Do something here, e.g. showing a permission request dialog
        // Make sure to call Activate(<callback>) on the session object if
        // you want to start the session.
        // Provide a callback if you wish to handle errors during session
        // initiation.
        session.Activate(callback: null);
    }

    // ...
}
```

## Xamarin.Forms implementation

You can also achieve this functionality from a cross-platform project. In this case you don't have to implement your own delegate, but instead you subscribe to the `SessionDidRequest` event:

```csharp
using Xamarin.CobrowseIO.Abstractions;

public partial class App : Xamarin.Forms.Application
{
    public App()
    {
        InitializeComponent();

        CobrowseIO.Instance.License = "trial";
        CobrowseIO.Instance.Start();
        // and the rest of cobrowse setup ...
    }

    protected override void OnStart()
    {
        Subscribe();
    }

    protected override void OnSleep()
    {
        Unsubscribe();
    }

    protected override void OnResume()
    {
        Subscribe();
    }

    private void Subscribe()
    {
        CobrowseIO.Instance.SessionDidRequest += OnCobrowseSessionDidRequest;
    }

    private void Unsubscribe()
    {
        CobrowseIO.Instance.SessionDidRequest -= OnCobrowseSessionDidRequest;
    }

    private async void OnCobrowseSessionDidRequest(object sender, ISession session)
    {
        // Do something here, e.g. showing a permission request dialog
        // Make sure to call Activate(<callback>) on the session object if
        // you want to start the session.
        // Provide a callback if you wish to handle errors during session
        // initiation.
        bool allowed = await this.MainPage.DisplayAlert(
            title: "Cobrowse.io",
            message: "Allow Cobrowse.io session?",
            accept: "Allow",
            cancel: "Reject");
        if (allowed)
        {
            session.Activate(null);
        }
        else
        {
            session.End(null);
        }
    }
}
```
{% endtab %}

{% tab title="MacOS" %}
```objectivec
@implementation CBAppDelegate // should implement CobrowseIODelegate

- (void)applicationDidFinishLaunching:(NSNotification *)notification
    CobrowseIO.instance.delegate = self;
    // ... the rest of your app setup
}

-(void) cobrowseHandleSessionRequest:(CBIOSession*) session {
    // show your own UI here
    // call [session activate: <callback>] to accept and start the session
    // provide a callback to handle any errors during session initiation
    [session activate: nil];
}

@end
```
{% endtab %}

{% tab title="Windows" %}
You can override the default session authorization dialog by adding a handler to the `CobrowseIO.Instance.SessionAuthorizing` event:

```csharp
  CobrowseIO.Instance.SessionAuthorizing += OnSessionAuthorizing;
```

**Warning:** Callback will be called from non-UI thread, so be sure to dispatch it to the UI one.

* To confirm session:

```csharp
await CobrowseIO.Instance.CurrentSession.Activate();
```

* To reject a session:

```csharp
await CobrowseIO.Instance.CurrentSession.End();
```

WPF MessageBox example:

```csharp
if (MessageBox.Show(
        window,
        "Would you like to authorize the Cobrowse.io screenshare session?",
        "Authorization",
        MessageBoxButton.YesNo,
        MessageBoxImage.Question
      ) == MessageBoxResult.Yes)
  await CobrowseIO.Instance.CurrentSession.Activate();
else
  await CobrowseIO.Instance.CurrentSession.End();
```
{% endtab %}
{% endtabs %}

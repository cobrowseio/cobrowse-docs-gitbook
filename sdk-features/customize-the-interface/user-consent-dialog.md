# User consent dialog

By default, Cobrowse will show a user consent dialog when a new session is incoming. You may modify and customize the consent prompt as you wish, using the SDK hooks described below.

Admin users may also disable this consent prompt from your account settings if you prefer to remove it entirely: [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings)

Here are the SDK hooks:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.confirmSession = function() {
    return new Promise(function(resolve, reject) {
        // replace the code below with your own confirmation prompt, calling resolve or reject as a appropriate
        if (window.confirm('Allow session?')) resolve();
        else reject();
    });
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
    // Just be sure to call CobrowseIO.activateSession() to
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
            {text: 'OK', onPress: () => CobrowseIO.activateSession()},
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

        CobrowseIO.Instance.SetLicense("trial");
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
{% tab title="Windows" %}
```csharp
placeholder
```
{% endtab %}
{% endtabs %}


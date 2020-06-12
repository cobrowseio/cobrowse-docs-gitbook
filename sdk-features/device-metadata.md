# Add device metadata

## Add device metadata

To help you identify, search, and filter devices in your Cobrowse dashboard, it's helpful to specify any meaningful metadata.

You may add any custom key/value pairs you'd like, and they will all be searchable and filterable in your online dashboard. We've added a few placeholders for convenience only - all fields are optional.

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.customData = {
    user_name: "Joe Blogs",
    user_id: "12345",
    user_email: "joe@example.com"
}
```
{% endtab %}

{% tab title="iOS \(Swift\)" %}
```swift
 import CobrowseIO

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
{
    CobrowseIO.instance().license = "<your license key here>"

    print("Cobrowse device id:  \(CobrowseIO.instance().deviceId)")

    CobrowseIO.instance().customData = [
        kCBIOUserIdKey: "<your_user_id>" as NSObject,
        kCBIOUserNameKey: "<your_user_name>" as NSObject,
        kCBIOUserEmailKey: "<your_user_email>" as NSObject,
        kCBIODeviceIdKey: "<your_device_id>" as NSObject,
        kCBIODeviceNameKey: "<your_device_name>" as NSObject
    ]

    return true
}
```
{% endtab %}

{% tab title="iOS \(Obj-C\)" %}
```text
@import CobrowseIO;

- (BOOL)application:(UIApplication*) application didFinishLaunchingWithOptions:(NSDictionary*) launchOptions
{
    CobrowseIO.instance.license = @"<your license key here>";
    [CobrowseIO.instance start];
    return YES;
}
```
{% endtab %}

{% tab title="Android" %}
```java
package com.example;

import android.app.Application;
import io.cobrowse.CobrowseIO;

public class MainApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        CobrowseIO.instance().license("<your license key here>");

        Log.i("App", "Cobrowse device id: " + CobrowseIO.instance().deviceId(this));

        HashMap<String, Object> customData = new HashMap<>();
        customData.put("user_id", "<your_user_id>");
        customData.put("user_name", "<your_user_name>");
        customData.put("user_email", "<your_user_email>");
        customData.put("device_id", "<your_device_id>");
        customData.put("device_name", "<your_device_name>");
        CobrowseIO.instance().customData(customData);

        CobrowseIO.instance().start(this);
    }
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.customData = {
    user_email: "react-native@example.com"
};
```
{% endtab %}

{% tab title="Xamarin" %}
```csharp
CobrowseIO.Instance.SetLicense("<your license key here>");
CobrowseIO.Instance.SetCustomData(new Dictionary<string, object>
{
    { CobrowseIO.UserIdKey, "<your_user_id>" },
    { CobrowseIO.UserNameKey, "<your_user_name>" },
    { CobrowseIO.UserEmailKey, "<your_user_email>" },
    { CobrowseIO.DeviceIdKey, "<your_device_id>" },
    { CobrowseIO.DeviceNameKey, "<your_device_name>" }
});
CobrowseIO.Instance.Start();
```
{% endtab %}
{% endtabs %}




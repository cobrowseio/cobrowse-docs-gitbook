# Full device remote control

Full device remote control allows your support agents to take full control of a user's device, for example to change system settings or control other apps. This feature is only supported on the Android and Windows SDKs.

### Configuring full device remote control

On platforms that support full device remote control, there may be extra steps required during your SDK integration:

{% tabs %}
{% tab title="Android" %}
{% hint style="warning" %}
Due to recent Google Play Store policy changes these instructions have been updated. You must be using SDK version **v2.16.0** or above.
{% endhint %}

Full device remote control for Android, including unattended access, uses an Accessibility Service that must be enabled on the device to grant access.

This feature is supported in API 21 (5.0 Lollipop) and above.

Add a new service declaration to your application `AndroidManifest.xml` file:

```xml
<service
    android:name="io.cobrowse.CobrowseAccessibilityService"
    android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE"
    tools:node="merge" />
```

When added, your manifest should look something similar to this:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    ...
    <application>
        ...
        <service
            android:name="io.cobrowse.CobrowseAccessibilityService"
            android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE"
            tools:node="merge" />

    </application>
</manifest>
```

Enable the accessibility service the Cobrowse SDK will have added in the main device settings, eg. Settings -> Accessibility -> Your App Name. Note: this only has to be done the very first time.
{% endtab %}

{% tab title="React Native" %}
{% hint style="info" %}
Please follow the Android documentation to implement full device remote control using React Native.
{% endhint %}
{% endtab %}

{% tab title="Xamarin" %}
{% hint style="info" %}
Please follow the Android documentation to implement full device remote control using Xamarin.
{% endhint %}
{% endtab %}

{% tab title="Windows" %}
Full device remote control by default, no extra integration needed.
{% endtab %}
{% endtabs %}

### Detecting AccessibilityService state on Android

We have built some logic and APIs to detect if the Android accessibility service is running, and if not, to deep link the user to the settings to enable it.

Show the sample UI with:

{% tabs %}
{% tab title="Android" %}
```java
CobrowseAccessibilityService.showSetup(...);
```
{% endtab %}

{% tab title="React Native" %}
```javascript
import { CobrowseAccessibilityService } from 'cobrowse-sdk-react-native'
CobrowseAccessibilityService.showSetup(...);
```
{% endtab %}

{% tab title="Xamarin" %}
```csharp
CobrowseAccessibilityService.ShowSetup(...)
```
{% endtab %}
{% endtabs %}

Check if accessibility service is already running with:

{% tabs %}
{% tab title="Android" %}
```
CobrowseAccessibilityService.isRunning(...);
```
{% endtab %}

{% tab title="React Native" %}
```javascript
import { CobrowseAccessibilityService } from 'cobrowse-sdk-react-native'
CobrowseAccessibilityService.isRunning(...);
```
{% endtab %}

{% tab title="Xamarin" %}
```csharp
CobrowseAccessibilityService.IsRunning(...)
```
{% endtab %}
{% endtabs %}

Deep link user to accessibility settings with:

{% tabs %}
{% tab title="Android" %}
```java
Intent intent = new Intent(android.provider.Settings.ACTION_ACCESSIBILITY_SETTINGS);
intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```
{% endtab %}

{% tab title="Xamarin" %}
```csharp
Intent intent = new Intent(global::Android.Provider.Settings.ActionAccessibilitySettings);
intent.AddFlags(ActivityFlags.NewTask);
StartActivity(intent);
```
{% endtab %}
{% endtabs %}

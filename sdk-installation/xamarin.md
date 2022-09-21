---
description: Xamarin SDK
---

# Xamarin

## Installation

{% hint style="info" %}
Looking for `net6.0-android` and `net6.0-ios` support? Please [email us](mailto:hello@cobrowse.io)
{% endhint %}

We recommend installing the Cobrowse.io SDK using NuGet. Add the following package to your Xamarin projects:

* `CobrowseIO.Xamarin`: [![CobrowseIO.iOS NuGet](https://img.shields.io/nuget/v/CobrowseIO.Xamarin.svg?label=CobrowseIO.Xamarin)](https://www.nuget.org/packages/CobrowseIO.Xamarin/)

{% tabs %}
{% tab title="Xamarin.iOS" %}
To use Cobrowse.io in your Xamarin.iOS project, please add the following lines to your `AppDelegate.cs`:

```csharp
using Xamarin.CobrowseIO;

[Register("AppDelegate")]
public class AppDelegate : UIResponder, IUIApplicationDelegate
{
    [Export("application:didFinishLaunchingWithOptions:")]
    public bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        CobrowseIO.Instance.License = "put your license key here";
        CobrowseIO.Instance.Start();

        return true;
    }
}
```

__

**Xamarin.iOS Swift support**

Cobrowse.io SDK uses Swift language, and if you are targeting iOS 12.1 or earlier, it is required to add Xamarin.iOS.SwiftRuntimeSupport NuGet dependency to ship specific Swift dylibs used by Cobrowse with your application.

* [![NuGet](https://img.shields.io/nuget/v/Xamarin.iOS.SwiftRuntimeSupport.svg?label=Xamarin.iOS.SwiftRuntimeSupport)](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)

This NuGet copies the necessary Swift libraries in the right place when building your app or when generating the archive of the app (not the IPA).

This NuGet, however, does not copy the Swift libraries in the right place when generating the IPA using Visual Studio, causing the rejection of the App Store when uploading the IPA. Please follow [Xamarin documentation](https://github.com/xamarin/XamarinComponents/blob/master/iOS/SwiftRuntimeSupport/readme.txt) describing how to build and publish your Xamarin.iOS application using the Xcode IPA wizard. The steps are:

1. In Visual Studio, select a valid iOS device before archiving.
2. Go to _Build_ menu → _Archive for Publishing_
3. Once done, open Xcode and go to _Window_ → _Organizer_
4. Select the _Archives_ tab
5. On the left side of the window, select your app
6. Click on _Distribute App_ button and follow the wizard
{% endtab %}

{% tab title="Xamarin.Android" %}
To use Cobrowse.io in your Xamarin.Android project, please add the following lines to your Application subclass.

```csharp
using Xamarin.CobrowseIO;

[Application]
public class MainApplication : Application
{
    public override void OnCreate()
    {
        base.OnCreate();

        CobrowseIO.Instance.License = "put your license key here";
        CobrowseIO.Instance.Start(this);
    }
}
```

**Important:**

* Also make sure you are targeting Android 10 (API 29). In Visual Studio:
  * Open the project settings
  * Navigate to _Build_ → _General_
  * In _"Compile using Android version: (Target Framework)_" drop-down list choose **Android 10.0 (Q)**
* The Cobrowse.io SDK uses AndroidX, so make sure you have [migrated to AndroidX](https://docs.microsoft.com/en-us/xamarin/android/platform/androidx#migration-tooling)

You may also start CobrowseIO in your `MainActivity` or other Activity if necessary. In that case, the SDK will continue to function even as new Activities are being created and destroyed.
{% endtab %}

{% tab title="Xamarin.Forms" %}
Alternatively, it is possible to access Cobrowse.io features from Xamarin.Forms projects. In this case you only need to initialize the Cobrowse.io SDK in your cross-platform project:

```csharp
using Xamarin.CobrowseIO.Abstractions;

public partial class App : Xamarin.Forms.Application
{
    public App()
    {
        InitializeComponent();

        CobrowseIO.Instance.License = "put your license key here";
        CobrowseIO.Instance.Start();
    }
}
```
{% endtab %}
{% endtabs %}

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

### Xamarin sample app

We provide a sample application showing how to use Cobrowse.io NuGet packages in Xamarin. [Xamarin.Forms sample](https://github.com/cobrowseio/cobrowse-sdk-xamarin/tree/master/SampleForms) includes using 6-digit codes, full device screen sharing, and redacting sensitive data. [Xamarin Classic sample](https://github.com/cobrowseio/cobrowse-sdk-xamarin/tree/master/Sample) is also available.

## Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* iOS 11.0 or later
* Android API version 19 (4.4 KitKat) or later

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

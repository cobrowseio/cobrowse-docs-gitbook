---
description: .NET iOS and Android SDK
---

# .NET Mobile

## Installation

We recommend installing the Cobrowse.io SDK using NuGet. Add the following package to your .NET 8 project:

* `CobrowseIO.DotNet`: [![CobrowseIO.DotNet NuGet](https://img.shields.io/nuget/v/CobrowseIO.DotNet.svg?label=CobrowseIO.DotNet)](https://www.nuget.org/packages/CobrowseIO.DotNet/)

Add the following lines to your code which will register this device with the Cobrowse servers so you can connect to it. You could choose to do this on app startup, or when your users visits a support page in your application, or any other time.

```csharp
using Cobrowse.IO;

public partial class App : Microsoft.Maui.Controls.Application
{
    public App()
    {
        InitializeComponent();

        CobrowseIO.Instance.License = "put your license key here";
        CobrowseIO.Instance.Start();
    }
}
```

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your mobile app with your Cobrowse account.

### MAUI sample app

We provide a sample application showing how to use Cobrowse.io NuGet packages in MAUI with .NET 8. [MAUI sample](https://github.com/cobrowseio/cobrowse-sdk-xamarin/tree/dotnet/Sample) includes using 6-digit codes, full device screen sharing, and redacting sensitive data.

## Try it out

Once you have your app running in the iOS Simulator or on a physical device, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* iOS 11.0 or later
* Android API version 21 (5.0 Lollipop) or later

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

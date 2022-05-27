---
description: Windows SDK
---

# Windows

## Installation

We recommend installing the Cobrowse.io SDK using NuGet. Add the following package to your projects:

[![CobrowseIO.Windows NuGet](https://img.shields.io/nuget/v/CobrowseIO.Windows.svg?label=CobrowseIO.Windows)](https://www.nuget.org/packages/CobrowseIO.Windows/)

### Usage

To use Cobrowse.io in your project, use the `Cobrowse.IO` namespace:

```csharp
using Cobrowse.IO;
```

To start Cobrowse.io:

```csharp
CobrowseIO.Instance.License = "put your license key here";
await CobrowseIO.Instance.Start();
```

### Add your License Key

Please register an account and generate your free License Key at [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings).

This will associate sessions from your Windows app with your Cobrowse account.

## Try it out

Once you have your app running on a Windows computer, navigate to [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard) to see your device listed. You can click the "Connect" button to initiate a Cobrowse session!

## Requirements

* .NET Framework 4.6
* Minimum supported OS is Windows 7
* _x86_ and _x64_ architectures only are supported

### HiDPI Support

For Windows 10 HiDPI mode (when display scale is not 100%) client application should be "DPI aware". To enable this please add `app.manifest` ("New item" -> "Application Manifest File" in Visual Studio) and add/uncomment the following XML tag:

```markup
<application xmlns="urn:schemas-microsoft-com:asm.v3">
  <windowsSettings>
    <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
    <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
  </windowsSettings>
</application>
```

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

## **Firewalls**

If your agents work behind a firewall (e.g. a corporate firewall), then the **agent-side** API routes will need to be whitelisted as specified here: [https://docs.cobrowse.io/enterprise-self-hosting/advanced/firewalls#agent-side-required-apis](https://docs.cobrowse.io/enterprise-self-hosting/advanced/firewalls#agent-side-required-apis).

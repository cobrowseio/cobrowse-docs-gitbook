---
description: Plugin version v1.x
---

# Engage Workspace Desktop Edition (WDE)

## Installation

Supported WDE versions: 8.5.147.05+.

[WebView2](https://docs.genesys.com/Documentation/IW/8.5.1/Developer/Chromium) installed on your WDE instances.

### 1. Install plugin

Add the plugin and dependencies to your Workspace Desktop Edition installation:

* `Genesyslab.Desktop.Modules.CobrowseIO.dll`
* `Genesyslab.Desktop.Modules.CobrowseIO.module-config`

These files should be added adjacent to your other Module `.dll` files, in the same directory as `InteractionWorkspace.exe`. This executable is most commonly found at`C:\Program Files (x86)\GCTI\Workspace Desktop Edition`.

{% hint style="info" %}
Cobrowse can provide these files to you upon request, free of charge, in order to conduct a proof-of-concept (PoC) or other technical evaluation.
{% endhint %}

### 2. Add required config

The configuration values can be added via Genesys GAX, Genesys Administrator, or directly in a `InteractionWorkspace.exe.config` file adjacent to your `InteractionWorkspace.exe`

When using Genesys GAX, please navigate to Environment/Applications -> WorkspaceDesktop -> Application Options -> interaction-workspace and add the following fields there.&#x20;

| Field                                                                    | Type   | Value                                                                                                       |
| ------------------------------------------------------------------------ | ------ | ----------------------------------------------------------------------------------------------------------- |
| <p><strong>cobrowse.api</strong><br><strong></strong>required</p>        | string | <p>eg. <code>https://cobrowse.yourcompany.com</code><br><br>default is <code>https://cobrowse.io</code></p> |
| <p><strong>cobrowse.license</strong><br><strong></strong>required</p>    | string | your license key                                                                                            |
| <p><strong>cobrowse.debug</strong><br><strong></strong>optional</p>      | string | <p><code>true</code> / <code>false</code><br><br>default is <code>false</code></p>                          |
| <p><strong>cobrowse.initialUrl</strong><br><strong></strong>optional</p> | string | Override initial Url the plugin tab will load                                                               |

<figure><img src="../../../.gitbook/assets/genesys_wde_gax_settings.png" alt=""><figcaption><p>Screenshot of GAX configuration for interaction-workspace</p></figcaption></figure>

### 3. Start Workspace Desktop Edition

You should now see the COBROWSE tab within your Workspace Desktop Edition when interacting with an end-user, such as on a phone call, or via a live chat.&#x20;

Demo video - [https://www.youtube.com/watch?v=IuDv8YqcUa0](https://www.youtube.com/watch?v=IuDv8YqcUa0)

## Troubleshooting

### Why is the COBROWSE tab always blank?

Please verify if your version of Genesys Workspace Desktop Edition is 8.5.147.05+. If you are running an earlier version, or if you are running a supported version which still faces this issue, please contact us at [hello@cobrowse.io](mailto:hello@cobrowse.io).

### How do I debug the COBROWSE tab?

Please enable the optional setting `cobrowse.debug = true` as described above.&#x20;

This will provide additional logging from the COBROWSE tab to the Genesys Workspace Desktop Edition logs.&#x20;

The plugin will also activate remote debugging of the web app on port `9222`. Open MS Edge, go to `edge://inspect/#devices` and the plugin will appear there:

<figure><img src="../../../.gitbook/assets/edge_debugging.png" alt=""><figcaption></figcaption></figure>

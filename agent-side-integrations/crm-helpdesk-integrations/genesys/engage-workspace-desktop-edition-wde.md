---
description: Plugin version v1.0.0
---

# Engage Workspace Desktop Edition (WDE)

## Installation

Supported WDE versions: 8.5.147.05+.

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

### 3. Start Workspace Desktop Edition

You should now see the COBROWSE tab within your Workspace Desktop Edition when interacting with an end-user, such as on a phone call, or via a live chat.&#x20;

Demo video - [https://www.youtube.com/watch?v=IuDv8YqcUa0](https://www.youtube.com/watch?v=IuDv8YqcUa0)


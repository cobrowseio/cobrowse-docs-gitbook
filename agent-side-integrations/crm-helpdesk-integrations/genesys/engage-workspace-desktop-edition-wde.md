# Engage Workspace Desktop Edition \(WDE\)

## Installation

### 1. Install plugin

Add the plugin to your Workspace Desktop Edition installation:

* Genesyslab.Desktop.Modules.CobrowseIO.dll
* Genesyslab.Desktop.Modules.CobrowseIO.module-config

These files should be added adjacent to your other Module `.dll` files, in the same directory as `InteractionWorkspace.exe`. This executable is most commonly found at`C:\Program Files (x86)\GCTI\Workspace Desktop Edition`.

Cobrowse.io can provide these files to you upon request, free of charge, in order to conduct a proof-of-concept \(PoC\) or other technical evaluation.

### 2. Add required config

The configuration values can be added via Genesys GAX, Genesys Administrator, or directly in a `InteractionWorkspace.exe.config` file adjacent to your `InteractionWorkspace.exe`

When using Genesys GAX, please navigate to Environment/Applications -&gt; WorkspaceDesktop -&gt; Application Options -&gt; interaction-workspace and add the following fields there. 

| Field | Type | Value |
| :--- | :--- | :--- |
| cobrowse.api | string | eg. https://cobrowse.yourcompany.com |
| cobrowse.license | string | your license key |

### 3. Start Workspace Desktop Edition

You should now see the COBROWSE tab within your Workspace Desktop Edition when interacting with an end-user, such as on a phone call, or via a live chat. 

Demo video - [https://www.youtube.com/watch?v=IuDv8YqcUa0](https://www.youtube.com/watch?v=IuDv8YqcUa0)




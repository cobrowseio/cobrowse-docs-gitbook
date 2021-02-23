# Engage Workspace Desktop Edition \(WDE\)

## Installation

### 1. Install plugin

Installation requires adding two files to your Workspace Desktop Edition installation:

* Genesyslab.Desktop.Modules.CobrowseIO.dll
* Genesyslab.Desktop.Modules.CobrowseIO.module-config

These files should be added adjacent to your other Module dll files, in the same directory as `InteractionWorkspace.exe`. This executable is most commonly found at`C:\Program Files (x86)\GCTI\Workspace Desktop Edition`.

Cobrowse.io can provide these files to you upon request, free of charge, in order to conduct a proof-of-concept \(PoC\) or other technical evaluation.

### 2. Add required config

The configuration values can be added via Genesys GAX, Genesys Administrator, or directly in a `InteractionWorkspace.exe.config` file adjacent to your `InteractionWorkspace.exe`

| Field | Type | Value |
| :--- | :--- | :--- |
| cobrowse.api | string | eg. https://cobrowse.yourcompany.com |
| cobrowse.license | string | your license key |

## 




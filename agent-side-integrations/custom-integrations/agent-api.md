---
description: Client-side JavaScript SDK to build custom agent-side integrations
---

# Agent API \(alpha\)

## Overview

Our Agent API can be used to build 100% customized agent-side integrations into your own products and services. 

## Quick start

Navigate to our [Agent SDK Demo Page](https://cobrowse-agent-sdk-examples.cbrws.io/) and open up our [Cobrowse.io Online Demo](https://cobrowse.io/demo) in another browser tab to generate your Demo ID. 

This Demo Page uses our [Agent UI components library](https://github.com/cobrowseio/cobrowse-agent-ui), our [Agent JavaScript SDK](https://www.npmjs.com/package/cobrowse-agent-sdk), and ties it together into an [examples project](https://github.com/cobrowseio/cobrowse-agent-sdk-examples) which you can run locally. 

## Features

The Agent API can be used to:

* List Devices and/or Sessions matching your filter criteria
  * Build your own 100% customized dashboard or device list
* Subscribe to events on Devices and Sessions
  * Create a smart connect button, that changes color when the devices goes online/offline
* Interact with embedded Cobrowse.io iFrames
  * Get notified in the parent page when a session begins and ends
  * Attach a button in the parent page to end the session, or select the agent's tool
  * Programmatically end the session before the parent page closes



{% hint style="info" %}
The Agent API is currently in alpha release, and subject to change without notice. 
{% endhint %}








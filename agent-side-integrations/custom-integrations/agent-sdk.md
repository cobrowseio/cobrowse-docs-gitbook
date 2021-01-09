---
description: Client-side JavaScript SDK to build custom agent-side integrations
---

# Agent SDK \(beta\)

## Overview

Our Agent SDK can be used to build 100% customized agent-side integrations into your own products and services. 

## Quick start

Navigate to our [Agent SDK Demo Page](https://cobrowse-agent-sdk-examples.cbrws.io/) and open up our [Cobrowse.io Online Demo](https://cobrowse.io/demo) in another browser tab to generate your Demo ID. 

This Demo Page uses our [Agent UI components library](https://github.com/cobrowseio/cobrowse-agent-ui), our [Agent JavaScript SDK](https://www.npmjs.com/package/cobrowse-agent-sdk), and ties it together into an [examples project](https://github.com/cobrowseio/cobrowse-agent-sdk-examples) which you can run locally. 

## Features

The Agent SDK can be used to:

* List Devices and/or Sessions matching your filter criteria
  * Build your own 100% customized dashboard or device list
  * Show only a single user's devices in the context of a support ticket or chat
* Subscribe to events on Devices and Sessions
  * Create a smart connect button, that changes color when the devices goes online/offline
* Interact with embedded Cobrowse.io iFrames
  * Get notified in the parent page when a session begins and ends
  * Attach a button in the parent page to end the session, or select the agent's tool
  * Programmatically end the session before the parent page closes

### List devices and sessions

```javascript
import CobrowseAPI from 'cobrowse-agent-sdk';

(async function() {
    const cobrowse = new CobrowseAPI();
    cobrowse.token = 'jwt token'; // generate a JWT (see docs)
    
    // list devices and sessions
    const [devices, sessions] = await Promise.all([
        cobrowse.devices.list(),
        cobrowse.sessions.list({state:['active', 'ended'], activated: new Date(0)})
    ]);
    
    // subscribe to updates for these resources
    devices.forEach(device => device.subscribe());
    sessions.forEach(sessions => sessions.subscribe());
    
    // listen for updates
    devices.forEach(device => device.on('updated', (d) => console.log(d)));
    sessions.forEach(session => session.on('updated', (s) => console.log(s)));
    
    // get details for a single device
    if (devices.length > 0) {
        console.log('last active', devices[0].last_active);
        console.log('custom data', JSON.stringify(devices[0].custom_data));
    }
}());
```

### Attach context to iFrame

```javascript
import CobrowseAPI from 'cobrowse-agent-sdk';

(async function() {
    const cobrowse = new CobrowseAPI(); // JWT not required
    
    // attach to iframe
    const frameEl = document.getElementById('myIframe');
    const ctx = await cobrowse.attachContext(frameEl);
    
    // listen for updates to session
    ctx.on('session.updated', (session) => {
        console.log('session was updated', session);
        if (session.ended) {
            console.log('session has ended');
            ctx.destroy();
        }
    });
    
    // interact with iframe
    const currentTool = await ctx.getTool();
    await ctx.setTool('laser');
    await ctx.clearAnnotations();
    await ctx.endSession();
}());
```

{% hint style="info" %}
The Agent SDK is currently in beta release. Some APIs may change.
{% endhint %}








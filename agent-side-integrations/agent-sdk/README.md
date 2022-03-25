---
description: Client-side JavaScript SDK to build custom agent-side integrations
---

# Agent SDK

Our Agent SDK can be used to build 100% customized agent-side integrations into your own products and services. You can use to both to access our API and to control embedded Cobrowse IFrames.

### Quick start demo

Navigate to our [Agent SDK Demo Page](https://cobrowseio.github.io/cobrowse-agent-sdk-examples/react-example/) and open up our [Cobrowse.io Online Demo](https://cobrowse.io/demo) in another browser tab to generate your Demo ID.&#x20;

This Demo Page uses our [Agent UI component library](https://github.com/cobrowseio/cobrowse-agent-ui), our [Agent SDK](https://www.npmjs.com/package/cobrowse-agent-sdk), and ties it together into an [Agent SDK Examples project](https://github.com/cobrowseio/cobrowse-agent-sdk-examples).&#x20;

### Authentication

If you are using the Agent SDK just to communicate to an embedded IFrame, a JWT is not required.&#x20;

Device and Session listing via the Agent SDK requires a JSON Web Token (JWT) for authentication. Learn how to generate a JWT for your account at [Authentication (JWTs)](../json-web-tokens-jwts/).&#x20;

### Features

The Agent SDK can be used to:

* List Devices and/or Sessions matching your filter criteria
  * Build your own 100% customized dashboard or device list
  * Show only a single user's devices in the context of a support ticket or chat
* Subscribe to events on Devices and Sessions, for example to create a smart connect button that indicates when the devices goes online/offline
* Interact with embedded Cobrowse iFrames
  * Get notified in the parent page when a session begins and ends
  * Attach a button in the parent page to end the session, or select the agent's tool
  * Programmatically end the session before the parent page closes

We have put together a list of commonly ask questions and useful code snippets to get you started:

{% content-ref url="how-do-i....md" %}
[how-do-i....md](how-do-i....md)
{% endcontent-ref %}

You can find the full developer documentation for the agent SDK API here:

{% content-ref url="api-reference.md" %}
[api-reference.md](api-reference.md)
{% endcontent-ref %}


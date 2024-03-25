---
description: >-
  Cobrowse provides a range of embeddable URLs to suit a range of use cases,
  including full dashboard embeds, direct device connection or screen-share
  only.
---

# IFrame embeds

We provide a number of different embeddable URLs to suit a range of use cases:

#### Full Dashboard Embed

We recommend embedding the Cobrowse Dashboard URL (`https://cobrowse.io/dashboard`) as an IFrame in your CRM if you are looking for the simplest integration options. The full range of Cobrowse features will be available through our default UIs. You can pre-filter device or session lists it as described below in the query params section to show specific devices in different contexts.

#### 6 Digit Codes Only

If you are using our 6-digit codes to initiate sessions, and that is the only mechanism to start sessions you'd like to support for your agents, we have a simplified Cobrowse Code URL (`https://cobrowse.io/code`) that you can embed.

#### Direct Device Connection

Using the Cobrowse Connect URL (`https://cobrowse.io/connect`), Cobrowse will automatically try to connect to the matching device. If multiple devices match the filters you provide, you will be prompted which to select. Make sure to set customData in our SDKs, and use those values in your filters!

#### Screenshare Only

If you just want to embed the UI for an active session, or even just the video feed, and will replace all other UI with your own implementations, then this is the endpoint you should use: `https://cobrowse.io/session/<session id>`. You will need to replace the `<session id>` with a valid session ID. See our [Agent SDK](agent-sdk/) docs.

{% hint style="info" %}
Not sure which embed URL is right for you? Get in touch with us at [hello@cobrowse.io](mailto:hello@cobrowse.io) and we'll be happy to advise for your use case.
{% endhint %}

### Embed Configuration Options

All of our embed URLs support configuration via the parameters described here. We support several query parameters to configure what is shown:

| Query Parameter      | Description                                                                                                                                                                                                                                                                                                                                                                                           |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **token**            | Provide a [JWT](json-web-tokens-jwts/) to auto-authenticate the specified user. For example `?token=eyJh...dCI6`                                                                                                                                                                                                                                                                                      |
| **filter\_\[name]**  | <p>Filter the list of devices based on any metadata specified in your SDK integration. You may add multiple filters as additional query parameters. All filters are ANDed together when filtering the device list.</p><p>Please prefix each with "filter_" and ensure the [name] matches the field used in your SDK integration.<br>For example, <code>?filter_user_email=john@example.com</code></p> |
| **navigation**       | Set to `none` to remove the navigation bar. For example `?navigation=none`                                                                                                                                                                                                                                                                                                                            |
| **end\_action**      | <p>Set to <code>dashboard</code> to redirect back to the device list (/dashboard) on session end. This is the default behaviour.</p><p>Set to <code>none</code> will show a simple session ended message.</p><p>Set to <code>code</code> will return to the full screen code entry interface (/code).</p><p>For example <code>end_action=none</code></p>                                              |
| **popout**           | Set to `none` to disable the popout button when in a session. For example`?popout=none`                                                                                                                                                                                                                                                                                                               |
| **agent\_tools**     | Set to `none`  to remove the agent tool selection UI. This is useful when replacing our default UI with your own.                                                                                                                                                                                                                                                                                     |
| **device\_controls** | Set to `none`  to remove the device options UI (e.g. full device mode switch). This is useful when replacing our default UI with your own.                                                                                                                                                                                                                                                            |
| **session\_details** | Set to `none` to remove the device and custom data pull-up footer in the session view.                                                                                                                                                                                                                                                                                                                |
| **messages**         | Set to `none` to remove messages overlaid onto the screen to indicate events such as "Device not responding". This is useful when replacing our default UI with your own.                                                                                                                                                                                                                             |

This logic enables you to embed the relevant IFrame data on each place in your CRM. For example, when an agent is looking at a user account view in your CRM, you might use filters to show all of that user's available devices.

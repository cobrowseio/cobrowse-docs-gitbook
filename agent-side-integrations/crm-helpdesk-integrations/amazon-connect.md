---
description: Easily start a Cobrowse session from Amazon Connect
---

# Amazon Connect

## Overview

Cobrowse.io provides an integration with Amazon Connect, enabling real-time
visual collaboration between agents and customers.

{% embed url="https://vimeo.com/1165968102?share=copy" %}
Cobrowse for Amazon Connect
{% endembed %}


## Installation guide

To install and configure the Cobrowse.io integration with Amazon Connect:

1. Sign up for a [Cobrowse.io account](https://cobrowse.io/register) and
   configure a [SAML 2.0 integration](../authentication-saml-2.0.md) on your
   Cobrowse account. The integration uses SAML authentication to verify agent
   identities, so this must be set up before proceeding.
2. As an AWS privileged user, create an
   [Amazon Connect third-party application](https://docs.aws.amazon.com/connect/latest/adminguide/3p-apps.html#onboard-3p-apps-how-to-integrate)
   using the following information:
   * Display name: `Cobrowse`
   * Application type: `Standard application`
   * Contact scope: `Per contact`
   * Access URL:
     `https://cobrowse.io/api/1/amazonconnect/launch?license=<your cobrowse license key>`
3. Under instance association, associate the application with your Amazon
   Connect instance.
4. Ensure your Amazon Connect roles have access to the Cobrowse application.
   See [Controlling user access](#controlling-user-access)

### Enabling Cobrowse.io for the chat widget

The Cobrowse device ID must be passed to the chat widget. Add the following to
your snippet:

```javascript
amazon_connect("contactAttributes", {
  cobrowse_device_id: deviceId,
});
amazon_connect("registerCallback", {
  PARTICIPANT_JOINED: (_, { chatDetails }) => {
    CobrowseIO.customData = {
      amazon_connect_contact_id: chatDetails?.contactId,
    };
  },
});
```

Ensure the [Cobrowse.io SDK](../../sdk-installation/web.md) is included on the
webpage where the chat widget is running. No further changes are required on the
Amazon Connect side to enable cobrowsing.

## Controlling user access

As an Amazon Connect administrator, grant your Amazon Connect roles access to
the Cobrowse.io application by navigating to _Connect Workspace_ > _Users_ >
_Security Profiles_. Edit each role that requires access to the Cobrowse.io
application. Under the _Agent Applications_ section, grant _Access_ to the
Cobrowse application.
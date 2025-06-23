---
description: >-
  Cobrowse integrates seamlessly with Omnichannel for Dynamics 365 Customer
  Service, enabling real-time visual collaboration between agents and customers.
  This integration is available through the Microsoft AppSource store.
---

# Dynamics 365 - Customer Service

## Overview

Cobrowse integrates seamlessly with Omnichannel for Dynamics 365 Customer
Service, enabling real-time visual collaboration between agents and customers.
This integration is available through the [Microsoft AppSource store](TODO).

<!-- TODO: Link to AppSource listing -->

<!-- TODO: Create video
{% embed url="https://vimeo.com/924748894?share=copy" %} Cobrowse for Omnichannel for Dynamics 365 Customer Service
{% endembed %}
-->

## Installation guide

To install and configure the Cobrowse integration with Dynamics 365:

1. Install the app from the [Microsoft AppSource store](TODO).
2. _(Optional)_ If you are
   [self-hosting](../../../enterprise-self-hosting/self-hosting-overview.md)
   your Cobrowse.io instance, follow the
   [self-hosted instructions](#self-hosted).
3. As a Dynamics administrator, open the Cobrowse.io application from the app
   launcher. Under **Setup**, select the **OAuth Consent** link. This will
   generate a URL that must be approved by an Azure administrator.

### Configuring chat channels

To enable Cobrowse functionality for specific chat channels:

1. Navigate to the **Copilot Service Admin Center** for your organization.
2. Go to **Channels > Chat > Manage**.
3. Edit your desired channel and open **User features**.
4. Toggle the **Co-browse** feature on and select **Cobrowse.io** as the
   provider.

### Add our SDKs to get started!

See [Getting started](../../) to add our SDKs and begin end-to-end testing! Your
license key can be found in your **Account Settings**, seen above.

### Self hosted

When [self-hosting](../../../enterprise-self-hosting/self-hosting-overview.md)
the Cobrowse.io instance, there are two preliminary steps required:

1. Update the `cobrowse_api` environment variable

   1. Visit [make.powerapps.com](https://make.powerapps.com).
   2. Select the environment where the Cobrowse application is deployed
   3. Navigate to **Solutions** and open the **Default Solution**
   4. Update the value to your self-hosted instance URL, including protocol and
      hostname (e.g., `https://cobrowse.example.com`).

2. Register an OAuth application.

   1. As an azure administrator, go to the
      [Azure portal](https://portal.azure.com) >
      [Microsoft Entra ID](https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Overview) > +
   2. Select **+ Add** > **App registration** and configure

      1. **Name**: `Cobrowse.io Application`
      2. **Supported account types**:
         `Accounts in this organizational directory only`
      3. **Redirect URI**

         **Platform**: `Web`

         **URL**:
         `https://<your cobrowse instance>/api/1/dynamics/auth/callback`

   3. Click **Register**
   4. After registration:

      - Note the **Application (Client) ID**.
      - Create a client secret under \*\*Certificates & secrets

   5. In your cobrowse.io deployment, set the following environment variables:

   ```
   dynamics__client_id=<your client id>
   dynamics__client_secret=<your client secret>
   ```

## Controlling user access

The Cobrowse Dynamics application includes two roles for managing user
permissions:

1. `Cobrowse.io - Administrator`

   Grants access to the Cobrowse.io application via the app launcher. Users with
   this role can view active devices, sessions, and settings.

2. `Cobrowse.io - Agent`

   Grants access to the Cobrowse sidebar, allowing agents to initiate cobrowse
   sessions during customer interactions.

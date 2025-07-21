---
description: >-
  Cobrowse integrates seamlessly with Microsoft Dynamics 365 Customer Service,
  enabling real-time visual collaboration between agents and customers. This
  integration is available through the Microsoft AppSource store.
---

# Microsoft Dynamics 365 - Customer Service

## Overview

Cobrowse integrates seamlessly with Dynamics 365 Customer Service, enabling
real-time visual collaboration between agents and customers. This integration is
available through the
[Microsoft AppSource store](https://appsource.microsoft.com/en-us/product/dynamics-365/cobrowseiollc.integration).

{% embed url="https://vimeo.com/1095905912?share=copy" %} Cobrowse for Microsoft
Dynamics 365 Customer Service {% endembed %}

## Installation guide

To install and configure the Cobrowse integration with Microsoft Dynamics 365:

1. Install the app from the
   [Microsoft AppSource store](https://appsource.microsoft.com/en-us/product/dynamics-365/cobrowseiollc.integration).
2. _(Optional)_ If you are
   [self-hosting](../../../enterprise-self-hosting/self-hosting-overview.md)
   your Cobrowse.io instance, follow the
   [self-hosted instructions](#self-hosted).
3. Sign into your Microsoft Dynamics 365 installation and assert you have a
   verified email address. You can verify in **Copilot Service Admin Center** >
   User management > Users > Manage > Your user > **Approve Email**
4. As a Microsoft Dynamics administrator, open the Cobrowse.io application from
   the app launcher. Under **Setup**, select the **OAuth Consent** link. This
   will generate a URL that must be approved by an Azure administrator.

### Enabling Cobrowse for live chat

To enable Cobrowse functionality for live chat channels:

1. Navigate to the **Copilot Service Admin Center** for your organization.
2. Go to **Channels > Chat > Manage**.
3. Edit your desired channel and open **User features**.
4. Toggle the **Co-browse** feature on and select **Cobrowse.io** as the
   provider.

Note that to use the Cobrowse user feature with the Customer Service live chat
widget, you must integrate the [Cobrowse SDK](../../sdk-installation/web.md)
into the webpage where the widget is hosted.

### Enabling Cobrowse for other channel types

Cobrowse functionality is also available for other communication channels such
as voice, WhatsApp, and more. While the agent will not automatically see the
user’s device in these cases, a cobrowsing session can still be initiated using
a [6-digit code](../../sdk-features/6-digit-codes.md).

If your channel is already set up, follow these steps to enable Cobrowse:

1. Navigate to the **Copilot Service Admin Center** for your organization.
2. Go to **Workspaces > Session Templates > Manage**.
3. Select the session template associated with your channel and click **Edit**.
4. Under **Additional Tabs**, choose **Add Existing Application**
5. Search for **Cobrowse.io Tab** and add it to the list
6. Click **Save & Close**

Note: Default session templates cannot be edited. To work around this, create a
new session template, replicate the settings from the default template, and then
add the Cobrowse.io application tab.

This will add Cobrowse as a default tab when agents open an interaction.

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
   4. Narrow down the object list to `Environment variables`, search for
      `API base`. Identify the `cobrowse_api` variable and `Edit` it.
   5. Set the `Current Value` the value to your self-hosted instance URL,
      including protocol and hostname (e.g., `https://cobrowse.example.com`).

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

The Cobrowse Microsoft Dynamics application includes two roles for managing user
permissions:

1. `Cobrowse.io - Administrator`

   Grants access to the Cobrowse.io application via the app launcher. Users with
   this role can view active devices, sessions, and settings.

2. `Cobrowse.io - Agent`

   Grants access to the Cobrowse sidebar, allowing agents to initiate cobrowse
   sessions during customer interactions.

Note that in order for your users to authenticate into Cobrowse, their email
address has to be verified in Microsoft Dynamics 365.

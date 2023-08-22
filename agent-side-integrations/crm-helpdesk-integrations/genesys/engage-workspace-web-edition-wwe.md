---
description: Engage Cloud and Engage On-Premise WWE
---

# Engage Workspace Web Edition (WWE)

## Installation

### 1. Define the Interaction Tab

Define a new Interaction Tab in your WWE Cluster configuration using Genesys GAX or Genesys Administrator.

{% hint style="info" %}
If you are using Engage Cloud (AWS or Azure), you may configure the Interaction Tab following the instructions at [https://all.docs.genesys.com/PEC-AS/Current/ManageCC/External\_URLs](https://all.docs.genesys.com/PEC-AS/Current/ManageCC/External\_URLs).&#x20;
{% endhint %}

You may name the tab as you wish, for example `CobrowseIOTab`. The tab should have the following values:

* **label** – The label that should be displayed in the tab within the WWE UI, eg. COBROWSE
* **url** – The URL to the web content that will be displayed within the tab.  The URL can also contain variable names pertaining to agent and interaction data. This value will be provided by Cobrowse.io. The default value when using the Cobrowse.io hosted version is `https://cobrowse.io/apps/genesys/index.html?env=wwe&interactionId=$Interaction.Id$&license=your license key`

More information about how to add a new Interaction Tab to Workspace Web Edition (WWE) available at [https://developer.genesys.com/customizing-genesys-workspace-web-edition-2/](https://developer.genesys.com/customizing-genesys-workspace-web-edition-2/).

Next, navigate to the `interaction-workspace/interaction.web-content` and add the value `CobrowseIOTab` to this list to enable it.&#x20;

### 2. Start Workspace Web Edition

You should now see the COBROWSE tab within your Workspace Web Edition when interacting with an end-user, such as on a phone call, or via a live chat.&#x20;

Demo video - [https://www.youtube.com/watch?v=ZKx5bOsodtw](https://www.youtube.com/watch?v=ZKx5bOsodtw)

## Configuring SSO

If you wish to sign into Cobrowse from within WWE using SSO you must first follow our documentation to [setup SAML](https://docs.cobrowse.io/agent-side-integrations/authentication-saml-2.0).

With SAML configured, visit the [integration settings](https://cobrowse.io/dashboard/settings/integrations) page of your Cobrowse dashboard and look to the **login page** URL seen within the **SAML Config** section. It should look something like `https://<DOMAIN>/login/saml/<PROVIDER_ID>`.

Take note of your `<PROVIDER_ID>`. It should a string of 6 characters after the `/login/saml/` path seen in the login page URL.

Update **cobrowse.initialUrl** to the URL below, replacing `<DOMAIN>` with the domain of the Cobrowse instance, often `cobrowse.io`, `<PROVIDER_ID>` with the 6 characters that you took note of in the previous step and `<YOUR_LICENSE_KEY>` with the license key for your account.

`https://<DOMAIN>/api/1/saml/auth?provider=<PROVIDER_ID>&redirectTo=%2Fapps%2Fgenesys%2Findex.html%3Fenv%3Dwde%26interactionId%3D%24Interaction.Id%24%26license%<YOUR_LICENSE_KEY>`

Now when opening the COBROWSE tab from within WWE you should be taken to your SSO provider allowing you to sign in and be authenticated within WWE.


# Freshworks

## Overview

Cobrowse.io provides an integration with Freshdesk and Freshchat available in the Freshdesk and Freshchat App Marketplaces.&#x20;

## App Marketplace links

You can view our app descriptions and see more information for each platform below.

**Freshdesk**: [https://www.freshworks.com/apps/freshdesk/cobrowseio\_1/](https://www.freshworks.com/apps/freshdesk/cobrowseio\_1/)

**Freshchat**: [https://www.freshworks.com/apps/freshchat/cobrowse\_io/](https://www.freshworks.com/apps/freshchat/cobrowse\_io/)

## Installation instructions

Register for an account at [https://cobrowse.io/register.](https://cobrowse.io/register) **Note**: you only need to register for an account once. You can use the same account details in both Freshdesk and Freshchat.

Make note of your account licence key at [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard).

Generate a new private/public key pair following our article: [https://support.cobrowse.io/generating-cryptographic-keys-for-jwt-authentication](https://support.cobrowse.io/generating-cryptographic-keys-for-jwt-authentication)

Make sure you can access the contents of your `private.pem` file, as you will need to copy it later.  For example, using the command `cat private.pem` in the directory of your downloaded key will print the contents to a terminal window.

{% hint style="warning" %}
You must keep your private key secure at all times as it grants access to your account.
{% endhint %}

Install the Freshdesk or Freshchat app from the app gallery (Admin -> Settings -> Apps and search cobrowse.io) or from the correct marketplace links above.

Add your licence key and private key when prompted (include the first and final lines that look like  `-----BEGIN/END PRIVATE KEY-----`). Cobrowse.io API can be left as-is, unless you are using an account on our EU Cloud or a self-hosted instance.&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2024-02-16 at 10.24.55.png" alt=""><figcaption><p>Add your licence key and private key in the boxes. </p></figcaption></figure>

Afterwards, click install for the app to install. You can check successful installation by going to Manage Apps in the Admin settings to see if Cobrowse.io is there and enabled.

<figure><img src="../../.gitbook/assets/Screenshot 2024-02-16 at 10.35.31.png" alt=""><figcaption><p>The app is installed and enabled. </p></figcaption></figure>

The app should then appear in the sidebar during Freshchats and Freshdesk tickets and contacts. If you do not see it then please refresh the page (you may need to log out/in again).

To connect to user devices, your next step is to install our SDKs for your platforms. You can find the links for each platform here: [https://docs.cobrowse.io/](https://docs.cobrowse.io/).

### Group based access control

You can restrict access to Cobrowse.io based on user groups. To do this, navigate to the Cobrowse.io app's settings page on Freshdesk/Freshchat and select the **Show advanced settings checkbox**. This action will display additional configuration options. By entering the appropriate Freshdesk or Freshchat API credentials, a multi-select input will appear. This input allows you to specify which groups can access Cobrowse.io. If no groups are selected, Cobrowse.io will be accessible to all groups.

#### Freshdesk

The sub-domain is the initial segment of your Freshdesk URL. For instance, if your URL is _https://mycompany.freshdesk.com_, the sub-domain is _mycompany_.

You can find the API key in your user profile settings. From the dashboard page, click on the profile icon located at the top right corner, then select **Profile settings**. Clicking on the **View API Key** button will display the API key you need to input into the app's settings.

#### Freshchat

{% hint style="warning" %}
The Freshchat API is only available on the **Pro** and **Enterprise** plans.
{% endhint %}

Your Freshchat sub-domain and API key can be found in the profile settings within the Freshworks sales area.

{% hint style="info" %}
The Freshchat dashboard is divided into two separate sections: sales and messaging. To access the correct settings, ensure you are in the sales area. You can verify this by checking the page URL, which should be _https://mycompany.myfreshworks.com/crm/sales/..._. If you see _/crm/messaging_ instead of _/crm/sales_, navigate to a different section (e.g., Admin Settings or Contacts pages).
{% endhint %}

Click on the profile icon at the top right corner of the page, then select **Settings**. In the Profile Settings page, click on **API Settings**. The **API DETAILS FOR CHAT** contains the information we need to copy over to the app's settings. Copy the API key from **Your API Key** and the sub-domain from **Your chat URL** (e.g., _mycompany-945894b457ce01117278801_).

<figure><img src="../../.gitbook/assets/Screenshot 2024-02-16 at 11.13.58.png" alt=""><figcaption><p>Copy the API key and sub-domain.</p></figcaption></figure>

## Connect from Freshchat

After you have installed our SDK on the same websites or apps as your Freshchat widget, then you should be able to connect to users in one click from live Freshchats.

## Connect from Freshdesk

For Freshdesk, there is also a standalone app view which appears in the left-hand side bar.

<figure><img src="../../.gitbook/assets/Screenshot 2022-11-23 at 14.19.27.png" alt=""><figcaption><p>The app, once loaded and our SDKs have been installed into your website/apps.</p></figcaption></figure>

After installation, you can then connect to the correct device of the customer to cobrowse: [https://support.cobrowse.io/how-do-i-connect-to-the-right-device.](https://support.cobrowse.io/how-do-i-connect-to-the-right-device)

#### Connect from the ticket or contact view

Requires:

* The installation steps to be complete
* `user_email` custom data to be set on the SDK-side, that matches the email in the ticket/contact

You will see the device appear to connect to in the sidebar app!




# Freshdesk

## Overview

Cobrowse.io provides an integration with Freshdesk available in the Freshdesk App Marketplace.&#x20;

## Freshdesk App Marketplace link

You can view our app description and see more information here: [https://www.freshworks.com/apps/freshdesk/cobrowseio\_1/](https://www.freshworks.com/apps/freshdesk/cobrowseio\_1/)

## Installation instructions

Register for an account at [https://cobrowse.io/register.](https://cobrowse.io/register)

Make note of your account licence key at [https://cobrowse.io/dashboard](https://cobrowse.io/dashboard).

Generate a new private/public key pair following our article: [https://support.cobrowse.io/generating-cryptographic-keys-for-jwt-authentication](https://support.cobrowse.io/generating-cryptographic-keys-for-jwt-authentication)

Make sure you can access the contents of your `private.pem` file, as you will need to copy it later.  For example, using the command `cat private.pem` in the directory of your downloaded key will print the contents to a terminal window.

{% hint style="warning" %}
You must keep your private key secure at all times as it grants access to your account.
{% endhint %}

Install the Freshdesk app from the app gallery (Admin -> Settings -> Apps and search cobrowse.io) or from the marketplace link: [https://www.freshworks.com/apps/freshdesk/cobrowseio\_1/.](https://www.freshworks.com/apps/freshdesk/cobrowseio\_1/)

Add your licence key and private key when prompted (include the first and final lines that look like  `-----BEGIN/END PRIVATE KEY-----`). Cobrowse.io API can be left as-is, unless you are using an account on our EU Cloud or a self-hosted instance.&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2022-11-23 at 14.04.57.png" alt=""><figcaption><p>Add your licence key and private key in the boxes. </p></figcaption></figure>

Afterwards, click install for the app to install. You can check successful installation by going to Manage Apps in the Admin settings to see if Cobrowse.io is there and enabled.

<figure><img src="../../.gitbook/assets/Screenshot 2022-11-23 at 14.09.40.png" alt=""><figcaption><p>The app is installed and enabled. </p></figcaption></figure>

The app should then appear in the left-hand sidebar after one or two page refreshes (you may need to log out/in again).

<figure><img src="../../.gitbook/assets/Screenshot 2022-11-23 at 14.19.27.png" alt=""><figcaption><p>The app, once loaded and our SDKs have been installed into your website/apps.</p></figcaption></figure>

To make devices appear (as in the screenshot), your next step is to install our SDKs for your platforms. You can find the links for each platform here: [https://docs.cobrowse.io/](https://docs.cobrowse.io/)

You can then connect to the correct device of the customer to cobrowse with them: [https://support.cobrowse.io/how-do-i-connect-to-the-right-device.](https://support.cobrowse.io/how-do-i-connect-to-the-right-device)

## Connect from Freshchats (within Freshdesk)

Requires:

* The installation steps to be complete
* Your [Freshdesk account to be linked with your Freshchat account](https://support.freshchat.com/en/support/solutions/articles/50000000131-freshdesk-support-desk-integration)
* Freshchat to be operational on your website, e.g. chat deployed successfully
* [6-digit support code connection flow](https://support.cobrowse.io/how-do-i-connect-to-the-right-device) implemented successfully

Within your Freshdesk account, serve chats with the Cobrowse.io app in the background. Ask users to generate support codes using your supported method, and send them to you in the chat!

<figure><img src="../../.gitbook/assets/Screenshot 2022-11-23 at 15.14.14.png" alt=""><figcaption></figcaption></figure>

## Connect in ticket/contact view

Requires:

* The installation steps to be complete
* `user_email` custom data to be set on the SDK-side, that matches the email in the ticket/contact

You will see the device appear to connect to in the sidebar app!




---
description: v2.x
---

# Salesforce

## Overview

The new version of our Salesforce integration is now available. System admins can follow the steps below in order to give your organization access to Cobrowse.io within Salesforce.

**Migrating from version 1? Follow the instructions here first:** [**Migrating from legacy to v2**](migrating-from-legacy-to-v2.md)**.**

## Install the new app

Search for 'Cobrowse.io' in the App Marketplace and select our application. Follow the install guidance and complete the information until you see the screen below.

Select Install for All Users at the install page.

![Select Install for All Users on the install page.](../../../.gitbook/assets/screenshot-2021-09-01-at-22.28.03.png)

This will allow all users to access the application after permissions have been granted within Salesforce \(as shown later\). The install process will take a few minutes.

Verify that the app has been installed by searching for Cobrowse.io in the App Launcher search bar.

![Searching for Cobrowse.io should show that the app has installed successfully.](../../../.gitbook/assets/screenshot-2021-09-01-at-22.36.37.png)

Click on either option and the below screen should appear.

![The Cobrowse.io screen should show a message refusing to connect.](../../../.gitbook/assets/screenshot-2021-09-01-at-22.38.50.png)

This is expected and verifies that the application has been installed correctly. The next step is to give appropriate access to the users in your organisation \(including admins\).

## Grant access to Users

Go to Setup and search for App Manager. Select Manage for the CobrowseIOWebApp as shown below.

![Manage the CobrowseIOWebApp to grant access to Users.](../../../.gitbook/assets/screenshot-2021-09-01-at-22.42.58.png)

Select Edit Policies and change Permitted Users to "Admin Approved users are pre-authorized" and then Save.

![Select Admin approved users are pre-authorized to enable access to users in your organization.](../../../.gitbook/assets/screenshot-2021-09-01-at-22.44.44.png)

## Managing access using Salesforce Profiles 

If you allow manage access using Salesforce Profiles and require all users within your organisation to access Cobrowse.io then follow the steps below. If you require specific Permission Sets, skip to the next section.

Next select Manage Profiles from the Profiles section of the Manage page display. 

![Select Manage Profiles to allow user profiles access to Cobrowse.io. ](../../../.gitbook/assets/screenshot-2021-09-01-at-22.47.33.png)

Within the Manage Profiles options select System Administrator and Standard User then Save. 

Search Cobrowse.io in the App Launcher search bar again and go back to the dashboard by selecting either option.

![Search for Cobrowse.io again in the App Launcher and go to the dashboard.](../../../.gitbook/assets/screenshot-2021-09-01-at-22.53.59.png)

The Cobrowse.io dashboard should now be visible to both administrators and support agents. Account settings from the previous integration will have been preserved.

## Managing access using Salesforce Permission Sets

If you manage access using Salesforce Permission Sets, then follow the instructions below. Otherwise, use the previous section to enable Cobrowse.io for all Users in your organisation using Salesforce Profiles.  

Due to an underlying Salesforce restriction, we will need to clone the Permission Sets that are shipped with our App when it is installed in order to enabled them. 

To do this, go to your organisation's Permission Sets tab and Clone the Cobrowse.io Agent and Cobrowse.io Admin Permission Sets. **Note: clone them without making any changes to the Permission Sets.** This will mean that two Permission Sets for both Cobrowse.io Agent and Cobrowse.io Admin appear, this is expected. 

![Click the Clone button on the Permission Sets and save them without changing anything else.](../../../.gitbook/assets/screenshot-2021-09-14-at-16.42.46.png)

Go back to the App Manager and manage CobrowseIOWebApp again. Select Manage Permission Sets and add both of the cloned Cobrowse.io Agent and Cobrowse.io Admin Permission Sets to the App. The correct ones are the ones that are able to be added, the non-cloned ones will show an error message. 

![Add the cloned Permission Sets to the CobrowseIOWebApp, leave the original in place.](../../../.gitbook/assets/screenshot-2021-09-14-at-16.44.57.png)

Next, simply add the relevant Permission Sets to the Profiles or Users who you wish to access Cobrowse.io. Make sure to correctly add the appropriate Permission Set \(either Admin or Agent\) and to add both copies of the Permission Set to be sure the right one has been added, e.g. both Cobrowse.io Agent Permission Sets for each agent who does not have admin rights.

![Add the Permission Sets to the correct Users \(or Profiles\)](../../../.gitbook/assets/screenshot-2021-09-14-at-16.46.41.png)

The Cobrowse.io application will now appear to your agents and admins who have been granted the correct permissions.

## Sidebar widget placement

You can also add the Cobrowse.io widget to your record pages for Cases, Contacts, Accounts, or any other record pages accessible via Sales Console, Service Console, or otherwise. To do this, navigate to the  page you would like to add the widget to and click the settings button in the top right to "Edit Page".

![Select Edit Page to add the sidebar widget. It has already been added to the Contact Record page above.](../../../.gitbook/assets/screenshot-2021-09-01-at-23.12.10.png)

You can then drag and drop the Cobrowse.io Sidebar managed component \(from Custom - Managed on the left\) into the location of your choice and Save.

![Drag and drop the Cobrowse.io Sidebar component.](../../../.gitbook/assets/screenshot-2021-09-01-at-23.15.09.png)

The sidebar widget should then be displayed in all of the pages that it has been added to.

## Configuring Cobrowse.io Account Settings

[Account Settings](../../../sdk-features/account-configuration.md) can be configured by an admin user by going to the Cobrowse.io application and clicking on the Account Settings icon.

![Configure your Cobrowse.io Account Settings](../../../.gitbook/assets/screenshot-2021-09-20-at-11.34.44.png)

## Add our SDKs to get started!

See [Getting started](../../../) to add our SDKs and begin end-to-end testing! Your license key can be found in your Account Settings, seen above.

## Filtering devices using Salesforce Record metadata \(optional\)

This step is optional for clients who wish to override the default device filtering in the Cobrowse.io Sidebar widget. For example in Cases and Contacts, the sidebar widget will filter by devices that match the end user's email address by default.

In Salesforce legacy, this filtering was provided in the Cobrowse.io Settings tab. It is now located within the Cobrowse.io Dashboard. Go to Cobrowse.io Dashboard \(e.g. via App Launcher search\) then Account Settings \(Admins only\), then Integrations, then Salesforce record Mapping. 

![](../../../.gitbook/assets/screenshot-2021-09-01-at-23.25.24.png)

The first argument specifies the page for the filtering to be applied to, e.g. "Case". Then the key:value pair specifies the SalesforceMetadata:Cobrowse.ioCustomData that should be mapped together, linking the Salesforce metadata from a page to the corresponding Cobrowse.io custom data field to filter the devices.

In the example above \(and by default\), "ContactEmail" in Salesforce is mapped to "user\_email" in Cobrowse.io custom data. This means that "ContactEmail" filters the devices shown on the "Case" pages.

This information must match the correct metadata to work, and it must be formatted in correct JSON \(as the other examples are\).

See [https://docs.cobrowse.io/sdk-features/identify-your-devices](https://docs.cobrowse.io/sdk-features/identify-your-devices) for more information on using Cobrowse.io custom data to identify your devices.

If you have any questions about the above steps then please get in touch at [support@cobrowse.io](mailto:support@cobrowse.io)!

## Troubleshooting

### Granting multiple users Cobrowse.io Admin role permissions

As part of the install, you may need to grant multiple users the Admin role permissions within Cobrowse.io, e.g. so that they can see all agents' session history. This should happen automatically, but if it does not work then it can also be done manually.

Simply go the User's profile within Salesforce via Setup then Users and Users again. Select the User and inspect their Permission Set Assignments. Edit Assignments and add the Cobrowse.io Admin permission set to their profile. They will then have Cobrowse.io Admin rights which can be verified in the Cobrowse.io Dashboard.


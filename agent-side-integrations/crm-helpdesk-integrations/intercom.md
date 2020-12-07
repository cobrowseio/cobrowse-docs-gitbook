# Intercom

## Overview

Cobrowse.io provides an integration with Intercom available in the Intercom App Marketplace. Get started here [https://cobrowse.io/intercom](https://cobrowse.io/intercom)

## Troubleshooting

#### I can connect from https://cobrowse.io/dashboard, but not from within Intercom.

It's likely your Intercom account is linked to a separate Cobrowse.io account. When you install Cobrowse.io for Intercom, a new license key is generated for you. This may be different from an existing license key you are using when you signed up directly at [https://cobrowse.io/register](https://cobrowse.io/register). 

#### No devices found for user, cannot start cobrowse session.

Please ensure Cobrowse.io Javascript Snippet has been added to your website where you are using Intercom chat. Installation instructions at [Web SDK](../../sdk-installation/web.md). 

#### Agent is stuck on "waiting for authorization".

We are aware of a possible issue with React Native apps only, where the user consent prompt is hidden behind the Intercom SDK's UI, so it is not visible for the user to take action. To confirm this is what is happening, you can disable "Require user consent" from [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings) and try to start a new session via Intercom. 

#### Sorry, you do not have permissions to modify this app installation.

Intercom recently changed the way their OAuth tokens work, and it now requires users to have the "Can install, configure and delete apps" permission in order to use the Cobrowse.io Messenger App. 

To work around this problem, we have released the Cobrowse.io Inbox App, which lives in the sidebar of Intercom, and enables connecting to the end-user's device\(s\) with just one click. 

![Cobrowse.io Inbox App for Intercom](../../.gitbook/assets/intercom-inbox-app-cobrowse-io.png)




# Intercom

## Overview

Cobrowse.io provides an integration with Intercom available in the [Intercom App Store](https://www.intercom.com/app-store/?app\_package\_code=cobrowse-screenshare-permissions).&#x20;

## Install the app

Install the app from the [Intercom App Store](https://www.intercom.com/app-store/?app\_package\_code=cobrowse-screenshare-permissions) and follow the setup instructions!

## Troubleshooting

#### I can connect from https://cobrowse.io/dashboard, but not from within Intercom.

It's likely your Intercom account is linked to a separate Cobrowse.io account. When you install Cobrowse.io for Intercom, a new license key is generated for you. This may be different from an existing license key you are using when you signed up directly at [https://cobrowse.io/register](https://cobrowse.io/register).&#x20;

#### No devices found for user, cannot start cobrowse session.

Please ensure a Cobrowse.io SDK has been added to the platform where you are using Intercom chat. Installation instructions for each SDK are found at [Getting Started](../../).&#x20;

#### Agent is stuck on "waiting for authorization".

We are aware of a possible issue with React Native apps only, where the user consent prompt is hidden behind the Intercom SDK's UI, so it is not visible for the user to take action. To confirm this is what is happening, you can disable "Require user consent" from [https://cobrowse.io/dashboard/settings](https://cobrowse.io/dashboard/settings) and try to start a new session via Intercom.&#x20;

#### Sorry, you do not have permissions to modify this app installation.

Intercom requires users to have the "Can install, configure and delete apps" permission in order to "Login with Intercom", and therefore also the Cobrowse.io Messenger App.   You may grant your users this permission in Settings -> General -> Teammates -> Edit.

To work around this problem, we have released the Cobrowse.io Inbox App, which lives in the sidebar of Intercom, and enables connecting to the end-user's device(s) with just one click.&#x20;

![Cobrowse.io Inbox App for Intercom](../../.gitbook/assets/intercom-inbox-app-cobrowse-io.png)


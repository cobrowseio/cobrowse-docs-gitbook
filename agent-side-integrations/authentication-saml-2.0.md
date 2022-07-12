# Authentication (SAML 2.0)

## Overview

Allow your users to authenticate using your SAML 2.0 identity provider for simple SSO.

## Configure SAML 2.0

In your account settings, go to dashboard/settings/integrations and under SAML config enter your SAML certificate and entrypoint. This will generate a login URL in the format:

`https://<your hosted domain>/login/saml/<provider ID>`

where `<your hosted domain>` defaults to the domain that is hosting your cobrowse account.

This URL can be used as the login page for your agents.&#x20;

## Configure within your identity provider

You may be required to add configuration for Cobrowse.io within your identity provider.

### Registering Cobrowse.io

If your SAML identity provider requires registering Cobrowse.io as an approved application, then please use the following information:

* service provider id: `cobrowseio-saml`
* ACS / Callback / Recipient / Destination / SSO URL: `https://cobrowse.io/api/1/saml/auth/callback`

### Configuring user roles

All users who login via SAML will by default have the Support Agent role. If you'd like to manage your Admin users through SAML, you must:

* create a group/role named "Cobrowse.io Administrator" within your identity provider
* pass this value through as an attribute in your SAML profile as a value or an array, e.g.:
  * _"groups" : "Cobrowse.io Administrator"_
  * _"groups" : \["Cobrowse.io Administrator", "abc", "def", ...]_

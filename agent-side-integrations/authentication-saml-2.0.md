---
description: >-
  Allow your users to authenticate using your SAML 2.0 identity provider for
  simple SSO.
---

# Authentication (SAML 2.0)

Allow your users to authenticate using your SAML 2.0 identity provider for simple SSO. You must support user email address as the ID type with your identity provider.&#x20;

## Configure SAML 2.0

In your account settings, go to `/dashboard/settings/integrations` and under SAML config enter your:

* SAML certificate (your certificate only, e.g. `MII...` excluding any `<>` tags) and
* entry point URL as configured within your IdP (for a Service Provider-initiated request).

This will generate a login URL in the format `https://<your hosted domain>/login/saml/<provider ID>` where `<your hosted domain>` defaults to the domain that is hosting your cobrowse account, e.g. `cobrowse.io` in most cases. This URL can be used as the login page for your agents.&#x20;

## Configure within your identity provider

You may be required to add configuration for Cobrowse.io within your identity provider.

### Registering Cobrowse.io

If your SAML identity provider requires registering Cobrowse.io as an approved application, then please use the following information:

* service provider id: `cobrowseio-saml`
* ACS / Callback / Recipient / Destination / SSO URL: `https://<your hosted domain>/api/1/saml/auth/callback`

{% hint style="info" %}
Please replace `<your hosted domain>` with `cobrowse.io` when using our hosted service.
{% endhint %}

### Configuring Admin users

All users who login via SAML will, by default, have the Cobrowse.io "Support Agent" role. If you'd like to manage your Admin users through SAML, you must:

* create a group/role named "cobrowseio\_administrator" within your identity provider
* pass this value through as an attribute in your SAML profile as a value or an array, e.g.:
  * `"groups" : "cobrowseio_administrator"`
  * `"groups" : ["cobrowseio_administrator", "abc", "def", ...]`

### Configuring display names

To import the real names of your users from your SAML provider, please configure a `displayName` SAML assertion within your IdP for your users.

## IFrame integrations

If you are running Cobrowse in your own IFrame integration, then you may optionally choose to perform the SSO within the IFrame by loading it from:

`https://<your hosted domain>/api/1/saml/auth?provider=<provider ID>&redirectTo=<your URI encoded Cobrowse route>`

{% hint style="info" %}
`<your hosted domain>` is `cobrowse.io` when using our hosted service.
{% endhint %}

The parameter `<your URI encoded Cobrowse route>` is the endpoint specified by your [choice of IFrame embed](custom-iframe-embeds.md) and must be correctly URI encoded, e.g. for `/code` use `%2Fcode`.

{% hint style="warning" %}
Your IFrame settings and identity provider must allow sharing of cookies to your IFrame'd domain. This includes for additional steps with your provider, such as MFA.&#x20;
{% endhint %}

## Advanced

### Validating the Cobrowse.io SAML request (optional)

Our SAML requests are signed to verify their origin. You can optionally validate this signature within your identity provider. To do this, please use the certificate provided at this endpoint: [https://cobrowse.io/api/1/saml/certificate](https://cobrowse.io/api/1/saml/certificate).

---
description: >-
  Let your identity provider create, update and deactivate Cobrowse account
  members automatically using SCIM.
---

# User provisioning (SCIM)

## Overview

Cobrowse supports user provisioning via SCIM 2.0 (System for Cross-domain Identity Management). Point your identity provider at Cobrowse and it will create account members as people join your team, keep their details up to date and remove access when they leave.

{% hint style="info" %}
SCIM manages **who has an account**. [SAML SSO](https://docs.cobrowse.io/agent-side-integrations/authentication-saml-2.0) manages **how they log in**. They are designed to be used together but each can be configured on its own.
{% endhint %}

## Requirements

You will need:

* an identity provider that supports SCIM 2.0 outbound provisioning
* your [Cobrowse license key](https://cobrowse.io/dashboard/settings)
* a public key configured in [your Cobrowse account](https://cobrowse.io/dashboard/settings/integrations) and the private key to generate the authorization token

## 1. Create a Cobrowse JWT

SCIM uses a [signed Cobrowse JWT](../json-web-tokens-jwts/) bearer token. Your identity provider sends the same JWT with every request so it needs permission to create, read, update and delete account members.

A complete set of claims looks like this:

```json
{
  "sub": "<member>",
  "iss": "<license key>",
  "aud": "https://cobrowse.io",
  "iat": <now>,
  "exp": <expiry>,
  "role": null,
  "policy": {
    "version": 3,
    "members": {
      "permissions": ["create", "read", "update", "delete"]
    }
  }
}
```

The [`policy` claim](../json-web-tokens-jwts/jwt-policies) gives full access to the `members` resource. Set `role` to `null` as provisioning takes its permissions from the policy rather than from a user role.

The claims are signed using **RS256** with a private key that you should keep safe. Save the public key in [your Cobrowse account](https://cobrowse.io/dashboard/settings/integrations) so Cobrowse can verify the token is authentic.

You can use [https://jwt.io/](https://jwt.io/) to sign these claims with your private key to create a JWT.

{% hint style="warning" %}
Provisioning stops when the JWT expires and your identity provider will report this as a loss of permission rather than an expired token. We recommend choosing an `exp` that matches how often you are able to rotate the token.
{% endhint %}

## 2. Point your provider at the SCIM endpoint

Cobrowse exposes a single SCIM base URL which most providers refer to as the **Tenant URL** or **Base URL**:

```
https://cobrowse.io/api/1/scim
```

{% hint style="info" %}
If you use a dedicated or self-hosted instance, then replace `cobrowse.io` with `<your instance domain>`.
{% endhint %}

Some providers ask for more detail than a single base URL. Where they do, use these values:

<table><thead><tr><th width="220">Setting</th><th>Value</th></tr></thead><tbody><tr><td>SCIM version</td><td><code>2.0</code></td></tr><tr><td>Users resource</td><td><code>/Users</code></td></tr><tr><td>Groups resource</td><td>Leave empty. We do not support <code>Groups</code>.</td></tr></tbody></table>

Enter the JWT from step 1 as the bearer token alongside these settings.

## 3. Choose the provisioning actions

Most providers ask which actions your SCIM service supports. Cobrowse supports **creating**, **updating** and **deactivating** users, so enable those and leave anything else disabled.

Where your provider offers a choice, the following also apply:

* **Updates.** We accept both `PATCH` and `PUT` style updates, so either setting will work.
* **Deprovisioning.** Choose to **disable** users rather than delete them. Both revoke access immediately but deleting also removes the SCIM resource so your identity provider has to create them again rather than reactivate them.
* **Groups.** Leave group provisioning off. See [users, not groups](./#users-not-groups) below.

## 4. Map the supported attributes

Cobrowse supports the following user attributes:

<table><thead><tr><th width="170">Attribute</th><th>Description</th></tr></thead><tbody><tr><td><strong>userName</strong></td><td>The username used to log in to Cobrowse. This is usually an email address.</td></tr><tr><td><strong>displayName</strong></td><td>The name of the user shown in the Cobrowse dashboard.</td></tr><tr><td><strong>active</strong></td><td>Whether the user can access Cobrowse.</td></tr><tr><td><strong>externalId</strong></td><td>A unique ID your identity provider uses for reconciliation if provisioning state becomes out of sync.</td></tr></tbody></table>

Only `userName` is required to create a user. Any attributes outside of this list are ignored and can be safely removed.

{% hint style="info" %}
On self-hosted instances, a provisioned user's `userName` must match [`allowed_usernames`](../../enterprise-self-hosting/getting-started/restricting-who-can-sign-in.md) if that is configured, or the provisioning request is rejected.
{% endhint %}

### Matching on userName

Use `userName` as the matching attribute and where your provider asks for a matching precedence, set it to 1. This is the unique key we join on, so your identity provider will adopt existing account members by username instead of creating duplicates.

If your provider lets you write the lookup filter yourself, it must be a single equality term, for example:

```
userName eq "user@example.com"
```

We match on `userName` or `externalId`. Attribute names, the `eq` operator, and the username itself are all treated case insensitively. Filters combining several terms with `and` or `or` are not supported.

{% hint style="warning" %}
`userName` is immutable. Changing the user's username in your identity provider will provision a new account member in Cobrowse.
{% endhint %}

### Users, not groups

We support the SCIM `Users` resource. We do not support `Groups` so turn off group provisioning in your attribute mappings otherwise you will see provisioning errors.

## How Cobrowse handles provisioned users

### Roles

Newly provisioned users are assigned the `agent` role. Roles cannot be managed through SCIM. Roles are assigned through [SAML](https://docs.cobrowse.io/agent-side-integrations/authentication-saml-2.0) or an administrator can change a user's role in the Cobrowse dashboard.

### Deactivation

Setting `active` to `false` removes access immediately with no grace period and no lingering session.

### Usernames must match your SSO identity

If your users log in through [SAML](https://docs.cobrowse.io/agent-side-integrations/authentication-saml-2.0), the SAML attribute Cobrowse uses as the username is `nameID`. The value mapped to `nameID` in the SAML assertion must match the value mapped to `userName` in your provisioning configuration.

{% hint style="danger" %}
If these values do not match, provisioning and login will both appear to work but will create two separate account members for the same user. Please check them against each other before enabling provisioning.
{% endhint %}

## Checking what we support

Our SCIM service describes its own capabilities and some providers read this automatically during setup. You can also request it yourself using the same bearer token as the rest of your configuration:

<table><thead><tr><th width="250">Endpoint</th><th>What it returns</th></tr></thead><tbody><tr><td><code>/ServiceProviderConfig</code></td><td>The features we support, such as filtering and the maximum number of results per page.</td></tr><tr><td><code>/ResourceTypes</code></td><td>The resources we expose.</td></tr><tr><td><code>/Schemas</code></td><td>The attributes available on each resource.</td></tr></tbody></table>

```bash
curl https://cobrowse.io/api/1/scim/ServiceProviderConfig \
  -H "Authorization: Bearer <your JWT>"
```

## Step-by-step guidance

The steps above apply to any SCIM 2.0 provider. For a walkthrough using a specific provider, see:

{% content-ref url="microsoft-entra.md" %}
[microsoft-entra.md](microsoft-entra.md)
{% endcontent-ref %}

{% content-ref url="okta.md" %}
[okta.md](okta.md)
{% endcontent-ref %}

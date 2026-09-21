---
description: >-
  Configure SCIM provisioning for Cobrowse in Microsoft Entra, from creating the
  enterprise application to starting provisioning.
---

# Microsoft Entra

This guide follows the general [SCIM setup](./) using Microsoft Entra. The shared rules on attributes, matching and roles are documented there, so this page describes the Entra configuration only.

{% hint style="info" %}
Application provisioning requires a Microsoft Entra ID P1 or P2 license, or a plan that includes one such as Microsoft 365 E3 or E5. It is not available on the free Microsoft Entra ID tier.
{% endhint %}

## Before you start

[Create a Cobrowse JWT](./#id-1.-create-a-cobrowse-jwt) to use as the bearer token. You will enter it in step 2 below.

## 1. Create the enterprise application

Cobrowse is not listed in the Microsoft Entra application gallery so you will need to create your own **Enterprise Application**:

1. Click **Create your own application**.
2. Give the application a name.
3. Select **Integrate any other application you don't find in the gallery (Non-gallery)**.

## 2. Configure provisioning

Once the application exists, configure it to connect to Cobrowse:

1. Open **Provisioning**, then under **Create configuration** click **Connect your application**.
2. Set **Authentication method** to **Bearer authentication**.
3. Set **Tenant URL** to `https://cobrowse.io/api/1/scim`.
4. Set **Secret token** to the JWT you created.

{% hint style="info" %}
If you use a dedicated or self-hosted instance, then replace `cobrowse.io` with `<your instance domain>`.
{% endhint %}

## 3. Test the connection

Click **Test connection**. Entra queries for a user that does not exist and checks that the response status code and schema are correct. The test will succeed if the token is valid.

## 4. Configure the scoping filters

Scoping filters control both which object types Entra provisions and which users are in scope. Open the **Provisioning** tab and under **Manage** select **Scoping filters**.

### 1. Scope settings

Cobrowse supports the SCIM `Users` resource but not `Groups`. Entra provisions both by default, so if you leave groups enabled Entra will try to create groups in Cobrowse and report provisioning errors.

1. Set **Enable user provisioning** to **Enabled**.
2. Set **Enable group provisioning** to **Disabled**.

### 2. Scope by assignment

Set **Users scope** to **Selected users** to limit provisioning to specific users or groups. We recommend this for most accounts.

Setting it to **All users** provisions everyone in your directory into Cobrowse. Each of them becomes an account member with the `agent` role, so only choose it if every user in your tenant needs Cobrowse access.

### 3. Select users and groups

If you chose **Selected users**, click **Add user/group** and select the users and groups who need Cobrowse access. This is the same assignment list used for single sign-on, so there is no need to assign them separately under **Users and groups** on the application.

{% hint style="warning" %}
Entra provisions the direct members of an assigned group only. Nested groups are not supported, so a user who is a member of a group inside an assigned group will not be provisioned. Assign each group that contains users who need Cobrowse access.
{% endhint %}

### 4. Scope by attribute

Leave the **Attribute scope filter** empty. Cobrowse does not need attribute-based clauses, though you can add them to narrow the list further.

### 5. Review and create

Review the settings to ensure user provisioning is enabled but group provisioning is disabled, then click **Save**.

## 5. Review the attribute mapping

Cobrowse uses four attributes: `userName`, `displayName`, `active` and `externalId`. See [the supported attributes](./#id-4.-map-the-supported-attributes) for what each one does.

Entra's default mappings include many more attributes than these. Cobrowse ignores them and you can remove them under **Provisioning** > **Attribute Mapping** to keep the mappings readable.

## 6. Start provisioning

On the **Provisioning** tab, set **Provisioning Status** to **On** and save to start the first provisioning cycle. Entra reads the users in scope, creates them in Cobrowse and then synchronizes automatically from then on.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

---
description: >-
  Configure SCIM provisioning for Cobrowse in Okta, from creating the app
  integration to enabling provisioning actions.
---

# Okta

This guide follows the general [SCIM setup](./) using Okta. The shared rules on attributes, matching and roles are documented there, so this page describes the Okta configuration only.

{% hint style="info" %}
SCIM provisioning is only available on applications created with the Okta App Integration Wizard. It is not available on OIDC integrations created in the Classic experience.
{% endhint %}

## Before you start

[Create a Cobrowse JWT](./#id-1.-create-a-cobrowse-jwt) to use as the bearer token. You will enter it in step 3 below.

## 1. Create the app integration

Cobrowse is not listed in the Okta Integration Network so you will need to create your own app integration:

1. In the Okta Admin Console, go to **Applications** > **Applications**, then click **Create App Integration**.
2. Select **SAML 2.0** if you also want your agents to log in to Cobrowse through Okta or **SWA** if you only need provisioning.
3. Complete the wizard and give the application a name.

If you select SAML 2.0, configure the SAML side using our [SAML 2.0 guide](https://docs.cobrowse.io/agent-side-integrations/authentication-saml-2.0).

## 2. Enable SCIM provisioning

1. Open the **General** tab and in **App Settings** click **Edit**.
2. Set **Provisioning** to **SCIM**.
3. Click **Save**.

A **Provisioning** tab will now appear on the application.

## 3. Configure the SCIM connection

Open the **Provisioning** tab, then **Integration**, and click **Edit**:

1. Set **SCIM connector base URL** to `https://cobrowse.io/api/1/scim`.
2. Set **Unique identifier field for users** to `userName`.
3. Under **Supported provisioning actions**, enable **Push New Users** and **Push Profile Updates**.
4. Set **Authentication Mode** to **HTTP Header**.
5. In the **Authorization field**, enter the JWT you created. Okta adds the Bearer prefix for you.

{% hint style="info" %}
If you use a dedicated or self-hosted instance, then replace `cobrowse.io` with `<your instance domain>`.
{% endhint %}

{% hint style="warning" %}
Leave **Push Groups** disabled. We support the SCIM `Users` resource but not `Groups` and enabling it will cause provisioning errors.
{% endhint %}

Click **Test Connector Configuration**. Okta checks that it can reach the base URL and authenticate with the token and reports which provisioning actions the connection supports. If the test succeeds, click **Save**.

## 4. Enable the provisioning actions

Still on the **Provisioning** tab, open **To App** and click **Edit**. Then enable:

* **Create Users**
* **Update User Attributes**
* **Deactivate Users**

Deactivating a user in Okta sets `active` to `false` in Cobrowse which removes their access immediately.

## 5. Review the attribute mappings

Cobrowse uses four attributes: `userName`, `displayName`, `active`, and `externalId`. See [the supported attributes](./#id-4.-map-the-supported-attributes) for what each one does.

Okta's default SCIM profile also maps attributes such as `name.givenName`, `name.familyName`, `emails` and `locale`. Cobrowse ignores these and you can remove them under **Provisioning** > **To App** > **Attribute Mappings** to keep the mappings readable.

Check that `displayName` is mapped. Okta does not always populate it by default and without it your agents will appear in the Cobrowse dashboard without a name.

## 6. Assign users

Open the **Assignments** tab and assign the people or groups who need Cobrowse access. Okta provisions an account member for each assigned user and deactivates them in Cobrowse when the assignment is removed.

Assigning a group provisions each of its members as an individual account member. This is different from **Push Groups** in step 3, which creates the group itself in Cobrowse and is not supported.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

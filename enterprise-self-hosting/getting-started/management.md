---
description: >-
  Manage your Cobrowse deployment effectively by regularly backing up the
  database, staying updated on new releases, and more.
---

# Managing your deployment

### Managing the database

You are responsible for backing up the database regularly. Cobrowse will not do this automatically in any way. See the [MongoDB docs](https://docs.mongodb.com/manual/core/backups/) for recommendations on backup strategies. [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) can be configured to do this automatically.

### New releases

To be notified when a new release is available (for any deployment type), please subscribe to this repo: [https://github.com/cobrowseio/cobrowse-enterprise-helm](https://github.com/cobrowseio/cobrowse-enterprise-helm).

Changelog notes for all releases are available: [https://github.com/cobrowseio/cobrowse-enterprise-helm/releases](https://github.com/cobrowseio/cobrowse-enterprise-helm/releases). Releases follow [Semantic Versioning](https://semver.org). Details on breaking changes are included in the release notes for each major version. Security fixes are usually released as patch versions, which are backward compatible.

#### Testing your deployment

A sanity test package is available as a container as part of our Enterprise software. It can be used to check a deployment was succesful. It can be used with Docker in the following way:

```
docker run -e COBROWSE_E2E_SUPERUSER_TOKEN=<super_user_token>                       \
           -e COBROWSE_E2E_BASE_URL=https://<domain> -e CI=true                     \
           -e COBROWSE_E2E_CLIENT_SDK_URL=https://<domain>/sdk-js/CobrowseIO.mjs    \
           -v $(pwd)/playwright-report:/app/playwright-report                       \
           cobrowse-integration-tests-enterprise
```

### Upgrading Cobrowse

**AWS, Azure, GCP and Docker-compose**

Our command line utility allows you to upgrade your deployment, you can use it like this:

```
npx cobrowse-enterprise upgrade <directory>
```

One you have done that, you can update your deployment via Terraform or Docker Compose as in the initial deployment.

_Note: We recommend making a database backup before upgrading to new versions_

**Helm**

To upgrade your Helm deployment use:

```
helm repo update 
helm upgrade cobrowse cobrowse-enterprise/cobrowse-enterprise
```

### Rotating secrets

Some credentials on your instance are owned by you and should be rotated on your own schedule, or immediately if you believe they have been exposed.

**Account JWT keys**

Each account verifies the JWTs your backend signs against the public keys listed under **Public keys** in `/dashboard/settings/integrations` (see [Authentication (JWTs)](../../agent-side-integrations/json-web-tokens-jwts/)). An account can hold several keys at once, so you can rotate without downtime:

1. Generate a new RS256 key pair.
2. Add the new public key to the account.
3. Switch your backend to sign with the new private key.
4. Delete the old public key from the account.

JWTs signed with the old key stop being accepted as soon as it is deleted. Repeat for every account on the instance that uses JWT authentication.

**SSO integrations**

SAML configuration is also per account, under **SAML config** in `/dashboard/settings/integrations` (see [Authentication (SAML 2.0)](../../agent-side-integrations/authentication-saml-2.0.md)). When your identity provider rotates its signing certificate, update the **Certificate** field with the new one. If you have configured a `saml:private_key` for the instance to sign its SAML requests, rotating it also requires updating the certificate registered with your identity provider.

**Instance service account**

The `service_account:public_key` configuration key is the public half of the instance level service account. It verifies JWTs issued with `iss=service`, which is the only issuer allowed to grant superuser tokens. Treat its private key as the most sensitive credential on the instance. To rotate it:

1. Generate a new RS256 key pair.
2. Replace the value of `service_account:public_key` in the instance configuration. Superusers can do this at `/admin/configuration`, or you can set the `service_account__public_key` environment variable on the **api** service.
3. Restart the **api** pods or containers. Configuration from the database is only read at startup.
4. Move any tooling that signs service tokens over to the new private key.

{% hint style="info" %}
Database, Redis, SMTP and cloud provider credentials are managed by your infrastructure, not by Cobrowse. Rotate them through your deployment configuration and roll the affected services as you would for any other configuration change.
{% endhint %}

### Dedicated staging instance

We recommend self-hosting a separate staging instance of our software to develop against and also to test new releases before moving to production. Please contact us if you are interested.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

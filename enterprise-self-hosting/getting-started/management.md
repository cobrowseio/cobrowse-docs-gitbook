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

Changelog notes for all releases are available: [https://github.com/cobrowseio/cobrowse-enterprise-helm/releases](https://github.com/cobrowseio/cobrowse-enterprise-helm/releases). Image updates usually contain security patches.

#### Testing your deployment

A sanity test package is available as a container as part of our Enterprise software. It can be used to check a deployment was succesful. It can be used with Docker in the following way:\


```
docker run -e COBROWSE_E2E_SUPERUSER_TOKEN=<super_user_token>                       \ 
           -e COBROWSE_E2E_BASE_URL=https://example.com -e CI=true                  \                       \       
           -e COBROWSE_E2E_CLIENT_SDK_URL=https://<deployment>/sdk-js/CobrowseIO.js \
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

### Dedicated staging instance

We recommend self-hosting a separate staging instance of our software to develop against and also to test new releases before moving to production. Please contact us if you are interested.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

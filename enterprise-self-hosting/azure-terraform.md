# Azure terraform

This is the documentation for how to use our terraform for Azure to set up Cobrowse Enterprise in your cloud.

## Initial Setup

You will need the following installed locally:

* [**Terraform**](https://www.terraform.io/)
* [**NodeJS**](https://nodejs.org/en/) – version 10 or above
* **Azure CLI**
* **kubectl**

## Installation steps

These are the steps you'll need to go through to get Cobrowse running:

### 1. Database setup

The first thing you'll need is access to a MongoDB database. Cobrowse will need a connection string containing the address and authentication information for your cluster.

A MongoDB cluster is required for running Cobrowse. We **do not** provide this as part of the terraform environment.

You will need to create a cluster and provide the connection URL as a part of the Cobrowse configuration. You can either [run your own MongoDB cluster](https://docs.mongodb.com/manual/administration/install-community/) and manage the deployment and backups yourself. Alternatively, we recommend using a hosted service such as [MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/). They have a [range of certifications](https://www.mongodb.com/cloud/trust) required by many enterprises with compliance requirements.

### 2. Create initial Azure resources

There are some resources that are not created by our Terraform. You will need to manually create:

1. A resource group for Cobrowse. You can do with via the cli or in the Azure portal: [https://portal.azure.com/\#blade/HubsExtension/BrowseResourceGroups](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups)
2. A key vault for Cobrowse secrets. You can do this via the cli or in the Azure portal: [https://portal.azure.com/\#blade/HubsExtension/BrowseResource/resourceType/Microsoft.KeyVault%2Fvaults](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.KeyVault%2Fvaults)
3. Add a secret in the key vault called "mongo-url" with the value set to the url of the Mongo database from step 1.

### 3. Generate the config directory

We have provided a small command line utility to help you get started. This utility will gather the required config for your deployment. Run the following command from your terminal:

```bash
> npx cobrowse-enterprise create azure ./example
```

You can replace "./example" with the directory where you wish to save the configuration data. The directory will be created if it does not exist yet.

### 4. Deploy the Terraform

{% hint style="warning" %}
_**Warning:**_ This will store all Terraform state locally. We **strongly** recommend [adding a backend](https://www.terraform.io/docs/language/settings/backends/azurerm.html) configuration for almost all deployment scenarios.
{% endhint %}

Once you have successfully generated a configuration directory via our command line utility you are then ready to deploy the terraform to Azure. Navigate to the configuration directory you created and run the following commands:

```bash
> terraform init
```

This will instruct terraform to prepare the resources it needs to deploy. Run the following command to start the deployment of resources to Azure:

```bash
> terraform apply
```

This will list the modifications that terraform will make to your Azure account. If that looks good, type 'yes' to continue the deployment.

### 5. Configure your DNS provider

Configure your DNS provider with an A record to point to the IP address provisioned by the terraform. We recommend doing this as soon as the IP is provisioned in the Azure portal to prevent DNS propagation delays, although this is not essential.

Once the terraform is deployed and the DNS configured you may have to wait for the certificate to be provisioned. Depending on DNS propagation delays this can take some time \(usually between 5 minutes and an hour\).

### 6. Check your deployment

Your Cobrowse instance should now be deployed. Head to `/register`on your domain to create an account.

{% page-ref page="getting-started/" %}

## Managing your deployment

Next, learn about managing and upgrading your deployment.

{% page-ref page="getting-started/management.md" %}


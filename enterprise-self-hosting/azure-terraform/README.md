# Azure terraform

This is the documentation for how to use our Terraform for Azure to set up Cobrowse Enterprise in your cloud.

## Initial Setup

You will need the following installed locally:

* [**Terraform**](https://www.terraform.io/)
* [**NodeJS**](https://nodejs.org/en/) – at least [minimum LTS version](https://nodejs.org/en/about/releases/)
* **Azure CLI**
* **kubectl**

## Installation steps

These are the steps you'll need to go through to get Cobrowse running:

### 1. Database setup

The first thing you'll need is access to a MongoDB database. Cobrowse will need a connection string containing the address and authentication information for your cluster.

A MongoDB cluster is required for running Cobrowse. We **do not** provide this as part of the terraform environment.

You will need to create a cluster and provide the connection URL as a part of the Cobrowse configuration. You can either [run your own MongoDB cluster](https://docs.mongodb.com/manual/administration/install-community/) and manage the deployment and backups yourself. Alternatively, we recommend using a hosted service such as [MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/). They have a [range of certifications](https://www.mongodb.com/cloud/trust) required by many enterprises with compliance requirements.

Due to compatibility issues we **do not** support [Azure Cosmos DB](https://azure.microsoft.com/en-gb/products/cosmos-db/).

### 2. Set up the Azure CLI

Terraform requires the Azure CLI to be installed and authenticated.

1. [Install](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-macos) the Azure CLI
2. [Authenticate](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli) the Azure CLI (hint: `az login`)

### 3. Create initial Azure resources

There are some resources that are not created by our Terraform. You will need to manually create:

1. A storage bucket to save terraform state (this is optional but **strongly** recommended)
2. A resource group for Cobrowse. You create it via the cli or in the Azure portal:[ https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups)
3. A key vault for Cobrowse secrets. You can do this via the cli or in the Azure portal: [https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.KeyVault%2Fvaults](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.KeyVault%2Fvaults)
4. Add a secret in the key vault called "mongo-url" with the value set to the url of the Mongo database from step 1.
5. Make sure you have [initialized the Azure CLI](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli) with your access credentials.

{% hint style="info" %}
**Azure CLI Examples**

```bash
# 1. Create a resource group (replace <your location> with your Azure region)
> az group create --location <your location> --name CobrowseEnterprise

# 2. Create a key vault
> az keyvault create --resource-group CobrowseEnterprise --name CobrowseEnterpriseKV

# 3. Add a secret in the key vault called "mongo-url"
> az keyvault secret set --vault-name CobrowseEnterpriseKV --name mongo-url --file ./mongo-url.txt
```
{% endhint %}

### 4. Generate the config directory

We have provided a small command line utility to help you get started. This utility will gather the required config for your deployment. Run the following command from your terminal:

```bash
> npx cobrowse-enterprise create azure ./example
```

You can replace "./example" with the directory where you wish to save the configuration data. The directory will be created if it does not exist yet.

### 5. Deploy the Terraform

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

{% hint style="info" %}
**Configure kubectl**

`kubectl` is an essential utility for navigating and inspecting the kubernetes cluster deployed with the terraform scripts. Once the terraform finishes applying, we recommend installing and configuring `kubectl` to communicate with the kubernetes cluster.

If you have not installed `kubectl` you can do so by running:

```bash
> az aks install-cli
```

Once installed, you can configure it by running:

```bash
> az aks get-credentials --resource-group CobrowseEnterprise --name cobrowse-enterprise
```

When complete, test that it works by running:

```bash
> kubectl get pod
```
{% endhint %}

### 6. Configure your DNS provider and SSL certificate

Configure your DNS provider with an A record to point to the IP address provisioned by the terraform. We recommend doing this as soon as the IP is provisioned in the Azure portal to prevent DNS propagation delays, although this is not essential

Once the terraform is deployed and the DNS is configured you will have to wait for the certificate to be provisioned. Depending on DNS propagation delays this can take some time (usually between 5 minutes and an hour).

{% hint style="info" %}
**How is SSL Configured?**

The terraform automatically creates an SSL certificate for you using the [Let's Encrypt HTTP-01 challenge](https://letsencrypt.org/docs/challenge-types/#http-01-challenge).

In practice, this means that upon initial deployment, a pod resource is polling your configured public service domain at the HTTP-01 path. Once the DNS provider configuration has finished propagating, the HTTP-01 challenge check will succeed and your SSL certificate will be issued.

Only once the SSL certificate is issued will an HTTPS listener be created for your deployment. Once complete, the Let's Encrypt resources used to verify your DNS name will be destroyed.&#x20;
{% endhint %}

### 7. Check your deployment

Your Cobrowse instance should now be deployed. Head to `/register`on your domain to create an account.

{% content-ref url="../getting-started/" %}
[getting-started](../getting-started/)
{% endcontent-ref %}

## Managing your deployment

Next, learn about managing and upgrading your deployment.

{% content-ref url="../getting-started/management.md" %}
[management.md](../getting-started/management.md)
{% endcontent-ref %}

# GCP terraform

This is the documentation for how to use our terraform for GCP to set up Cobrowse Enterprise in your cloud.

## Initial Setup

You will need the following installed locally:

* [**Terraform**](https://www.terraform.io/)
* [**NodeJS**](https://nodejs.org/en/) – at least [minimum LTS version](https://nodejs.org/en/about/releases/)
* **Google Cloud SDK (gcloud CLI)**
* **kubectl**

## Installation steps

These are the steps you'll need to go through to get Cobrowse running:

### 1. Database setup

The first thing you'll need is access to a MongoDB database. Cobrowse will need a connection string containing the address and authentication information for your cluster.

A MongoDB cluster is required for running Cobrowse. We **do not** provide this as part of the terraform environment.

You will need to create a cluster and provide the connection URL as a part of the Cobrowse configuration. You can either [run your own MongoDB cluster](https://docs.mongodb.com/manual/administration/install-community/) and manage the deployment and backups yourself. Alternatively, we recommend using a hosted service such as [MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/). They have a [range of certifications](https://www.mongodb.com/cloud/trust) required by many enterprises with compliance requirements.

### 2. Setup the Google Cloud SDK

Terraform requires that the Google Cloud SDK be installed and authenticated:

1. [Install](https://cloud.google.com/sdk/docs/install) the Google Cloud SDK
2. [Initialize](https://cloud.google.com/sdk/docs/initializing) the Google Cloud SDK (hint: `gcloud init`)
3. [Authenticate with application default credentials](https://cloud.google.com/sdk/gcloud/reference/auth/application-default/login) using the gcloud CLI (hint: `gcloud auth application-default login`)

### 3. Create initial GCP resources

There are some resources that are not created by our Terraform. You will need to manually create:

1. A secret in the [Secret Manager](https://console.cloud.google.com/security/secret-manager). Create a secret called "mongo\_url" with the value set to the url of the Mongo database from step 1.&#x20;
2. A storage bucket to save terraform state (this is optional but **strongly** recommended)

{% hint style="info" %}
**gcloud CLI examples**

```bash
# 1. Create a secret called "mongo_url"
> gcloud secrets create mongo_url --data-file=mongo-url.txt
```
{% endhint %}

### 4. Generate the config directory

We have provided a small command line utility to help you get started. This utility will gather the required config for your deployment. Run the following command from your terminal:

```bash
> npx cobrowse-enterprise create gcp ./example
```

You can replace "./example" with the directory where you wish to save the configuration data. The directory will be created if it does not exist yet.

### 5. Deploy the Terraform

{% hint style="warning" %}
_**Warning:**_ This will store all Terraform state locally. We **strongly** recommend [adding a backend](https://www.terraform.io/docs/language/settings/backends/azurerm.html) configuration for almost all deployment scenarios.
{% endhint %}

Once you have successfully generated a configuration directory via our command line utility you are then ready to deploy the terraform to GCP. Navigate to the configuration directory you created and run the following commands:

```bash
> terraform init
```

This will instruct terraform to prepare the resources it needs to deploy. Run the following command to start the deployment of resources to GCP:

```bash
> terraform apply
```

This will list the modifications that terraform will make to your GCP account. If that looks good, type 'yes' to continue the deployment.

{% hint style="info" %}
**Configure kubectl**

`kubectl` is an essential utility for navigating and inspecting the kubernetes cluster deployed with the terraform scripts. Once the terraform finishes applying, we recommend installing and configuring `kubectl` to communicate with the kubernetes cluster.

If `kubectl` is not installed already, you can install it by running:

```bash
> gcloud components install kubectl
```

Once installed, you can configure it with your cluster by running:

```bash
> gcloud container clusters get-credentials cluster-enterprise
```

When complete, test that it works by running:

```bash
> kubectl get pod
```
{% endhint %}

### 6. Configure your DNS provider and SSL certificate

Configure your DNS provider with an A record to point to the IP address provisioned by the terraform. We recommend doing this as soon as the IP is provisioned in the GCP cloud console to prevent DNS propagation delays, although this is not essential.

Once the terraform is deployed and the DNS configured you may have to wait for the certificate to be provisioned. Depending on DNS propagation delays this can take some time (usually between 5 minutes and an hour).

{% hint style="info" %}
**How is SSL configured?**

The terraform creates a GKE ManagedCertificate resource that will provision your SSL certificate for the domain you previously configured for your cobrowse enterprise deployment.

In practice, this means that once the ManagedCertificate process detects that your DNS name is configured to point to your cluster IP address, the SSL certificate will be issued you will be able to access your cobrowse deployment at `https://<your domain>`.
{% endhint %}

### 7. Check your deployment

Your Cobrowse instance should now be deployed. Head to `/register`on your domain to create an account.&#x20;

{% content-ref url="getting-started/" %}
[getting-started](getting-started/)
{% endcontent-ref %}

## Managing your deployment

Next, learn about managing and upgrading your deployment.

{% content-ref url="getting-started/management.md" %}
[management.md](getting-started/management.md)
{% endcontent-ref %}

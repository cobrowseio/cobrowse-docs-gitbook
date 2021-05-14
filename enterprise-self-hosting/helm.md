# Helm chart

This is the documentation for how to use Helm to set up Cobrowse Enterprise in your Kubernetes cluster.

{% hint style="warning" %}
Helm deployment is currently in **alpha**. There may be breaking changes without warning.
{% endhint %}

## Initial Setup

Make sure you have installed \(on your local machine\) the required tools to manage and deploy the Cobrowse infrastructure to your Kuberntes cluster. You'll need these installed before running the setup:

* **Helm**

### Installation

Run the following commands to add our helm repository.

```bash
> helm add repo cobrowse-enterprise https://cobrowseio.github.io/cobrowse-enterprise-helm/packages
> helm install cobrowse-enterprise
```

### Common Parameters

| Parameter Name | Description |
| :--- | :--- |
| `imageCredentials.password` | The docker password required to access the Cobrowse images \(provided by Cobrowse.io\) |
| `license` | Your Cobrowse enterprise license \(provided by Cobrowse.io\) |
| `domain` | The domain name for your deployment |
| `superusers` | RegEx to specify superuser email addresses |
| `mongo.url` | Your MongoDB connection URL |
| `redis.url` | Your Redis cluster connection URL  |
| `ingress.class` | The ingress class name to use |
| `ingress.annotations` | Extra annoations to add to the Kubernetes ingress. |
| `storage` | Amount of NFS storage to provision for recordings. We require a storage class called "nfs" to exist in the cluster that supports "ReadWriteMany" access. Default is 50Gb. |

### Dependencies

For all deployments there are some dependencies that must be configured outside of the Cobrowse Helm chart:

1. **Redis** - we require access to a redis cluster. It must be running in cluster mode. Bitnami provide an easy to use [Helm chart](https://github.com/bitnami/charts/tree/master/bitnami/redis-cluster).
2. **MongoDB** - we require access to a MongoDB cluster. We recommend using a hosted service such as [MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/) whenever possible. Alternatively MongoDB provide a [helm chart](https://www.mongodb.com/blog/post/introducing-the-mongodb-enterprise-operator-for-kubernetes) for deploying to your own insfrastrcture.
3. **NFS storage class** - we require that the cluster provides an NFS storage provisioner for the storage class called "nfs".

There's also some extra configuration available for some cloud providers.

{% tabs %}
{% tab title="Azure / AKS" %}
#### **SSL Generation**

We support SSL certificate generation in Azure via CertManager. You must install [CertManager](https://cert-manager.io/docs/installation/kubernetes/) in addition to the Cobrowse Helm chart. You can then set `ssl.generator` to `cert-manager` in the Cobrowse Helm chart configuration to allow Cobrowse generate and renew and SSL certs automatically.

**Other Common Dependencies for AKS**

To use Azure Application Gateway, you will need to enable [AGIC](https://docs.microsoft.com/en-us/azure/application-gateway/ingress-controller-overview) and [AAD pod-identity](https://docs.microsoft.com/en-us/azure/aks/use-azure-ad-pod-identity) either via Helm chart or AKS addon.
{% endtab %}

{% tab title="GCP / GKE" %}
**SSL Generation**

If you use the GCE ingress \(set `ingress.class` to `gce`\), we will automatically provision a certificate using a GCP ManagedCertificate. You do not need install anything extra to use this.  
{% endtab %}
{% endtabs %}

## Managing your deployment

Next, learn about managing and upgrading your deployment.

{% page-ref page="getting-started/management.md" %}


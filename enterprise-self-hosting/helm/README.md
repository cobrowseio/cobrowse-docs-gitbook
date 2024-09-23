---
description: >-
  Learn how to use Helm to set up Cobrowse Enterprise in your Kubernetes
  cluster.
---

# Helm chart

This is the documentation for how to use Helm to set up Cobrowse Enterprise in your Kubernetes cluster.

## Initial Setup

Make sure you have installed (on your local machine) the required tools to manage and deploy the Cobrowse infrastructure to your Kubernetes cluster. You'll need these installed before running the setup:

* **Helm**

### Installation

Run the following commands to add our helm repository.

```bash
helm repo add cobrowse-enterprise https://cobrowseio.github.io/cobrowse-enterprise-helm/packages
helm install cobrowse cobrowse-enterprise/cobrowse-enterprise
```

### Common Parameters

| Parameter Name                                                                | Description                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>imageCredentials.password</code></p><p><strong>required</strong></p> | The docker password required to access the Cobrowse images (provided by Cobrowse.io). **Note:** If you do not wish for the Helm chart to manage your cobrowse image pull secret, consider using the [advanced image pull secret configuration instructions](broken-reference) instead.                                                                                       |
| <p><code>license</code></p><p><strong>required</strong></p>                   | Your Cobrowse enterprise license. (Provided by Cobrowse.io)                                                                                                                                                                                                                                                                                                                  |
| <p><code>domain</code></p><p><strong>required</strong></p>                    | The domain name for your deployment.                                                                                                                                                                                                                                                                                                                                         |
| `superusers`                                                                  | RegEx to specify superuser email addresses.                                                                                                                                                                                                                                                                                                                                  |
| <p><code>mongo.url</code></p><p><strong>required</strong></p>                 | Your MongoDB connection URL.                                                                                                                                                                                                                                                                                                                                                 |
| <p><code>redis.url</code></p><p><strong>required</strong></p>                 | <p>Your Redis cluster connection URL in the format:</p><p><code>redis://username:password@your-redis-host.com:PORT</code></p><p>Or, if you are using TLS:</p><p><code>rediss://username:password@your-redis-host.com:PORT</code></p><p>(note the double <code>s</code> in <code>rediss://</code>)</p><p>Note: redis <strong>must</strong> be configured in cluster mode.</p> |
| <p><code>ingress.class</code></p><p><strong>required</strong></p>             | The ingress class name to use                                                                                                                                                                                                                                                                                                                                                |
| `ingress.annotations`                                                         | Extra annotations to add to the Kubernetes ingress.                                                                                                                                                                                                                                                                                                                          |
| `storage.size`                                                                | Amount of storage to provision for recordings. Default is 50Gb.                                                                                                                                                                                                                                                                                                              |
| `storage.class`                                                               | A storage class available in the cluster that supports "ReadWriteMany" access. Default is "nfs".                                                                                                                                                                                                                                                                             |

### Dependencies

For all deployments there are some dependencies that must be configured outside of the Cobrowse Helm chart:

1. **Redis** - we require access to a redis cluster. It must be running in cluster mode. Bitnami provide an easy to use [Helm chart](https://github.com/bitnami/charts/tree/master/bitnami/redis-cluster).
2. **MongoDB** - we require access to a MongoDB cluster. We recommend using a hosted service such as [MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/) whenever possible. Alternatively Bitnami provides a [helm chart](https://bitnami.com/stack/mongodb/helm) that is tested with Cobrowse deployments.
3. **NFS storage** - we require that the cluster provides an NFS storage. A dynamic provisioner can be used by by setting `storage.class`, that is by default called "nfs". Alternatively an existing PVC can be used and set through `sockets.storage.persistentVolumeClaimName` and `recording.storage.persistentVolumeClaimName`.

There's also some extra configuration available for some cloud providers.

{% tabs %}
{% tab title="Azure / AKS" %}
**SSL Generation**

We support SSL certificate generation in Azure via CertManager. You must install [CertManager](https://cert-manager.io/docs/installation/kubernetes/) in addition to the Cobrowse Helm chart. You can then set `ssl.generator` to `cert-manager` in the Cobrowse Helm chart configuration to allow Cobrowse generate and renew and SSL certs automatically.

**Other Common Dependencies for AKS**

To use Azure Application Gateway, you will need to enable [AGIC](https://docs.microsoft.com/en-us/azure/application-gateway/ingress-controller-overview) and [AAD pod-identity](https://docs.microsoft.com/en-us/azure/aks/use-azure-ad-pod-identity) either via Helm chart or AKS addon.
{% endtab %}

{% tab title="GCP / GKE" %}
**SSL Generation**

If you use the GCE ingress (set `ingress.class` to `gce`), we will automatically provision a certificate using a GCP ManagedCertificate. You do not need install anything extra to use this.
{% endtab %}
{% endtabs %}

## Scaling the infrastructure

By default we only run one replica per service as can be seen in our public [values.yaml](https://github.com/cobrowseio/cobrowse-enterprise-helm/blob/master/values.yaml) file. This should be changed for high availability (HA) setups.

The individual service replica counts should be set according to your load and availability requirements, for example `api.replicas = 3`sets 3 replicas of the api service.

## Managing your deployment

Next, learn about managing and upgrading your deployment.

{% content-ref url="../getting-started/management.md" %}
[management.md](../getting-started/management.md)
{% endcontent-ref %}

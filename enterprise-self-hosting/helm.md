# Helm chart

This is the documentation for how to use Helm to set up Cobrowse Enterprise in your Kubernetes cluster.

## Initial Setup

Make sure you have installed (on your local machine) the required tools to manage and deploy the Cobrowse infrastructure to your Kubernetes cluster. You'll need these installed before running the setup:

* **Helm**

### Installation

Run the following commands to add our helm repository.

```bash
> helm repo add cobrowse-enterprise https://cobrowseio.github.io/cobrowse-enterprise-helm/packages
> helm install cobrowse cobrowse-enterprise/cobrowse-enterprise
```

### Common Parameters

| Parameter Name                                                                | Description                                                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>imageCredentials.password</code></p><p><strong>required</strong></p> | The docker password required to access the Cobrowse images (provided by Cobrowse.io)                                                                                                                                                                                                                                                                                                                      |
| <p><code>license</code></p><p><strong>required</strong></p>                   | Your Cobrowse enterprise license (provided by Cobrowse.io)                                                                                                                                                                                                                                                                                                                                                |
| <p><code>domain</code></p><p><strong>required</strong></p>                    | The domain name for your deployment                                                                                                                                                                                                                                                                                                                                                                       |
| `superusers`                                                                  | RegEx to specify superuser email addresses                                                                                                                                                                                                                                                                                                                                                                |
| <p><code>mongo.url</code></p><p><strong>required</strong></p>                 | Your MongoDB connection URL                                                                                                                                                                                                                                                                                                                                                                               |
| <p><code>redis.url</code></p><p><strong>required</strong></p>                 | <p>Your Redis cluster connection URL in the format:</p><p> </p><p><code>redis://username:password@your-redis-host.com:PORT</code></p><p></p><p>Or, if you are using TLS:</p><p></p><p><code>rediss://username:password@your-redis-host.com:PORT</code></p><p>(note the double <code>s</code> in <code>rediss://</code>)</p><p></p><p>Note: redis <strong>must</strong> be configured in cluster mode.</p> |
| <p><code>ingress.class</code></p><p><strong>required</strong></p>             | The ingress class name to use                                                                                                                                                                                                                                                                                                                                                                             |
| `ingress.annotations`                                                         | Extra annotations to add to the Kubernetes ingress.                                                                                                                                                                                                                                                                                                                                                       |
| `storage.size`                                                                | Amount of storage to provision for recordings. Default is 50Gb.                                                                                                                                                                                                                                                                                                                                           |
| `storage.class`                                                               | A storage class available in the cluster that supports "ReadWriteMany" access. Default is "nfs".                                                                                                                                                                                                                                                                                                          |

### Dependencies

For all deployments there are some dependencies that must be configured outside of the Cobrowse Helm chart:

1. **Redis** - we require access to a redis cluster. It must be running in cluster mode. Bitnami provide an easy to use [Helm chart](https://github.com/bitnami/charts/tree/master/bitnami/redis-cluster).
2. **MongoDB** - we require access to a MongoDB cluster. We recommend using a hosted service such as [MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/) whenever possible. Alternatively MongoDB provide a [helm chart](https://www.mongodb.com/blog/post/introducing-the-mongodb-enterprise-operator-for-kubernetes) for deploying to your own infrastructure.
3. **NFS storage class** - we require that the cluster provides an NFS storage provisioner for the storage class that is by default called "nfs". You can configure the name of the storage class by setting `storage.class`.&#x20;

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

If you use the GCE ingress (set `ingress.class` to `gce`), we will automatically provision a certificate using a GCP ManagedCertificate. You do not need install anything extra to use this. &#x20;
{% endtab %}
{% endtabs %}

## Advanced Configuration

Many of the cobrowse service component configurations are managed using environment variables specified in `ConfigMap` and `Secret` resources, and these configurations can be overridden outside of the Helm chart by resources that you manage.

### ConfigMap Resources

To see a list of all `ConfigMap` resources managed by the Helm chart, you can run:

```bash
> kubectl get configmap -l "app=cobrowse-cobrowse-enterprise"
NAME                          DATA   AGE
cobrowse-api-envvars          10     25m
cobrowse-frontend-envvars     1      25m
cobrowse-proxy-envvars        4      25m
cobrowse-recording-envvars    3      25m
cobrowse-sockets-envvars      6      25m
```

Each of these **envvars** `ConfigMap`s can be overridden by creating `ConfigMap` resources named (respectively):

```
cobrowse-api-custom-envvars
cobrowse-frontend-custom-envvars
cobrowse-proxy-custom-envvars
cobrowse-recording-custom-envvars
cobrowse-sockets-custom-envvars
```

To see what environment variables are available to be configured for a particular service component, you can run (for example):

```bash
> kubectl describe configmap cobrowse-api-envvars
```

### Secret Resources

To see a list of all `Secret` resources managed by the Helm chart, you can run:

```bash
> kubectl get secret -l "app=cobrowse-cobrowse-enterprise"
NAME                         TYPE     DATA   AGE
cobrowse-api-envvars         Opaque   2      41m
cobrowse-frontend-envvars    Opaque   0      41m
cobrowse-proxy-envvars       Opaque   0      41m
cobrowse-recording-envvars   Opaque   0      41m
cobrowse-sockets-envvars     Opaque   2      41m
```

Each of these **envvars** `Secret`s can be overridden by creating `Secret` resources named (respectively):

```
cobrowse-api-custom-envvars
cobrowse-frontend-venvvars
cobrowse-proxy-custom-envvars
cobrowse-recording-custom-envvars
cobrowse-sockets-custom-envvars
```

{% hint style="info" %}
**Order of Priority**

The following order of priority is taken, when resolving environment variables using the **api** service component as an example:

1. **Secret:** cobrowse-api-custom-envvars
2. **Secret:** cobrowse-api-envvars
3. **ConfigMap:** cobrowse-api-custom-envvars
4. **ConfigMap**: cobrowse-api-envvars

Thus if you override an environment variable such as `redis_url` in the `cobrowse-api-custom-envvars` **ConfigMap**, then the value will be overridden by the Helm-managed `cobrowse-api-envvars` **Secret**. Thus, make sure before overriding an environment variable in the **ConfigMap** that it isn't set in one of the **Secret** resources first.

****
{% endhint %}

## Managing your deployment

Next, learn about managing and upgrading your deployment.

{% content-ref url="getting-started/management.md" %}
[management.md](getting-started/management.md)
{% endcontent-ref %}

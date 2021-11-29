# OpenMetrics Collection

Cobrowse Enterprise services make Node.js, Redis and custom application metrics available in the text-based OpenMetrics/Prometheus exposition format. OpenMetrics/Prometheus exposition format has become a widely adopted standard and provides the most flexibility in how you can consume and use your application metrics.

The right service to consume it is largely dependent on your organization and the existing tools and expertise you have at your disposal. Because of this, while cobrowse offers a variety of example configurations and guidance in this article for the most popular use-cases, it is your responsibility to provision and configure the resources that make the most sense for your organization and needs.

{% hint style="warning" %}
If you use a hosted service to collect application metrics, keep in mind that collecting more metrics into the service **almost always increases costs.**&#x20;

Without additional filters, collecting all application metrics into such a service can add **hundreds of new metrics**, depending on how the service handles different combinations of OpenMetrics labels and values.

Most service configurations for collecting metrics will offer an option to filter the metrics being collected. We recommend using these configurations if the volume of metrics being offered are cost prohibitive for your service.
{% endhint %}

{% hint style="info" %}
Application metrics for each depoyment are exposed through an HTTP endpoint in each pod on port `8080` and on path `/metrics`.

You can verify that metrics are being exposed by accessing a terminal session to a pod and requesting `http://localhost:8080/metrics` :

```bash
> kubectl exec -it <pod name> -- /bin/bash
> bash-5.1$ wget -O- http://localhost:8080/metrics
```

If metrics are successfully exposed, you should receive a text document in your terminal listing a variety of labelled process, nodejs and cobrowse metric values.&#x20;
{% endhint %}

### Google Cloud Workload Metrics

If you are running the Cobrowse Enterprise Google Cloud terraform configuration, Google Cloud's [Workload Metrics](https://cloud.google.com/stackdriver/docs/solutions/gke/managing-metrics) offering is the easiest way to collect these metrics and make them available in Google Cloud Monitoring.

Please follow the [instructions supplied by Google](https://cloud.google.com/stackdriver/docs/solutions/gke/managing-metrics) to enable Workload Metrics for your cobrowse enterprise cluster. Once enabled, you will be instructed to deploy a `PodMonitor` resource which specifies which metrics to collect.

For convenience, we have provided 2 sample `PodMonitor` resources in your cobrowse enterprise for GCP terraform configuration:

{% hint style="info" %}
Consider adding configurations from the [Managing costs](https://cloud.google.com/stackdriver/docs/solutions/gke/managing-metrics#managing-costs) section of the Workload Metrics instructions to manage how often metrics are scraped and which metrics are collected
{% endhint %}

* **Redis Cluster:** `cobrowse-redis-cluster-podmonitor.yml`
  * Update the `metadata.namespace` property to the namespace where your redis cluster is deployed
* **Cobrowse:** `etc/metrics/cobrowse-svc-podmonitor.yml`
  * Update the `metadata.namespace` property to the namespace where your cobrowse services are deployed

Once the PodMonitor resources are applied to your cluster namespace, metrics should begin showing up in Cloud Monitoring in a few minutes.

### Azure Monitor

If you are running the Cobrowse Enterprise Azure terraform configuration, Azure Monitor provides a [Prometheus integration](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-prometheus-integration) for its container insights offering. To configure the integration you'll be asked to sconfigure scrape settings (we have configured this already) and deploy a `ConfigMap` resource.

#### Prometheus scraping settings <a href="prometheus-scraping-settings" id="prometheus-scraping-settings"></a>

In the [prometheus scraping settings](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-prometheus-integration#prometheus-scraping-settings) section of the prometheus integration instructions, we will be using the "Cluster-wide" perspective using **Pod annotation** endpoints. The Cobrowse Enterprise for Azure Terraform configuration already applies all the pod annotations necessary for this integration, so no change to your cluster is required in this section of the instructions.

#### Configure and deploy ConfigMaps <a href="configure-and-deploy-configmaps" id="configure-and-deploy-configmaps"></a>

In the [Configure and deploy ConfigMaps](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-prometheus-integration#configure-and-deploy-configmaps) section of the prometheus integration instructions, you will need to create a new `ConfigMap` resource in your cluster to configure the Prometheus integration exporter to collect the application metrics.

For convenience, we have proided a sample configuration resource that has the key changes made from the Azure template to enable Prometheus metrics scraping. This configuration file is located in your cobrowse enterprise for Azure terraform directory at path `container-azm-ms-agentconfig.example.yaml`. The key changes made are:

* `[prometheus-data-collection-settings].[prometheus_data_collection_settings.cluster].monitor_kubernetes_pods`
  * Value changed to `true`

{% hint style="warning" %}
This example configuration will cause all pods in your cluster that have prometheus scraping enabled in their pod annotations to have their prometheus metrics collected and sent to Azure Monitor container insights.

If this is cost prohibitive, consider using the `fieldpass` and `fielddrop` settings to configure which metrics are collected. Furthermore, if you have multiple namespaces that contain other services whose prometheus metrics you do not wish to scrape, you can use the `monitor_kubernetes_pods_namespaces` setting to limit the namespaces where pod metrics are scraped.

More information to help control your Azure Monitor spend can be found in the [Container Insights pricing documentation](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-cost).
{% endhint %}

Once the `ConfigMap` resource is deployed, custom metrics should start being collected and available within the **Azure Monitor Logs** section of the Azure Portal within a few minutes.

### Prometheus and Grafana

If you are self-hosting your Cobrowse Enterprise kubernetes cluster, then deploying your own Prometheus and Grafana instances into your cluster may make the most sense for you.

#### Prometheus

If you do not have Prometheus running in your cluster already, then you may consider using the [Prometheus Community Helm Chart](https://github.com/prometheus-community/helm-charts) to deploy the components necessary to get you started.

There is one chart that deploys a configurable node-exporter, alertnamanager and server component with ingress, which can get you started querying metrics as quickly a possible:

{% hint style="warning" %}
These example configurations and commands are intended to demonstrate the Helm repositories and chart resources you can use as a reference to help deploy a non-production prometheus instance that is capable of scraping cobrowse redis cluster and service metrics.

For a production-grade deployment with appropriate **authentication**, **persistence**, and **ingress**, we recommend you research the [Prometheus configuration options](https://prometheus.io/docs/prometheus/latest/configuration/configuration/) and the [full prometheus helm chart values.yaml file](https://github.com/prometheus-community/helm-charts/blob/main/charts/prometheus/values.yaml) that are available to help you make the appropriate decisions for your organization.
{% endhint %}

```yaml
# values.yml
server:
  baseURL: http://<prometheus host>/prometheus/
  prefixURL: /prometheus
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx
    enabled: true
    hosts:
      - <prometheus host>/prometheus
    path: /prometheus
```

```bash
> helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
> helm repo update
> helm install prometheus prometheus-community/prometheus -f values.yml
```

#### Grafana

If you do not have Grafana installed in your cluster already, then may consider using the [Grafana official Helm Chart](https://github.com/grafana/helm-charts) to deploy the components necessary to get you started.

There is one chart that deploys the Grafana server with ingress and configuration to query your prometheus instance, which can get you querying metrics as quickly as possible:

{% hint style="warning" %}
These example configurations and commands are intended to demonstrate the Helm repositories and chart resources you can use as a reference to help deploy a non-production Grafana server that is capable of querying a local Prometheus server.

For a production-grade deployment with appropriate **authentication**, **persistence**, and **ingress**, we recommend you research the [Grafana configuration options](https://grafana.com/docs/grafana/latest/administration/configuration/) and the [full Grafana helm chart values.yaml](https://github.com/grafana/helm-charts/blob/main/charts/grafana/values.yaml) file that are available to make the appropriate decisions for your organization.
{% endhint %}

```yaml
# values.yml
persistence:
  enabled: true
  accessModes:
    - ReadWriteOnce
  size: 5Gi
grafana.ini:
  server:
    domain: <grafana host>
    root_url: "http://<grafana host>/grafana"
    serve_from_sub_path: true
ingress:
  annotations:
    kubernetes.io/ingress.class: nginx
  enabled: true
  hosts:
    - <grafana host>
  path: /grafana
datasources:
  datasources.yaml:
    apiVersion: 1
    datasources:
      - name: Prometheus
        type: prometheus
        url: http://<prometheus host>/prometheus
        access: proxy
        isDefault: true
```

```bash
> helm repo add grafana https://grafana.github.io/helm-charts
> helm repo update
> helm install grafana grafana/grafana -f values.yml
```

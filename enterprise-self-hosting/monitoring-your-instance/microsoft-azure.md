# Microsoft Azure

If you are running the Cobrowse Enterprise Azure terraform configuration, Azure Monitor provides a [Prometheus integration](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-prometheus-integration) for its container insights offering. To configure the integration you'll be asked to sconfigure scrape settings (we have configured this already) and deploy a `ConfigMap` resource.

#### Prometheus scraping settings <a href="#prometheus-scraping-settings" id="prometheus-scraping-settings"></a>

In the [prometheus scraping settings](https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-prometheus-integration#prometheus-scraping-settings) section of the prometheus integration instructions, we will be using the "Cluster-wide" perspective using **Pod annotation** endpoints. The Cobrowse Enterprise for Azure Terraform configuration already applies all the pod annotations necessary for this integration, so no change to your cluster is required in this section of the instructions.

#### Configure and deploy ConfigMaps <a href="#configure-and-deploy-configmaps" id="configure-and-deploy-configmaps"></a>

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

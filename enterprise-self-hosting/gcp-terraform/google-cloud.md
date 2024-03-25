---
description: >-
  Utilize Cobrowse's Google Cloud Terraform configuration for an effortless
  setup on GCP, ensuring efficient metric collection via Google Cloud's Workload
  Metrics
---

# GCP metrics configuration

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

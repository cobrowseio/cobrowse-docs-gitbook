# Monitoring your instance

### OpenMetrics / Prometheus

OpenMetrics/Prometheus exposition format has become a widely adopted standard and provides the most flexibility in how you can consume and use your application metrics. Cobrowse Enterprise services make Node.js, Redis and custom application metrics available in the text-based OpenMetrics/Prometheus exposition format.

The right service to consume these metrics is largely dependent on your organization and the existing tools and expertise available to you. Because of this, while Cobrowse offers a variety of example configurations and guidance in this article for the most popular use-cases, it is your responsibility to provision and configure the resources that make the most sense for your organization and needs.

{% hint style="info" %}
If you use a hosted service to collect application metrics, keep in mind that collecting more metrics into the service **almost always increases costs.**

Without additional filters, collecting all application metrics into such a service can add **hundreds of new metrics**, depending on how the service handles different combinations of OpenMetrics labels and values.
{% endhint %}

### Metrics Endpoint Details

Application metrics for each of our Kubernetes Deployments / StatefulSets are exposed through an HTTP endpoint in each Kubernetes Pod on port `8080` and on path `/metrics`. You can verify that metrics are being exposed by accessing a terminal session to a pod and requesting `http://localhost:8080/metrics` :

```bash
kubectl exec -it <pod name> -- /bin/bash
bash-5.1$ wget -O- http://localhost:8080/metrics
```

If metrics are successfully exposed, you should receive a text document in your terminal listing a variety of labelled process, NodeJS and Cobrowse metric values. Find more details for your deployment type below:

{% content-ref url="aws-terraform/amazon-web-services.md" %}
[amazon-web-services.md](aws-terraform/amazon-web-services.md)
{% endcontent-ref %}

{% content-ref url="gcp-terraform/google-cloud.md" %}
[google-cloud.md](gcp-terraform/google-cloud.md)
{% endcontent-ref %}

{% content-ref url="azure-terraform/microsoft-azure.md" %}
[microsoft-azure.md](azure-terraform/microsoft-azure.md)
{% endcontent-ref %}

{% content-ref url="monitoring-your-instance/self-hosted-prometheus.md" %}
[self-hosted-prometheus.md](monitoring-your-instance/self-hosted-prometheus.md)
{% endcontent-ref %}

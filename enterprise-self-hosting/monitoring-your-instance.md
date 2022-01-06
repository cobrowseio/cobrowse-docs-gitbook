# Monitoring your instance

Cobrowse Enterprise services make Node.js, Redis and custom application metrics available in the text-based OpenMetrics/Prometheus exposition format. OpenMetrics/Prometheus exposition format has become a widely adopted standard and provides the most flexibility in how you can consume and use your application metrics.

The right service to consume it is largely dependent on your organization and the existing tools and expertise you have at your disposal. Because of this, while cobrowse offers a variety of example configurations and guidance in this article for the most popular use-cases, it is your responsibility to provision and configure the resources that make the most sense for your organization and needs.

{% hint style="warning" %}
If you use a hosted service to collect application metrics, keep in mind that collecting more metrics into the service **almost always increases costs.**

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

If metrics are successfully exposed, you should receive a text document in your terminal listing a variety of labelled process, nodejs and cobrowse metric values.
{% endhint %}

Please explore the subsections of this article to find the deployment guidance that is best for you:

* [Amazon Web Services](monitoring-your-instance/amazon-web-services.md)
* [Google Cloud](monitoring-your-instance/google-cloud.md)
* [Microsoft Azure](monitoring-your-instance/microsoft-azure.md)
* [Self-Hosted Prometheus](monitoring-your-instance/self-hosted-prometheus.md)

## Custom metrics provided

Some of the custom cobrowse metrics we provide are:

| Metric Name               | Metric Type | Description                    |
| ------------------------- | ----------- | ------------------------------ |
| **cobrowse\_device\_registrations\_total** | Counter | Count of devices registered |
| **cobrowse\_presence\_queue\_delay\_seconds** | Gauge | Current wait time for presence queue processing (should be close to 0 most of the time) |
| **cobrowse\_presence\_queue\_length\_count** | Gauge | Current number of sockets waiting in the presence system processing queue (should be close to 0 most of the time) |
| **cobrowse\_session\_created\_total** | Counter | Count of sessions created |
| **cobrowse\_session\_duration\_seconds** | Summary | Duration of sessions |
| **cobrowse\_session\_unused\_total** | Counter | Count of sessions that were created but then never activated |
| **cobrowse\_websocket\_byte\_in\_total** | Counter | Count of websocket bytes received |
| **cobrowse\_websocket\_connection\_duration\_seconds** | Summary | Duration of websocket connection |
| **cobrowse\_websocket\_connection\_end\_total** | Counter | Count of websocket connections ended |
| **cobrowse\_websocket\_connection\_start\_total** | Counter | Count of websocket connections started |
| **cobrowse\_websocket\_connections\_count** | Gauge | Current number of open websocket connections |
| **cobrowse\_websocket\_message\_in\_total** | Counter | Count of websocket messages received |
| **cobrowse\_websocket\_message\_size\_bytes** | Summary | Size of websocket message |

Many of these will depend on the behaviour of your deployment and use case, so we do not provide specific ranges thresholds to monitor by default. You should monitor and decide what the appropriate limits are for your deployment.

As well as these custom metrics, the prometheus client also collects some generic system metrics and node specific metrics, see below for more information:

{% embed url="https://prometheus.io/docs/instrumenting/writing_clientlibs#standard-and-runtime-collectors" %}

{% embed url="https://github.com/siimon/prom-client/tree/master/lib/metrics" %}

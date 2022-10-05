# Self-Hosted Prometheus

If you are self-hosting your Cobrowse Enterprise Kubernetes cluster, then deploying your own Prometheus and Grafana instances into your cluster may make the most sense for you.

### Prometheus

If you do not have Prometheus running in your cluster already, then you may consider using the [Prometheus Community Helm Chart](https://github.com/prometheus-community/helm-charts) to deploy the components necessary to get you started.

{% hint style="warning" %}
These example configurations and commands are intended to demonstrate the Helm repositories and chart resources you can use as a reference to help deploy a non-production Prometheus instance that is capable of scraping cobrowse redis cluster and service metrics.

For a production-grade deployment with appropriate **authentication**, **persistence**, and **ingress**, we recommend you research the [Prometheus configuration options](https://prometheus.io/docs/prometheus/latest/configuration/configuration/) and the [full Prometheus helm chart values.yaml file](https://github.com/prometheus-community/helm-charts/blob/main/charts/prometheus/values.yaml) that are available to help you make the appropriate decisions for your organization.
{% endhint %}

There is one chart that deploys a configurable node-exporter, alertmanager and server component with ingress, which can get you started querying metrics as quickly a possible:

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

### Grafana

If you do not have Grafana installed in your cluster already, then may consider using the [Grafana official Helm Chart](https://github.com/grafana/helm-charts) to deploy the components necessary to get you started.

{% hint style="warning" %}
These example configurations and commands are intended to demonstrate the Helm repositories and chart resources you can use as a reference to help deploy a non-production Grafana server that is capable of querying a local Prometheus server.

For a production-grade deployment with appropriate **authentication**, **persistence**, and **ingress**, we recommend you research the [Grafana configuration options](https://grafana.com/docs/grafana/latest/administration/configuration/) and the [full Grafana helm chart values.yaml](https://github.com/grafana/helm-charts/blob/main/charts/grafana/values.yaml) file that are available to make the appropriate decisions for your organization.
{% endhint %}

There is one chart that deploys the Grafana server with ingress and configuration to query your Prometheus instance, which can get you querying metrics as quickly as possible:

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

# Amazon Web Services

Cobrowse exposes its application metrics using the OpenMetrics API (i.e., Prometheus data format). If you are hosting Cobrowse Enterprise in AWS, our Terraform can set everything up to export your Cobrowse EKS cluster metrics into CloudWatch for charting and alerts. We also include by default a sample CloudWatch dashboard as part of this deployment.

AWS provides a [comprehensive set of instructions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ContainerInsights-Prometheus-Setup.html) for installing the CloudWatch agent for a variety of existing OpenMetrics workload metrics, however none of these metrics are relevant to the Cobrowse deployment and related technologies. Additionally, the instructions to follow are slightly different when using an EC2 launch target for EKS instead of a Fargate launch target. To get you started exporting your Cobrowse prometheus metrics to CloudWatch as quickly as possible, we have summarized the instructions tailored to the Cobrowse Enterprise EKS deployment below.

### Prerequisites

1. `terraform` has been [installed](https://www.terraform.io/).
2. `kubectl` has been [installed](https://kubernetes.io/docs/tasks/tools/).
3. AWS CLI has been [installed](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
4. You have successfully deployed and verified the functionality of your Cobrowse Enterprise AWS Terraform environment.
5. You have configured `kubectl` to communicate with your Cobrowse Enterprise AWS EKS cluster using `aws eks update-kubeconfig --name cobrowse-enterprise` (or similar).

### Update the Terraform config

If you have followed our AWS terraform guide and created the config directory, you should already have a `terraform.tfvars.json` file at the root of that directory. This config file is formatted in standard JSON and will already contain some keys and values gathered during the initial setup. To enable metrics collection, add a `metrics` key with the value set to `true`.

```json
{
  "domain": "...",
  "stage": "...",
  "license": "...",
  "docker_password": "...",
  "region": "...",
  "documentdb_instances": "...",
  "superusers": "...",
  "metrics": "true"
}
```

**Note:** Please remember to add the trailing comma at the end of the previous line and to wrap the `true` value in double quotes.

### Deploy the Terraform

Apply the changes by running the Terraform with the following command:

```bash
terraform apply
```

This will list the modifications that Terraform will make to your AWS account. If everything looks good type `yes` to continue with the deployment.

### Verify that the agent is running

Once the resources are deployed, you can run the following commands to check the status of the deployment:

```bash
kubectl get pod -n fargate-container-insights
kubectl get statefulsets -n fargate-container-insights
```

If the results include both a `cobrowse-metrics-collector` pod in the `Running` state as well as a statefulset, the metrics collector should be up and running.

### Explore your metrics in CloudWatch

After a few minutes of the metrics collector running, you should find that you have metrics in CloudWatch under the `ContainerInsights` namespace.

All metrics that are exported by the Cobrowse application pods are prefixed with `cobrowse_`.

{% hint style="info" %}
For AWS deployments, access to Redis metrics through OpenMetrics is not supported. Instead, please refer to your AWS ElastiCache resources in CloudWatch for all available monitoring metrics.
{% endhint %}

### Set your retention period for the metrics logs

When the metrics collector begins collecting Prometheus metrics, it creates a CloudWatch Logs LogGroup to log them in. By default, this log data expires in one year, but can easily be modified.

In your AWS Console, navigate to CloudWatch > Logs > LogGroups and find a log group named `/aws/containerinsights/cobrowse-enterprise/performance` or similar. You can now update the retention period of these logs to be inline with how long you wish to retain the metrics logs. Note that if you expire the log data, the data will still be available to explore in CloudWatch metrics dashboards and charts.


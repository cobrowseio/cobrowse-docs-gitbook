# Amazon Web Services

Cobrowse exposes its application metrics using the OpenMetrics API (i.e., Prometheus data format). If you are hosting Cobrowse Enterprise in AWS, the CloudWatch Agent for Prometheus is the easiest way to export your Cobrowse EKS cluster metrics into CloudWatch for charting and alerts.

AWS provides a [comprehensive set of instructions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ContainerInsights-Prometheus-Setup.html) for installing the CloudWatch agent for a variety of existing OpenMetrics workload metrics, however none of these metrics are relevant to the Cobrowse deployment and related technologies. Additionally, the instructions to follow are slightly different when using an EC2 launch target for EKS instead of a Fargate launch target. To get you started exporting your Cobrowse prometheus metrics to CloudWatch as quickly as possible, we have summarized the instructions tailored to the Cobrowse Enterprise EKS deployment below.

### Prerequisites

1. `kubectl` has been [installed](https://kubernetes.io/docs/tasks/tools/).
2. AWS CLI has been [installed](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
3. `eksctl` has been [installed](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html).
4. You have successfully deployed and verified the functionality of your Cobrowse Enterprise AWS Terraform envrionment.
5. You have configured `kubectl` to communicate with your Cobrowse Enterprise AWS EKS cluster using `aws eks update-kubeconfig --name cobrowse-enterprise` (or similar)

{% hint style="info" %}
**Cluster Name**: In the instructions that follow, we refer to your **cluster name**. If you have not modified the `name` or `stage` terraform deployment variable, your cluster name is `cobrowse-enterprise` by default. If you did modify one or both of those variables, your cluster name is configured as: `${name}-${stage}`
{% endhint %}

### Create IAM Service Account

This IAM service account enables the CloudWatch agent to send metric data to CloudWatch. AWS API commands issued by the CloudWatch agent will be bound by the policy permissions we will attach to this role.

To create the IAM Service Account and attach its policy, run the following command:

```bash
eksctl create iamserviceaccount \
  --name cwagent-prometheus \
  --namespace amazon-cloudwatch \
  --cluster cobrowse-enterprise \
  --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy \
  --approve \
  --override-existing-serviceaccounts
```

### Create CloudWatch Agent deployment

We have included a sample set of kubernetes templates that install all resources that are necessary to run the CloudWatch Agent in your EKS cluster, pre-configured to collect Cobrowse prometheus metrics and send them to CloudWatch. We encourage you to review the resources and optionally update them to suit your needs.

This kubernetes resource file is located at `etc/cwagent-prometheus.yaml` in your terraform directory.

Before applying the resources, you must first edit all instances of `{{region}}` in the templates to be the region where you've deployed your EKS cluster. Also, if you updated the `name` or `stage` terraform variables, you must update all instances of `cobrowse-enterprise` to be your cluster name generated as `${name}-${stage}`. Here is a sample command that applies the resources and updates the region and cluster name to sample values:

```bash
  cat ./etc/cwagent-prometheus.yaml |\
    sed "s/{{region}}/your-region/" |\
    sed "s/cobrowse-enterprise/your-cluster-name/" |\
    kubectl apply -f -
```

### Verify that the agent is running

Once the resources are deployed, you can run the following command to check the status of the deployment:

```bash
kubectl get pod -l "app=cwagent-prometheus" -n amazon-cloudwatch
```

If the results include a single CloudWatch agent pod in the `Running` state, the agent is running and collecting Prometheus metrics.

### Explore your metrics in CloudWatch

After a few minutes of the CloudWatch agent running, you should find that you have metrics in CloudWatch under the `ContainerInsights/Prometheus` namespace.

All metrics that are exported by the Cobrowse application pods are prefixed with `cobrowse_`.

{% hint style="info" %}
For AWS deployments, access to Redis metrics through OpenMetrics is not supported. Instead, please refer to your AWS ElastiCache resources in CloudWatch for all available monitoring metrics.
{% endhint %}

### Set your retention period for the metrics logs

When the CloudWatch agent begins collecting Prometheus metrics, it creates a CloudWatch Logs LogGroup to log them in. By default, this log data never expires, which is probably not what you want.

In your AWS Console, navigate to CloudWatch > Logs > LogGroups and find a log group named `/aws/containerinsights/cobrowse-enterprise/prometheus` or similar. You can now update the retention period of these logs to be inline with how long you wish to retain the metrics logs. Note that if you expire the log data, the data will still be available to explore in CloudWatch metrics dashboards and charts.

### Deploy a sample dashboard

We have included a sample monitoring dashboard that works well with the sample CloudWatch agent deployment configuration.

This CloudWatch dashboard JSON file is located at `etc/cwagent-dashboard.json` in your terraform directory.

Before deploying the dashboard, you must first edit all instances of `{{region}}` in the JSON file to be the region where you've deployed your EKS cluster. Here is a sample command that creates your dashboard using region `us-east-2`:

```bash
aws cloudwatch put-dashboard \
    --dashboard-name cobrowse-enterprise \
    --dashboard-body "$(cat ./etc/cwagent-dashboard.json | sed 's/{{region}}/us-east-2/')"
```


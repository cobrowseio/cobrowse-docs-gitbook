---
description: >-
  Deploy your own instance of Cobrowse in your own AWS, Azure, or Google Cloud
  account, or fully on-premises including support for air-gapped networks.
---

# Self-hosting overview

## Deployment options

Cobrowse is deployed as containers, and we support a range of deployments covering public clouds and on-premise enterprise requirements.

{% content-ref url="docker-compose.md" %}
[docker-compose.md](docker-compose.md)
{% endcontent-ref %}

{% content-ref url="helm/" %}
[helm](helm/)
{% endcontent-ref %}

{% content-ref url="aws-terraform/" %}
[aws-terraform](aws-terraform/)
{% endcontent-ref %}

{% content-ref url="azure-terraform/" %}
[azure-terraform](azure-terraform/)
{% endcontent-ref %}

{% content-ref url="gcp-terraform/" %}
[gcp-terraform](gcp-terraform/)
{% endcontent-ref %}

{% hint style="success" %}
Any questions or specific requirements to review? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io)!
{% endhint %}

## Software version compatibility

Depending on your choice, your deployment will already include specific versions of these components which you can adjust to your preference.

MongoDB: We recommend 7.x or 8.x for new deployments. The absolute minimum supported version is 3.2, but using an end-of-life version this old is strongly discouraged.

Redis: 6.x or 7.x but earlier versions are very likely supported. New deployments are recommended to use the latest available version.

Docker compose:  https://docs.docker.com/compose/

Kubernetes: v1.32, our deployment is known to run on older versions up to 1.23.

helm: 3.x

## Next steps

{% content-ref url="getting-started/" %}
[getting-started](getting-started/)
{% endcontent-ref %}

{% content-ref url="advanced/" %}
[advanced](advanced/)
{% endcontent-ref %}

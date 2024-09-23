---
description: Using private repositories on your deployment.
---

# Image Pull Secret

In order for your Kubernetes cluster to install the Cobrowse service components, you need to specify the Github token that Cobrowse provided to you.

The easiest way to specify this is in `values.yaml` in the `imageCredentials.password` value. If you wish to manage this secret externally from the Helm chart, you can follow these instructions.

1. Remove the token from the `imageCredentials.password` value
2. Deploy the Helm chart without the password. You will notice that the Helm deploy process will delete a Secret resource called `cobrowse-docker-cfg`. **At this time, new pods will no longer be able to pull the cobrowse enterprise docker images from Github**.
3. Deploy the `cobrowse-docker-cfg` Secret resource yourself with the command:

```bash
kubectl create secret docker-registry cobrowse-docker-cfg \
   --docker-server=ghcr.io \
   --docker-username=cobrowse-enterprise \
   --docker-password="<github token provided by cobrowse>"
```

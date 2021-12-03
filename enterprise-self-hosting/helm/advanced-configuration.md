# Advanced Configuration

## Image Pull Secret

In order for your kubernetes cluster to install the Cobrowse service components, you need to specify the Github token that Cobrowse provided to you.

The easiest way to specify this is in `values.yaml` in the `imageCredentials.password` value. If you wish to manage this secret externally from the Helm chart, you can follow these instructions.

1. Remove the token from the `imageCredentials.password` value
2. Deploy the Helm chart without the password. You will notice that the Helm deploy process will delete a Secret resource called `cobrowse-docker-cfg`. **At this time, new pods will no longer be able to pull the cobrowse enterprise docker images from Github**.
3. Deploy the `cobrowse-docker-cfg` Secret resource yourself with the command:

```bash
> kubectl create secret docker-registry cobrowse-docker-cfg \
>   --docker-server=ghcr.io \
>   --docker-username=cobrowse-enterprise \
>   --docker-password="<token provided by cobrowse>"
```

## Environment Variables

Many of the cobrowse service component configurations are managed using environment variables specified in `ConfigMap` and `Secret` resources, and these configurations can be overridden outside of the Helm chart by resources that you manage.

### ConfigMap Resources

To see a list of all `ConfigMap` resources managed by the Helm chart, you can run:

```bash
> kubectl get configmap -l "app=cobrowse-cobrowse-enterprise"
NAME                          DATA   AGE
cobrowse-api-envvars          10     25m
cobrowse-frontend-envvars     1      25m
cobrowse-proxy-envvars        4      25m
cobrowse-recording-envvars    3      25m
cobrowse-sockets-envvars      6      25m
```

Each of these **envvars** `ConfigMap`s can be overridden by creating `ConfigMap` resources named (respectively):

```
cobrowse-api-custom-envvars
cobrowse-frontend-custom-envvars
cobrowse-proxy-custom-envvars
cobrowse-recording-custom-envvars
cobrowse-sockets-custom-envvars
```

To see what environment variables are available to be configured for a particular service component, you can run (for example):

```bash
> kubectl describe configmap cobrowse-api-envvars
```

### Secret Resources

To see a list of all `Secret` resources managed by the Helm chart, you can run:

```bash
> kubectl get secret -l "app=cobrowse-cobrowse-enterprise"
NAME                         TYPE     DATA   AGE
cobrowse-api-envvars         Opaque   2      41m
cobrowse-frontend-envvars    Opaque   0      41m
cobrowse-proxy-envvars       Opaque   0      41m
cobrowse-recording-envvars   Opaque   0      41m
cobrowse-sockets-envvars     Opaque   2      41m
```

Each of these **envvars** `Secret`s can be overridden by creating `Secret` resources named (respectively):

```
cobrowse-api-custom-envvars
cobrowse-frontend-venvvars
cobrowse-proxy-custom-envvars
cobrowse-recording-custom-envvars
cobrowse-sockets-custom-envvars
```

{% hint style="info" %}
**Order of Priority**

The following order of priority is taken, when resolving environment variables using the **api** service component as an example:

1. **Secret:** cobrowse-api-custom-envvars
2. **Secret:** cobrowse-api-envvars
3. **ConfigMap:** cobrowse-api-custom-envvars
4. **ConfigMap**: cobrowse-api-envvars

Thus if you override an environment variable such as `redis_url` in the `cobrowse-api-custom-envvars` **ConfigMap**, then the value will be overridden by the Helm-managed `cobrowse-api-envvars` **Secret**. Thus, make sure before overriding an environment variable in the **ConfigMap** that it isn't set in one of the **Secret** resources first.

****
{% endhint %}

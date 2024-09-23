---
description: How your Cobrowse deployment can be configured.
---

# Environment Variables

Many of the Cobrowse service component configurations are managed using environment variables specified in `ConfigMap` and `Secret` resources, and these configurations can be overridden outside of the Helm chart by resources that you manage.

### ConfigMap Resources

To see a list of all `ConfigMap` resources managed by the Helm chart, you can run:

```bash
kubectl get configmap -l "app=cobrowse-cobrowse-enterprise"
NAME                          DATA   AGE
cobrowse-api-envvars          10     25m
cobrowse-frontend-envvars     1      25m
cobrowse-proxy-envvars        4      25m
cobrowse-recording-envvars    3      25m
cobrowse-sockets-envvars      6      25m
```

Each of these **envvars** `ConfigMap`s can be overridden by creating `ConfigMap` resources named (respectively):

{% hint style="info" %}
The naming of each resource is prefixed with your `.Release.Name`. The examples below assume the `.Release.Name` is **`cobrowse`**.
{% endhint %}

```
cobrowse-api-custom-envvars
cobrowse-frontend-custom-envvars
cobrowse-proxy-custom-envvars
cobrowse-recording-custom-envvars
cobrowse-sockets-custom-envvars
```

### Secret Resources

To see a list of all `Secret` resources managed by the Helm chart, you can run:

```bash
kubectl get secret -l "app=cobrowse-cobrowse-enterprise"
NAME                         TYPE     DATA   AGE
cobrowse-api-envvars         Opaque   2      41m
cobrowse-frontend-envvars    Opaque   0      41m
cobrowse-proxy-envvars       Opaque   0      41m
cobrowse-recording-envvars   Opaque   0      41m
cobrowse-sockets-envvars     Opaque   2      41m
```

Each of these **envvars** `Secret`s can be overridden by creating `Secret` resources named (respectively):

{% hint style="info" %}
The naming of each resource is prefixed with your `.Release.Name`. The examples below assume the `.Release.Name` is **`cobrowse`**.
{% endhint %}

```
cobrowse-api-custom-envvars
cobrowse-frontend-venvvars
cobrowse-proxy-custom-envvars
cobrowse-recording-custom-envvars
cobrowse-sockets-custom-envvars
```

{% hint style="info" %}
**Order of Priority**

The following order of priority is followed while resolving environment variables, using the **api** service component as an example:

1. **Secret:** cobrowse-api-custom-envvars
2. **Secret:** cobrowse-api-envvars
3. **ConfigMap:** cobrowse-api-custom-envvars
4. **ConfigMap**: cobrowse-api-envvars

Thus if you override an environment variable such as `redis_url` in the `cobrowse-api-custom-envvars` **ConfigMap**, then the value will be overridden by the Helm-managed `cobrowse-api-envvars` **Secret**. Thus, make sure before overriding an environment variable in the **ConfigMap** that it isn't set in one of the **Secret** resources first.

***
{% endhint %}

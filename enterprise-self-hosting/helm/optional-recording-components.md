---
description: Minimising resource usage by disabling the recording components.
---

# Optional recording components

If session recordings are not enabled for your account, you may optionally disable the recording pod and persistent volume requirements.

### Disable recording pods

The recording pods are responsible for converting stored sessions into video recordings, and are not a requirement if session recordings are disabled. To disable the recording pods, you can configure your value parameters to set the number of recording replicas to be `0`.

```yaml
# values.yml
recording:
  replicas: 0
```

### Disable persistent volume requirement

The persistent volume is responsible for persistent storage of past session recordings, and is not a requirement if session recordings are disabled. To disable the persistent volume requirement, you can configure your value parameters to set the `storage` parameter to `false`.

```yaml
# values.yaml
storage: false
```

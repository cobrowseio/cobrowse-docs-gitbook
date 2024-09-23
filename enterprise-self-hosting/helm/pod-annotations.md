---
description: Further configure your deployment by using pod annotations.
---

# Pod Annotations

When required, annotations can be set on the different Pods.

```yaml
# values.yml
api:
  pod:
    annotations:
      key: value
      another-key: value
frontend:
  pod:
    annotations:
      key: value
sockets:
  pod:
    annotations:
      key: value
recording:
  pod:
    annotations:
      key: value
proxy:
  pod:
    annotations:
      key: value
```

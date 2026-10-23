---
description: >-
  Control how long your self-hosted Cobrowse instance keeps session data by
  setting a retention period.
---

# Configuring data retention

The `data_retention:sessions` config value sets how long, in seconds, your instance keeps session metadata. It is not set by default so a self-hosted instance keeps every session forever.

{% hint style="info" %}
For a breakdown of the personal data Cobrowse stores, see [Data that Cobrowse stores](https://support.cobrowse.io/data-that-cobrowse-stores-in-its-hosted-clouds).
{% endhint %}

### Setting data\_retention:sessions

1. Firstly make sure you have configured at least one [superuser](adding-a-superuser.md) in your instance.
2. Open `/admin/configuration` on your instance while logged in as a superuser. e.g. https://example.com/admin/configuration
3. Enter a new configuration key called "**data\_retention:sessions**". The value should be the retention period in seconds.
4. Save and restart the API service for the new value to take effect. Existing sessions older than the retention period are deleted too.

Some common values:

| Retention period | Value      |
| ---------------- | ---------- |
| 30 days          | `2592000`  |
| 90 days          | `7776000`  |
| 1 year           | `31536000` |

{% hint style="info" %}
The retention period can also be configured by setting a "**data\_retention\_\_sessions**" environment variable on the API service of your deployment.
{% endhint %}

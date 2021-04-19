# Adding a superuser

Superuser are the global administrators on your instance. They can access all accounts, priviliged sections of the system, and apply instance wide configurations. By default your Cobrowse Enterprise instance will not have any superusers configured.

To add a superuser, you must configure them as part of the infrastructure deployment. 

### Configuring superusers via Terraform

For Terraform based deployments you need to add a "superusers" entry to the `terraform.tfvars.json` that is created by our configuration utility. The value should be a regular expression that describes superuser emails. For example, `.*@example.com` would make all example.com users superusers.

```javascript
// This would make the single user myadmin@example.com a superuser
{
    "superusers": "myadmin@example.com"
}
```

### Configuring superusers via Docker Compose

For Docker Compose based deployments, you must configure the `SUPERUSERS` environment variable with a regular expression describing superuser emails. See the docs for [Docker Compose docs](../docker-compose.md).


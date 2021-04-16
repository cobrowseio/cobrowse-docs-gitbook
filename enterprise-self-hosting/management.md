# Managing your deployment

### Managing the database

You are responsible for backing up the database regularly. Cobrowse will not do this automatically in any way. See the [MonogDB docs](https://docs.mongodb.com/manual/core/backups/) for recommendations on backup strategies. [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) can be configured to do this automatically.

### Upgrading Cobrowse

The config directory created by our command line utility is a git repo. You can update to the latest version by doing:

```bash
> git pull upstream master
```

One you have done that, you can `terraform init && terraform apply` the to update your deployment.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}


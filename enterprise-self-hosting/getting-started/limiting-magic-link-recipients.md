---
description: How can you control who receives a magic link email?
---

# Limiting magic link recipients

If you wish to limit who can receive the magic links you can set `allowed_usernames` config value to a regular expression. If the email address given passes the regular expression the request to send a magic link will progress. No magic link email will be sent for any non-matching email address.

#### Setting allowed\_usernames via admin/configuration

1. Login to your Cobrowse dashboard as a [superuser](adding-a-superuser.md).
2. Visit `admin/configuration` for example https://example.com/admin/configuration
3. Add a new config value with the key being `allowed_usernames` and the value being a regular expression that will pass for any address you wish to receive a magic link email.
4. Save and restart the API service for the new values to take affect.

#### Setting allowed\_usernames via environment variable

Please set the `allowed_usernames` environment variable following the instructions for your deployment method.

* [Docker](https://docs.cobrowse.io/enterprise-self-hosting/docker-compose#customize-the-configuration)
* [Helm](https://docs.cobrowse.io/enterprise-self-hosting/helm#common-parameters)


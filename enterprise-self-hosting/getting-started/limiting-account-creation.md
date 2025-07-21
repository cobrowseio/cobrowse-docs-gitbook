---
description: >-
  Control account creation on your self-hosted Cobrowse instance by configuring
  specific email addresses or domains that are allowed to create new accounts.
---

# Limiting account creation

By default your Cobrowse Enterprise instance supports multiple tenants and new account creation will be restricted to [superusers](adding-a-superuser.md). In most instances you will want to change this to allow other members of your organisation to also create accounts.&#x20;

### Configuring account creators

1. Firstly make sure you have configured at least one [superuser](adding-a-superuser.md) in your instance.&#x20;
2. Open `/admin/configuration` on your instance while logged in as a superuser. e.g. https://example.com/admin/configuration
3. Enter a new configuration key called "**account\_creators**". The value should be a regular expression that describes the email addresses that should be allowed to create new accounts. For example `.*@example.com` would allow anyone with an example.com email to create an account.

### What do we mean by account?

An account in your Cobrowse instance is a single tenant that groups together devices, users, and cobrowsing sessions. You may choose to use a single account for all your devices, or create separate accounts for dev, test, production etc...


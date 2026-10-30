---
description: >-
  Limit what every user and token can do across all accounts on your self-hosted
  instance.
---

# Configuring an instance policy

The `instance_policy` config value is a policy applied to every API request on your instance. It can only take access away: whatever a user's role or a JWT allows, the instance policy is intersected with it, so nothing can exceed the instance policy.

Use it to enforce a rule across every account on the instance, for example turning off access to recordings, or making audit events read-only.

It uses the same format as [JWT policies](../../agent-side-integrations/json-web-tokens-jwts/jwt-policies.md).

{% hint style="info" %}
Superusers are not limited by the instance policy. A superuser signed in to the dashboard, or a service JWT with the `superuser` role, uses its own superuser policy instead.
{% endhint %}

#### Writing the policy

The value is a JSON object, stored as a string. It must follow these rules:

* **Include `"version": 3`.** A policy without a version is read as a legacy version 1 policy and will be rejected as invalid.
* **Include `"*": {}`.** Any resource the policy does not name falls back to `*`. Without it every unnamed resource is denied, which blocks almost all access to the instance.
* **List `permissions` only to restrict them.** Leaving `permissions` out of a resource leaves it unrestricted. Listing them allows only those actions: `create`, `read`, `update`, `delete`. An empty list `[]` denies the resource entirely.
* **Use only supported resource names.** An unknown resource makes the whole policy invalid. Your instance lists the supported resources at `/api/1/policy/schema`.

The instance policy cannot grant anything. Listing all four permissions on a resource does not give a user more than their role already allows.

#### Example policies

Deny access to session recordings on every account:

```json
{
  "version": 3,
  "*": {},
  "recordings": { "permissions": [] }
}
```

Allow audit events to be read, but never changed or deleted:

```json
{
  "version": 3,
  "*": {},
  "auditevents": { "permissions": ["read"] }
}
```

Only allow access to devices whose custom data has `region` set to `eu`:

```json
{
  "version": 3,
  "*": {},
  "devices": { "custom_data": { "region": "eu" } }
}
```

#### Setting `instance_policy`

{% hint style="warning" %}
If the instance policy is invalid, every API request fails with a `401` and the error `instance_policy_invalid`, including the configuration pages you would use to fix it. Check your JSON before saving, and see Recovering from an invalid policy.
{% endhint %}

You can set the policy in three ways. The API service reads configuration only at startup, so whichever you use, restart the API service for the new value to take effect.

**From the dashboard**

1. Firstly make sure you have configured at least one superuser in your instance.
2. Open `/admin/configuration` on your instance while logged in as a superuser. e.g. https://example.com/admin/configuration
3. Enter a new configuration key called "**instance\_policy**". The value should be your policy JSON.
4. Save and restart the API service for the new value to take effect.

**From the API**

As a superuser, send the policy as a JSON string in the `value` field. The value must be a string: a JSON object is rejected with `invalid_config`.

```bash
curl -X PUT https://example.com/api/1/service/configuration/instance_policy \
  -H "Authorization: Bearer <superuser token>" \
  -H "Content-Type: application/json" \
  -d '{"value": "{\"version\":3,\"*\":{},\"recordings\":{\"permissions\":[]}}"}'
```

To remove the policy, send a `DELETE` to the same URL. Either way, restart the API service afterwards.

**With an environment variable**

Set an `instance_policy` environment variable on the **api** service. For Helm deployments, see Environment Variables. An environment variable takes precedence over the value stored in the database.

#### Checking the policy

* `GET /api/1/service/configuration` lists configuration values as a superuser, but shortens each value to its first and last three characters, so it confirms the key is set rather than showing the policy.
* `GET /api/1/policy/current` returns the effective policy for the user or token making the request, with the instance policy already applied. Call it as an agent or with an agent JWT to see what they can do. Called as a superuser, it shows the superuser policy instead.

#### Recovering from an invalid policy

An invalid policy is only detected when the first request arrives after a restart, and from then on the dashboard and API refuse every request. To recover:

1. Set the `instance_policy` environment variable on the **api** service to a policy that restricts nothing: `{"version":3,"*":{}}`. This takes precedence over the invalid value in the database.
2. Restart the API service.
3. Fix or delete `instance_policy` at `/admin/configuration`.
4. Remove the environment variable and restart the API service again.

{% hint style="success" %}
Any questions at all? Please email us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}

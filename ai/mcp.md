---
description: >-
  Enable your AI agents to cobrowse with users by integrating the Cobrowse MCP server.
---

# Model Context Protocol (MCP)

## URL

The Cobrowse MCP server endpoint is available at:

```
https://cobrowse.io/virtual-agent/1/mcp
```

{% hint style="info" %}
If you self-host your instance, then replace`cobrowse.io` with your `<your instance domain>`.
{% endhint %}

Cobrowse MCP supports the Streamable HTTP transport.

## Authentication

The Cobrowse MCP server uses the standard `Authorization` header with a [Cobrowse JWT bearer token](../agent-side-integrations/json-web-tokens-jwts/README.md) to secure communications.

```
Authorization: Bearer <jwt_token>
```

## Billing

Virtual agent sessions are billed separately from standard Cobrowse usage.

{% hint style="success" %}
Have questions about virtual agent pricing? Contact us at [hello@cobrowse.io](mailto:hello@cobrowse.io).
{% endhint %}
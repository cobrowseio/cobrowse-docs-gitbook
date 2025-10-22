---
description: >-
  Enable your AI agents to cobrowse with users by integrating the Cobrowse MCP server.
---

# Model Context Protocol (MCP)

## URL

The Cobrowse MCP server endpoint is available at:

```
https://<your instance domain>/virtual-agent/1/mcp
```

Cobrowse MCP supports the Streamable HTTP transport.

## Authentication

The Cobrowse MCP server uses the standard `Authorization` header with a [Cobrowse JWT bearer token](../agent-side-integrations/json-web-tokens-jwts/README.md) to secure communications.

```
Authorization: Bearer <jwt_token>
```

## Tools

Cobrowse provides multiple tools for AI agents. To discover the complete set of available tools, use a [`tools/list` request](https://modelcontextprotocol.io/specification/2025-06-18/server/tools#listing-tools). Tools like [@modelcontextprotocol/inspector](https://npmjs.com/package/@modelcontextprotocol/inspector) enable quick exploration of Cobrowse MCP capabilities.

### connect_device

Initiates a cobrowsing session using a Cobrowse device identifier.

### join_session

Initiates a cobrowsing session using a [six-digit code](../sdk-features/6-digit-codes.md).

### get_html

Retrieves the semantic representation of the end user's current view. Requires an active cobrowsing session.

### highlight_element_and_wait

Highlights an element on the page and waits for user interaction. Requires an active cobrowsing session.
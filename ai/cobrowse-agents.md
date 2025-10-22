---
description: >-
  AI-powered virtual agents for automated customer support
---

# Cobrowse virtual agents

Cobrowse provides ready-to-use virtual agents powered by ElevenLabs, offering seamless chat and voice interactions with your users.

## Enablement

Virtual agents are configured in the settings panel of the Cobrowse Dashboard. To enable production-grade virtual agents, add your [ElevenLabs](https://elevenlabs.io) API key.

Each virtual agent has:
- A **display name** that users see (e.g., "Jessica")
- An **identifier** used for SDK integration
sica for the trial agent) and an identifier for use in the SDK.

## SDK integration

Configure the virtual agent before initializing Cobrowse:

```javascript
CobrowseIO.virtualAgent = 'my-agent-identifier';
```

When Cobrowse starts, the chat and voice widget will automatically appear on the page.

## Trial agents

A free trial agent is available for testing. Trial agents have limited conversations, duration, and concurrency.

By default, trial agents perform web searches at the start of each conversation to retrieve relevant help information for users.

## Customizing production agents

Production virtual agents connected to your ElevenLabs account have no usage limits and offer [full customization options](https://elevenlabs.io/docs/agents-platform/overview), including:
- Telephony support
- Conversation behavior and personality
- Voice selection and tuning
- Widget appearance and branding
- Additional MCP integrations

**Note:** Production agents do not perform automatic web searches. You must configure a [knowledge base](https://elevenlabs.io/docs/agents-platform/customization/knowledge-base/rag) with relevant content to provide context for your virtual agent.

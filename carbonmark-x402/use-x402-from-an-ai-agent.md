---
description: "Connect the x402 Carbon Retirement API to a wallet-connected AI agent so it can discover, quote and retire carbon credits from a plain-language request."
---

<!--
  DO NOT EDIT. Published automatically from Carbonmark/x402-klima-RA-new/api/docs/4_base_mcp.md.
  Changes made here will be overwritten by the next docs sync.
  Edit the source file and open a PR there instead.
-->

# Use x402 from an AI agent

Wire the x402 Carbon Retirement API into any Base MCP–capable AI agent so it can discover, quote, prepare, and submit a carbon retirement from a plain-language request.

The x402 Carbon Retirement API is built for the x402 agent-payments ecosystem and plugs directly into Base MCP. The fastest path is the Carbonmark x402 Base MCP plugin, which connects the endpoint to agents such as Claude Code.

{% hint style="warning" %}
On 27 October 2026 the `base-mcp` skill was removed from `base/skills`; Coinbase now maintains it as `wallet-mcp` in [`coinbase/agents`](https://github.com/coinbase/agents). The setup guide linked below was written for the earlier layout. If its install step fails, use one of the direct integrations under [Prefer a direct integration?](#prefer-a-direct-integration), which do not depend on Base MCP.
{% endhint %}

## Setup

{% stepper %}

{% step %}
#### Install

Connect Base MCP and add the Carbonmark x402 plugin by following the [Base MCP setup guide](https://github.com/Carbonmark/x402-retire-sdk/blob/main/base-mcp-setup.md). The plugin definition is [`plugins/carbonmark-x402.md`](https://github.com/Carbonmark/x402-retire-sdk/blob/main/plugins/carbonmark-x402.md).
{% endstep %}

{% step %}
#### Ask in plain language

For example: "Retire 2 tonnes of carbon under $15/tonne, beneficiary 'Acme Corp'."
{% endstep %}

{% step %}
#### Approve once

The agent runs `discover` → `quote` → `prepare/retire`, shows you the cost, submits a single wallet approval, and returns your Carbonmark certificate URL.
{% endstep %}

{% endstepper %}

## Prefer a direct integration?

You do not need MCP to use the endpoint. You can:

* Call the GET or POST endpoints yourself, then sign and submit the `prepare/retire` batch (`approve` + retirement) with your own wallet on Base — see [Retire carbon with x402](./retire-carbon-with-x402.md).
* Use the gasless relay path — sign one authorization and let the API's executor submit it — see [Gasless retirement (paid relay)](./gasless-retirement-paid-relay.md).

## Agent discovery

x402 directories and agent crawlers can index the endpoint automatically through its public well-known documents:

| Document | URL |
| --- | --- |
| Manifest | [`/.well-known/x402.json`](https://x402.carbonmark.com/.well-known/x402.json) |
| Error registry | [`/.well-known/x402-errors.json`](https://x402.carbonmark.com/.well-known/x402-errors.json) |
| Changelog | [`/.well-known/x402-changelog.json`](https://x402.carbonmark.com/.well-known/x402-changelog.json) |
| Docs index | [`/api/docs`](https://x402.carbonmark.com/api/docs) |

## Resources

* [Agent plugin + setup docs](https://github.com/Carbonmark/x402-retire-sdk) — Base MCP setup guide, carbonmark-x402 plugin, and full integration docs
* [Base MCP setup guide](https://github.com/Carbonmark/x402-retire-sdk/blob/main/base-mcp-setup.md) — connect Base MCP and add the Klima retirement plugin
* [Base MCP](https://docs.base.org/ai-agents/quickstart) — Base AI agent quickstart for wallet-connected agents on Base mainnet
* [Retirement Aggregator USAGE](https://github.com/KlimaDAO/retirement-aggregator/blob/main/USAGE.md) — direct on-chain access for callers who do not want the HTTP layer

{% hint style="warning" %}
Retirement is irreversible. Review the tonnes, price, and fee the agent presents before approving — a confirmed retirement cannot be undone, refunded, or resold.
{% endhint %}

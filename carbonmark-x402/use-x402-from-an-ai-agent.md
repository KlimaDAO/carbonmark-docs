<!--
  DO NOT EDIT. Published automatically from Carbonmark/x402-klima-RA-new/api/docs/4_base_mcp.md.
  Changes made here will be overwritten by the next docs sync.
  Edit the source file and open a PR there instead.
-->

# Use x402 from an AI agent

Wire the x402 Endpoint into any Base MCP–capable AI agent so it can discover, quote, prepare, and submit a carbon retirement from a plain-language request.

The x402 Endpoint is built for the x402 agent-payments ecosystem and plugs directly into Base MCP. The fastest path is the Klima Base MCP plugin, which connects the endpoint to agents such as Claude Code.

## Setup

{% stepper %}

{% step %}
#### Install

Connect Base MCP and add the Klima plugin by following the [Base MCP setup guide](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/blob/main/base-mcp-setup.md). The plugin definition is [`plugins/klima-retire.md`](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/blob/main/plugins/klima-retire.md).
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
* Use the gasless relay path — sign one authorization and let a Klima executor submit it — see [Gasless retirement (paid relay)](./gasless-retirement-paid-relay.md).

## Agent discovery

x402 directories and agent crawlers can index the endpoint automatically through its public well-known documents:

| Document | URL |
| --- | --- |
| Manifest | [`/.well-known/x402.json`](https://x402.klimalabs.com/.well-known/x402.json) |
| Error registry | [`/.well-known/x402-errors.json`](https://x402.klimalabs.com/.well-known/x402-errors.json) |
| Changelog | [`/.well-known/x402-changelog.json`](https://x402.klimalabs.com/.well-known/x402-changelog.json) |
| Docs index | [`/api/docs`](https://x402.klimalabs.com/api/docs) |

## Resources

* [Agent plugin + setup docs](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation) — Base MCP setup guide, klima-retire plugin, and full integration docs
* [Base MCP setup guide](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/blob/main/base-mcp-setup.md) — connect Base MCP and add the Klima retirement plugin
* [Base MCP](https://docs.base.org/ai-agents/quickstart) — Base AI agent quickstart for wallet-connected agents on Base mainnet
* [Klima Aggregator USAGE](https://github.com/KlimaDAO/retirement-aggregator/blob/main/USAGE.md) — direct on-chain access for callers who do not want the HTTP layer

{% hint style="warning" %}
Retirement is irreversible. Review the tonnes, price, and fee the agent presents before approving — a confirmed retirement cannot be undone, refunded, or resold.
{% endhint %}

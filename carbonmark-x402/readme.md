<!--
  DO NOT EDIT. Published automatically from Carbonmark/x402-klima-RA-new/api/docs/1_intro.md.
  Changes made here will be overwritten by the next docs sync.
  Edit the source file and open a PR there instead.
-->

# x402 Endpoint

Retire tokenized carbon credits on Base directly from an AI agent or any HTTP client, with no inventory to manage and no SDK required.

The Klima x402 Endpoint is co-developed by Carbonmark. It exposes carbon retirement on the Base blockchain through the Klima Protocol Retirement Aggregator as plain HTTP calls: it discovers carbon liquidity, returns live price quotes, hands back unsigned `[approve, retire]` calldata, and resolves the public Carbonmark certificate once the transaction confirms.

The endpoint is built for the x402 agent-payments ecosystem and plugs directly into Base MCP, so an agent can read the catalog, prepare a retirement, and submit it through the user's Base Account wallet in a single approval.

## What you can do

With the x402 Endpoint, you can:

* Discover retirable carbon classes, credits, and reference prices on Base
* Retrieve live, on-chain quotes for a given tonnage in USDC or kVCM
* Receive unsigned `approve` + `retire` calldata to submit from your own wallet
* Retire gaslessly through a relay by signing a single token authorization
* Resolve the public Carbonmark certificate for any confirmed retirement
* Integrate carbon retirement into AI agents via the Klima Base MCP plugin

## Endpoint basics

| Property | Value |
| --- | --- |
| GET base URL | `https://x402.klimalabs.com/api/...` |
| POST base URL | `https://x402.klimalabs.com/api` |
| Chain | Base mainnet (`chainId=8453`) |
| Auth | Free GET or POST — on-chain protocol fee only |
| Agent manifest | [`/.well-known/x402.json`](https://x402.klimalabs.com/.well-known/x402.json) |

All HTTP calls are free. Use **GET** with query parameters on action paths (`/discover`, `/quote`, and so on), or **POST** JSON to `/api` with an `action` field. Both return the same responses.

## Two ways to retire

The endpoint supports two integration paths. Choose based on whether you want to submit the transaction yourself or have a relay do it for you.

* **Build-your-own** — `discover` → `quote` → `prepare/retire` hands back unsigned `[approve, retire]` calldata that **you** submit. Reads are free; you pay gas and broadcast the batch yourself.
* **Paid relay** — `prepare-auth` → sign one EIP-712 token authorization → `actions/retire`. A Klima executor relays the retirement on-chain and pays the gas, reimbursed from your signed budget. No native ETH and no Base Account required.

## The endpoints

<!-- generated:endpoints -->
<!-- Do not edit by hand: npm run docs:render -->

| Action | How to call | Moves funds? | What it does |
| --- | --- | --- | --- |
| `discover` | `GET /api/discover` or `POST /api` | No | Lists carbon classes, credits, reference USDC/tonne prices, supported input tokens |
| `quote` | `GET /api/quote` or `POST /api` | No | Live on-chain price for a tonnage (retirement cost + protocol fee + slippage buffer) |
| `prepare/retire` | `GET /api/prepare/retire` or `POST /api` | No (you broadcast) | Unsigned `[approve, retire]` batch for self-submit |
| `prepare-auth` | `GET /api/prepare-auth` or `POST /api` | No | EIP-712 `typedData` + ready `actionsRetireRequest` for the relay path |
| `actions/retire` | `POST /api` | Yes (relayed) | Executor submits the retirement; requires signed `authPayload` |
| `certificate` | `GET /api/certificate` or `POST /api` | No | Resolves Carbonmark certificate URL(s) for a `txHash` |

<!-- /generated:endpoints -->

## How it works

The typical build-your-own flow is:

{% stepper %}

{% step %}
#### Discover what's retirable

Call `/discover` to list carbon classes, credits, reference prices, and supported input tokens.
{% endstep %}

{% step %}
#### Get a live quote

Call `/quote` for the true cost of your tonnage, including the on-chain fee and slippage-buffered maximum input.
{% endstep %}

{% step %}
#### Prepare the retirement

Call `/prepare/retire` to receive the unsigned `approve` + `retire` batch, then submit it atomically from your wallet.
{% endstep %}

{% step %}
#### Resolve the certificate

After the transaction confirms, call `/certificate` to get the public Carbonmark certificate URL.
{% endstep %}

{% endstepper %}

You do not need to pre-purchase or hold inventory to get started.

## Getting started

1. Review the Endpoint basics above and confirm you are targeting Base mainnet (`chainId=8453`).
2. Follow the build-your-own walkthrough to complete your first retirement, or use the gasless relay path.
3. To integrate with an AI agent, connect the Klima Base MCP plugin.

{% content-ref url="./retire-carbon-with-x402.md" %}
[Retire carbon with x402](./retire-carbon-with-x402.md)
{% endcontent-ref %}

{% content-ref url="./gasless-retirement-paid-relay.md" %}
[Gasless retirement (paid relay)](./gasless-retirement-paid-relay.md)
{% endcontent-ref %}

{% content-ref url="./use-x402-from-an-ai-agent.md" %}
[Use x402 from an AI agent](./use-x402-from-an-ai-agent.md)
{% endcontent-ref %}

{% content-ref url="./x402-reference.md" %}
[x402 reference](./x402-reference.md)
{% endcontent-ref %}

{% hint style="warning" %}
Retirement is irreversible. A confirmed retirement permanently burns the carbon credit — there is no undo, refund, or resale once the transaction confirms. Always review the quoted tonnes, price, and fee before approving.
{% endhint %}

## Resources

* [x402 protocol](https://www.x402.org/) — the HTTP 402 Payment Required protocol this endpoint implements
* [Base MCP](https://docs.base.org/ai-agents/quickstart) — wallet-connected AI agents on Base mainnet
* [Agent plugin + setup docs](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation) — Base MCP setup guide, klima-retire plugin, and full integration docs
* [Discovery manifest](https://x402.klimalabs.com/.well-known/x402.json) — the public `x402.json` that agent crawlers index automatically

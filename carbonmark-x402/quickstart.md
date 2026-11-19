---
description: "Retire 1 kg of carbon credits in a few minutes: two free calls with curl, then one script that signs a single USDC authorization on Base."
---

<!--
  DO NOT EDIT. Published automatically from Carbonmark/x402-klima-RA-new/api/docs/1b_quickstart.md.
  Changes made here will be overwritten by the next docs sync.
  Edit the source file and open a PR there instead.
-->

# Quickstart

Retire 1 kg of carbon credits over the x402 Carbon Retirement API. Step 1 needs nothing but `curl`. Step 2 is a complete script for the [gasless relay path](./gasless-retirement-paid-relay.md): it needs Node.js 18 or later and a wallet holding a few cents of USDC on Base mainnet. No ETH, no API key and no account.

## 1. See what is retirable and price it

Both calls are free and need no wallet. Each block holds one command, so it runs as copied.

List everything retirable: pooled credit classes and marketplace listings.

```bash
curl -s https://x402.klimalabs.com/api/discover
```

Get a live price for 1 kg (0.001 tonnes), paying in USDC on Base. The `carbonClass` value is a `carbonClassId` from the first response.

```bash
curl -s "https://x402.klimalabs.com/api/quote?chainId=8453&inputToken=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&amount=0.001&carbonClass=0x0008f35758a4318942ecb5d5414116ce7b1ede2d"
```

In Windows PowerShell 5, type `curl.exe` in place of `curl`.

The quote's `humanSummary` states the whole cost in one line. From a quote on 17 November 2026 (prices are live and will differ):

```
0.001 tonnes @ 0.000041 USDC + 0.01 USDC fee = 0.010041 USDC (max 0.010442 USDC with 4% slippage)
```

{% hint style="info" %}
`discover` names a class `carbonClassId`. Pass that value as the `carbonClass` parameter on `quote`, `prepare/retire` and `prepare-auth`.
{% endhint %}

## 2. Retire with one signature

Save this as `retire.mjs`, run `npm i viem`, then run it with a private key in `PRIVATE_KEY`. Set `DRY_RUN=1` to price the retirement and stop before anything is signed.

```js
// retire.mjs: retire 1 kg of carbon over Carbonmark's x402 endpoint (gasless relay).
// Setup:  npm i viem
// Run:    DRY_RUN=1 PRIVATE_KEY=0x... node retire.mjs   (prices it and stops; nothing is spent)
//         PRIVATE_KEY=0x... node retire.mjs             (retires for real; irreversible)
// Windows PowerShell sets variables differently:
//         $env:PRIVATE_KEY="0x..."; $env:DRY_RUN="1"; node retire.mjs   (dry run)
//         $env:DRY_RUN=$null; node retire.mjs                           (retires for real; irreversible)
import { privateKeyToAccount } from "viem/accounts";

const API = "https://x402.klimalabs.com/api";
const USDC = "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913"; // USDC on Base mainnet
const account = privateKeyToAccount(process.env.PRIVATE_KEY); // wallet holding USDC on Base; no ETH needed

const post = async (body) => {
  const res = await fetch(API, {
    method: "POST",
    headers: { "content-type": "application/json" },
    body: JSON.stringify(body),
  });
  const json = await res.json();
  if (!res.ok) throw new Error(`${json.error}: ${json.message}`);
  return json;
};

// 1. Discover (free): take the cheapest pooled credit class that retires from 1 kg.
const { carbonClasses } = await (await fetch(`${API}/discover`)).json();
const carbonClass = carbonClasses
  .filter((c) => c.minRetirementTonnesFormatted === "0.001")
  .sort((a, b) => Number(a.priceUsdcPerTonne) - Number(b.priceUsdcPerTonne))[0];

// 2. Prepare (free, moves no funds): live price plus the one authorization to sign.
const prep = await post({
  action: "prepare-auth",
  chainId: 8453,
  from: account.address,
  inputToken: USDC,
  carbonClass: carbonClass.carbonClassId,
  amount: "0.001", // tonnes, so this is 1 kg
  beneficiaryIsPayer: true,
  details: { beneficiaryString: "My AI agent", retirementMessage: "Retired over x402" },
});
console.log(`${carbonClass.name}: ${prep.quote.humanSummary}`);
console.log(`Authorizing up to ${prep.authValueFormatted} USDC (price + fee + relay gas)`);
if (process.env.DRY_RUN) process.exit(0);

// 3. Sign the USDC authorization and submit. From here it spends funds and cannot be undone.
const signature = await account.signTypedData(prep.typedData);
const request = prep.actionsRetireRequest;
const result = await post({ ...request, authPayload: { ...request.authPayload, signature } });

// 4. Certificate: public, permanent proof. Poll while the indexer catches up.
let retirements = result.retirements ?? [];
for (let i = 0; retirements.length === 0 && i < 10; i++) {
  await new Promise((r) => setTimeout(r, 3000));
  const res = await fetch(`${API}/certificate?txHash=${result.transactionHash}`);
  if (res.ok) retirements = (await res.json()).retirements ?? [];
}
console.log(retirements[0]?.certificateUrl ?? `Pending: tx ${result.transactionHash}`);
```

What the script does:

1. **Discover.** Picks the cheapest pooled class whose minimum is 1 kg. Some classes retire in whole tonnes only, and `discover` reports each class's minimum as `minRetirementTonnes`.
2. **Prepare.** `prepare-auth` re-prices the retirement and returns `typedData`, the EIP-712 object to sign. It moves no funds. `beneficiaryIsPayer: true` credits the paying wallet; a relayed retirement must name its beneficiary, or the call returns `400 attribution_required`.
3. **Sign and submit.** The only signature is a standard USDC authorization (EIP-3009). Posting `actionsRetireRequest` back with the signature has a Klima executor submit the retirement on-chain.
4. **Certificate.** The response carries the public Carbonmark certificate URL, or the script polls `/certificate` until the retirement is indexed.

{% hint style="warning" %}
Retirement is irreversible, and `beneficiaryString` and `retirementMessage` appear on the public certificate permanently. Run with `DRY_RUN=1` first.
{% endhint %}

## What it costs

A retirement costs the credit price plus a fee, and the relay adds a reimbursement for the executor's gas. For the 1 kg example above on 17 November 2026:

| Line | Amount |
| --- | --- |
| Credit price (0.001 t) | 0.000041 USDC |
| Fee | 0.01 USDC |
| Self-submit total (`quote.total`) | 0.010041 USDC |
| Executor gas on the relay path (`quote.relayExecutorGas`) | about 0.017 USDC |
| Authorization the script signs (`authValue`) | 0.027515 USDC |

`authValue` is the most the retirement can cost; it includes a slippage buffer. See [fees](./x402-reference.md#fees) for the fee schedule.

## Testing without spending

The endpoint runs on Base mainnet only; there is no testnet. `discover`, `quote` and `prepare-auth` are free and move no funds, so you can exercise the whole flow up to the signature at no cost. `DRY_RUN=1` in the script above does exactly that.

## Amounts under 1 kg

The smallest retirement is 1 kg, and every retirement carries a minimum fee. To account for smaller amounts, such as 50 g per transaction, record them in your own ledger and retire the running total each time it reaches 1 kg or more. One certificate then covers the whole batch.

## Next steps

* [Gasless retirement (paid relay)](./gasless-retirement-paid-relay.md): what you sign, the responses and their error codes.
* [Retire carbon with x402](./retire-carbon-with-x402.md): submit the transaction from your own wallet instead.
* [Use x402 from an AI agent](./use-x402-from-an-ai-agent.md): connect the endpoint to a wallet-connected agent.
* [TypeScript SDK](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/blob/main/sdk/x402-retire.ts): one `retire()` call that wraps the same four steps.

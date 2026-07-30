<!--
  DO NOT EDIT. Published automatically from Carbonmark/x402-klima-RA-new/api/docs/2_retire-carbon.md.
  Changes made here will be overwritten by the next docs sync.
  Edit the source file and open a PR there instead.
-->

# Retire carbon with x402

Use the x402 Endpoint to discover carbon, retrieve a live quote, submit an unsigned `approve` + `retire` batch from your own wallet, and resolve the public certificate.

This page walks through the **build-your-own** path: reads are free, and you sign and broadcast the retirement transaction yourself on Base mainnet. If you would rather have a relay submit the transaction and pay gas for you, see [Gasless retirement (paid relay)](./gasless-retirement-paid-relay.md).

All requests target Base mainnet (`chainId=8453`). Every action endpoint accepts **GET** with query parameters or **POST** JSON to `/api` with an `action` field; both return the same responses.

## Before you begin

You will need:

* A wallet on Base with a balance of an accepted input token (USDC or kVCM)
* Enough native ETH on Base to pay gas for the retirement transaction
* The ability to submit an atomic `approve` + `retire` batch (for example, via Base MCP `send_calls`)

See [x402 reference](./x402-reference.md) for input token addresses, amount rules, and fees.

## Step 1: Discover what's retirable

Call `/discover` to list carbon classes and credits. Each class returns a **reference USDC/tonne price**, the credits inside it (registry, vintage, token, available liquidity), supported input tokens, and contract addresses.

Filters are optional and AND-combined — for example, `maxUsdcPricePerTonne=20` returns only classes at or below $20/tonne. `chainId` is **not** accepted on this endpoint.

Example request:

```bash
# List carbon classes (optional filters)
curl "https://x402.klimalabs.com/api/discover?maxUsdcPricePerTonne=15"
```

The POST equivalent sends the same optional filters:

```json
{ "action": "discover", "maxUsdcPricePerTonne": 15 }
```

{% hint style="warning" %}
**Reference price is not the price at size.** `priceUsdcPerTonne` is the marginal (spot) price and is accurate near 1 tonne. Large orders walk up the AAM curve — for example, a credit quoted at roughly $107.82/t for 1 t can cost roughly $386.61/t for 100 t once it consumes a large share of pool liquidity. Always call `/quote` for the true cost of your size.
{% endhint %}

## Step 2: Get a live quote

Call `/quote` with your input token, carbon class, and amount to get the real cost of your tonnage.

**Required parameters:**

* `chainId` (`8453`)
* `inputToken`
* `carbonClass`
* `amount` (decimal tonne string)

**Optional parameters:** `creditToken`, `vintage`, `tokenId`. When you do not pin a specific credit, the API selects the most liquid credit in the class that can cover the requested `amount`.

Example request:

```bash
curl "https://x402.klimalabs.com/api/quote?chainId=8453\
&inputToken=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913\
&carbonClass=0xf4699531e0a5f6e9351a36de3753deaad329bf45&amount=1.5"
```

The response returns the retirement price, the on-chain `fee`, the `total` (price + fee), a `suggestedMaxInput` (total plus 4% slippage), a `humanSummary`, the `resolvedCredit` the server selected, and any `alternatives`.

Example response (trimmed for readability):

```json
{
  "tonnesFormatted": "1.5",
  "retirementPriceFormatted": "19.791336",
  "feeFormatted": "0.01",
  "totalFormatted": "19.801336",
  "suggestedMaxInputFormatted": "20.593389",
  "humanSummary": "1.5 tonnes @ 19.791336 USDC + 0.01 USDC fee = 19.801336 USDC (max 20.593389 USDC with 4% slippage)",
  "resolvedCredit": { "creditToken": "0xe662…71b8", "tokenId": 0, "vintage": 2021 }
}
```

## Step 3: Prepare the retirement

Call `/prepare/retire` with the same core parameters as `/quote`. The endpoint re-quotes on-chain and returns an **ordered batch**: an ERC-20 `approve` followed by the retirement, to be submitted atomically (for example, via Base MCP `send_calls`).

Optional parameters include `maxInputTokenIn` and `details` (a URL-encoded JSON object for certificate metadata; see below).

Example request:

```bash
curl "https://x402.klimalabs.com/api/prepare/retire?chainId=8453\
&inputToken=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913\
&carbonClass=0xf4699531e0a5f6e9351a36de3753deaad329bf45&amount=1.5\
&details=%7B%22beneficiaryString%22%3A%22Acme%20Corp%22%7D"
```

{% hint style="info" %}
The `to` field and `approvalInstructions.spender` in the response are both the Settlement Contract. Read them from the response rather than hard-coding an address.
{% endhint %}

### Certificate metadata (`details`)

`details` is an optional URL-encoded JSON object. The schema is strict — unknown keys return `400`.

| Field | Meaning |
| --- | --- |
| `retiringAddress` | Address performing the retirement |
| `beneficiaryAddress` | Address credited on the certificate. Defaults to the payer on this self-submit path; **required on the [relay path](./gasless-retirement-paid-relay.md)** unless `beneficiaryIsPayer: true` |
| `beneficiaryString` | Beneficiary display name — shows on the certificate |
| `retiringEntityString` | Retiring-entity display name |
| `retirementMessage` | Public message on the certificate |
| `beneficiaryLocation`, `consumptionCountryCode`, `consumptionPeriodStart`, `consumptionPeriodEnd` | Toucan Puro only — required for Puro credits |

{% hint style="warning" %}
**Set attribution up front.** `beneficiaryString` and `retirementMessage` are what make a certificate *named*, and the certificate **cannot be edited after the retirement confirms**. Note that the certificate's on-chain `retiringAddress` reflects an internal settlement/relayer address, not the `details.retiringAddress` you pass.
{% endhint %}

## Step 4: Submit the batch

Submit the returned `approve` + `retire` batch atomically from your wallet on Base. API calls are free; the protocol fee is settled on-chain inside this transaction, and the contract refunds any unused slippage budget in the same transaction.

## Step 5: Resolve the certificate

After the transaction confirms, call `/certificate` with the transaction hash to resolve the shareable Carbonmark certificate URL(s).

Use the optional `index` parameter to select one retirement out of a multi-retirement transaction; omit it to return all.

Example request:

```bash
curl "https://x402.klimalabs.com/api/certificate?txHash=0xYOUR_RETIRE_TX_HASH"
```

Example response (trimmed for readability):

```json
{
  "retirementCount": 1,
  "retirements": [{
    "certificateUrl": "https://app.carbonmark.com/retirements/id/8453-0x4a7f…f4bf-0",
    "amountInTonnes": "1",
    "beneficiaryName": "testing",
    "projectId": "UCR-423",
    "creditId": "UCR-423-2022"
  }]
}
```

{% hint style="info" %}
A `404 retirement_not_found` immediately after confirmation usually means the subgraph has not indexed the transaction yet. Wait a few seconds and retry.
{% endhint %}

## Notes

* Target Base mainnet only (`chainId=8453`).
* Reference prices from `/discover` approximate spot; always confirm real cost with `/quote`.
* When you do not pin a credit, the API picks the most liquid credit in the class that can cover your `amount`.
* Read the Settlement Contract address from the `/prepare/retire` response rather than hard-coding it.
* Set `beneficiaryString` and `retirementMessage` before retiring — certificates cannot be edited afterward.
* Retirement is irreversible once the transaction confirms.

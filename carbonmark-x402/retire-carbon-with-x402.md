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

Call `/discover` to see everything retirable. It returns two arrays, one per supply source, plus supported input tokens and contract addresses:

* `carbonClasses[]` — pooled protocol supply. Each class carries a **reference USDC/tonne price** and the credits inside it (registry, vintage, token, available liquidity).
* `marketplaceListings[]` — one entry per open marketplace listing: the seller, the exact credit, a firm `priceUsdcPerTonne`, and how much of it you can take (`leftToSell`, `minFill`, `expiration`). Sellers and asks are per listing, so two listings of the same credit are two entries, not one.

Every entry in either array is tagged with `source` (`"protocol"` or `"marketplace"`), and `marketplaceEnabled` tells you whether marketplace supply was searched at all — so an empty `marketplaceListings[]` means "nothing listed", not "turned off".

Filters are optional and AND-combined, and apply to **both** arrays: `source`, `carbonClass`, `creditToken`, `project`, `vintage`, `country`, `category`, `methodology`, `maxUsdcPricePerTonne`. For example, `maxUsdcPricePerTonne=20` returns only supply at or below $20/tonne from either source, and `source=marketplace` returns listings alone. A per-credit filter also trims a matched class down to its matching credits. `chainId` is **not** accepted on this endpoint.

Project metadata (country, category, methodologies) is joined per **credit**, not per class — a class spans projects, so its own attributes are an aggregate. Both arrays therefore carry the same attribute set, and every filter resolves against it.

Example request:

```bash
# List everything retirable (optional filters)
curl "https://x402.klimalabs.com/api/discover?maxUsdcPricePerTonne=15"

# Marketplace listings only
curl "https://x402.klimalabs.com/api/discover?source=marketplace"
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
* `amount` (decimal tonne string)
* exactly one supply source: `carbonClass` **or** `listingId`

**Optional parameters:** `creditToken`, `vintage`, `tokenId`. When you do not pin a specific credit, the API selects the most liquid credit in the class that can cover the requested `amount`.

### Two supply sources

`discover` returns both, each entry tagged with `source`. They are addressed differently and the choice belongs in the request:

| | `carbonClass` (`source: "protocol"`) | `listingId` (`source: "marketplace"`) |
| --- | --- | --- |
| What it is | Pooled supply, priced by the protocol AMM | One seller's fixed ask for one credit |
| Which credit | The API picks the most liquid in the class, or you pin one | Fixed by the listing |
| Price | Moves with the pool; quotes carry a slippage buffer | `unitPrice` exactly, or the fill reverts |
| `suggestedMaxInput` | `total` plus 4% slippage | `total` exactly, no buffer |
| Input token | USDC or kVCM | USDC only |
| Amount limits | The class's liquidity | The listing's `minFill` and `leftToSell` |
| Quote lifetime | Authorizations valid up to an hour | Authorizations capped at 5 minutes |

Pass one or the other, never both: a listing already names its credit, so there is no class to route through. Sending both, or neither, returns `400 schema_validation`.

A listing fill can fail in ways pooled supply cannot, because the seller controls the terms and other buyers compete for the same supply: `listing_not_found`, `listing_expired`, `below_min_fill`, `insufficient_listing_supply`. Each is documented in the error reference with the field to adjust.

Example request:

```bash
curl "https://x402.klimalabs.com/api/quote?chainId=8453\
&inputToken=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913\
&carbonClass=0xf4699531e0a5f6e9351a36de3753deaad329bf45&amount=1.5"
```

The response returns the retirement price, the on-chain `fee`, the `total` (price + fee), a `suggestedMaxInput` (total plus 4% slippage on the protocol path; exactly `total` for a listing, which has no price impact to buffer against), a `humanSummary`, the `resolvedCredit` the server selected, and any `alternatives`. A listing quote returns an empty `alternatives`: it is one seller's ask, so there is nothing to offer instead.

To quote a marketplace listing, swap `carbonClass` for a `listingId` from `discover.marketplaceListings[]`:

```bash
curl "https://x402.klimalabs.com/api/quote?chainId=8453\
&inputToken=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913\
&listingId=0x1f0c...&amount=1.5"
```

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

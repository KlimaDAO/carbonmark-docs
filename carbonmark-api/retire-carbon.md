---
description: >-
  Use the Carbonmark API to create retirement quotes, submit retirement orders,
  and confirm completion with a retirement receipt.
---

# Retire Carbon

This page shows the basic retirement workflow. For full parameter definitions and response schemas, see the [API reference](https://api.carbonmark.com/) for each endpoint.

{% hint style="info" %}
For details on retirement receipts and certificate provisioning from Carbonmark and a credit's underlying registry, see [Retirement Certificate Support](../registry-integrations/retirement-certificate-support.md).&#x20;
{% endhint %}

In the examples below, we use the versioned API base URL `https://v20.api.carbonmark.com`. Omitting the version prefix exposes your integration to breaking changes. Use the [latest stable version](versioning-and-release-process/) when building your integration.

## Before you begin

You will need:

* a Carbonmark account
* a sandbox or production API key
* a project or product with a visible price
* an `asset_price_source_id` from the `/prices` endpoint

Create a sandbox key in the [Developer Dashboard](https://developers.carbonmark.com/) for testing. For production keys, see [Billing and production access](billing-and-production-access.md). Keep your API key secure and do not expose it in client-side code or commit it to your repository.

## Step 1: Create an API key

Create an account or sign in to the Developer Dashboard.

Once signed in, go to the **Keys** page and generate an API key.

* Sandbox keys are free and can be used for testing
* Production keys are available once you set up billing. With prepaid billing, you create them yourself after completing business setup and adding funds. With postpaid billing, they are available after Carbonmark approves your account.
* Keys are shown only once, so copy and store them securely

## Step 2: Choose a project to retire

Use the Carbonmark marketplace or the API to find a project with a visible listing price.

For project-based retirements, use [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) and note the project `key`.

In this example, we use project `VCS-1146`.

## Step 3: Retrieve pricing and identify an asset price source

Before creating a quote, call `/prices` to retrieve seller listing price sources for the project you want to retire. The `assetPriceType` filter lets you narrow results to projects with specific pricing sources. Accepted values are `listing` (seller listings on Carbonmark) and `klimaprotocol` (Klima protocol pool prices).

Use the `sourceId` value from the `/prices` response as the `asset_price_source_id` in the quote request. The number after `listing-` is the network's chain ID: `8453` for Base and `137` for Polygon.

Example request:

```bash
curl -G https://v20.api.carbonmark.com/prices \
  --data-urlencode "projectIds=VCS-1146" \
  --header "Accept: application/json"
```

Example response (trimmed for readability):

```json
[
  {
    "sourceId": "listing-8453-0xea39b91a16003213b195a75476ea8277df1754309074437c9be72ae714ed1100",
    "type": "listing",
    "purchasePrice": 1.022,
    "baseUnitPrice": 0.73,
    "supply": 298144,
    "liquidSupply": 298144,
    "minFillAmount": 0.001,
    "settlementToken": "usdc",
    "listing": {
      "id": "0xea39b91a16003213b195a75476ea8277df1754309074437c9be72ae714ed1100",
      "creditId": {
        "vintage": 2015,
        "projectId": "VCS-1146"
      },
      "token": {
        "id": "8453-erc20-0x273e40e09a2b81c9ec8ac0cbd2a1b62354e397fe",
        "address": "0x273e40e09a2b81c9ec8ac0cbd2a1b62354e397fe",
        "decimals": 18,
        "tokenStandard": "erc20",
        "name": "Toucan Protocol : TCO2-VCS-1146-2015",
        "isExAnte": false,
        "symbol": "TCO2-VCS-1146-2015",
        "tokenId": 0
      },
      "sellerId": "0x2ccb9bd4b8810b277d3b29a05794e2cf66a769ac",
      "expiresAfter": 1946209710
    }
  }
]
```

For this example flow, we use this Base listing because it has enough supply to support a 1 tonne retirement.

## Step 4: Create a retirement quote

Create a quote using the [`/quotes`](https://api.carbonmark.com/#/paths/quotes/post) endpoint.

Required fields:

* `asset_price_source_id`
* `quantity_tonnes`

Example request:

```bash
curl --request POST \
  --url https://v20.api.carbonmark.com/quotes \
  --header "Accept: application/json" \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "asset_price_source_id": "listing-8453-0xea39b91a16003213b195a75476ea8277df1754309074437c9be72ae714ed1100",
    "quantity_tonnes": 1
  }'
```

Example response (trimmed for readability):

```json
{
  "asset_price_source_id": "listing-8453-0xea39b91a16003213b195a75476ea8277df1754309074437c9be72ae714ed1100",
  "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
  "quantity_tonnes": 1,
  "cost_usdc": 1.022,
  "consumed": 0
}
```

Save the `uuid` from the quote response. You will need it to create the order.

## Step 5: Create a retirement order

Create an order using the [`/orders`](https://api.carbonmark.com/#/paths/orders/post) endpoint.

Required fields:

* `quote_uuid`
* `beneficiary_name`
* `retirement_message`

Some registries may require additional fields. For example, certain Puro.earth retirements require `consumption_metadata`.

We recommend sending an `Idempotency-Key` header with every order. For production keys on prepaid accounts, retrying a request with the same key returns the original order instead of creating a second one. See [Retrying orders safely](errors.md#retrying-orders-safely).

Example request:

```bash
curl --request POST \
  --url https://v20.api.carbonmark.com/orders \
  --header "Accept: application/json" \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --header "Idempotency-Key: order-0001" \
  --data '{
    "quote_uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
    "beneficiary_name": "Mother Nature",
    "beneficiary_address": "0x6fb0e62afB345951997bd5149a313df9fcB72C2d",
    "retirement_message": "Retirement of 1 tonne from VCS-1146"
  }'
```

Example response (trimmed for readability):

```json
{
  "status": "SUBMITTED",
  "transaction_hash": null,
  "on_chain_explorer_url": null,
  "view_retirement_url": null,
  "quote": {
    "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
    "quantity_tonnes": 1,
    "cost_usdc": 1.022
  },
  "consumption_metadata": {},
  "registry_specific_data": {}
}
```

A successful order response with `status: "SUBMITTED"` means the retirement request has been accepted and is being processed.

On a prepaid account, a production order reserves its `cost_usdc` from your available balance when it is accepted. If your balance or a spend limit does not cover the order, you get an error instead of an order. See [Errors](errors.md).

## Step 6: Confirm the retirement is complete

To confirm completion, query `/orders` using the `quote_uuid`.

Example request:

```bash
curl -G https://v20.api.carbonmark.com/orders \
  --data-urlencode "quote_uuid=07495f6c-8b02-465d-8fdd-4bcc2d2047e9" \
  --header "Accept: application/json" \
  --header "Authorization: Bearer YOUR_API_KEY"
```

Example response (trimmed for readability)

```json
{
  "status": "COMPLETED",
  "transaction_hash": "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
  "on_chain_explorer_url": "https://basescan.org/tx/0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
  "view_retirement_url": "https://app.carbonmark.com/retirements/0x6fb0e62afB345951997bd5149a313df9fcB72C2d/33",
  "quote": {
    "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
    "quantity_tonnes": 1,
    "cost_usdc": 1.022
  }
}
```

When the order status is `COMPLETED`, the carbon credit has been retired. You can also use `on_chain_explorer_url` or `view_retirement_url` from the order response for additional confirmation.

If the status is `FAILED`, the retirement did not happen. On a prepaid account, the reserved amount is released back to your available balance.

## Retirement receipt

Once the order is complete, use the `view_retirement_url` returned by the API to access the retirement receipt.

The receipt URL follows this pattern:

{% code overflow="wrap" %}
```
https://app.carbonmark.com/retirements/{beneficiaryAddress}/{number}
```
{% endcode %}

Example:

{% code overflow="wrap" %}
```
https://app.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33
```
{% endcode %}

## Billing

Completed production orders appear in the Developer Dashboard. How you pay for them depends on your billing option:

* **Prepaid:** the order's cost is charged to your balance when the retirement completes.
* **Postpaid:** the order's cost is included in your monthly invoice.

Sandbox orders are never billed. See [Billing and production access](billing-and-production-access.md).

## Notes

* Always use a versioned API base URL in production integrations
* Use `/prices` to resolve a valid listing `asset_price_source_id` before creating a quote
* Use `/quotes` to price the retirement
* Use `/orders` to submit the retirement, with an `Idempotency-Key` header
* Poll `/orders` with the `quote_uuid` until the order status is `COMPLETED` or `FAILED`
* Some registries may require additional order metadata
* Handle errors by their `code`. See [Errors](errors.md)

---
description: >-
  Use the Carbonmark API to create retirement quotes, submit retirement orders,
  and confirm completion with a retirement receipt.
---

# Retire Carbon

Use the Carbonmark API to create retirement quotes, submit retirement orders, and confirm completion with a retirement receipt.

This page shows the basic retirement workflow. For full parameter definitions and response schemas, see the [API reference](https://api.carbonmark.com/) for each endpoint.&#x20;

In the examples below, we use a versioned API base URL such as `https://v1.api.carbonmark.com`. Omitting the version prefix exposes your integration to breaking changes. Use the [latest stable version](versioning-and-release-process/) when building your integration.

## Before you begin

You will need:

* a Carbonmark account
* a sandbox or production API key
* a project or product with a visible price
* an `asset_price_source_id` from the `/prices` endpoint

Create a sandbox key in the [Developer Dashboard](https://developers.carbonmark.com/) for testing. Production access requires onboarding. Keep your API key secure and do not expose it in client-side code or commit it to your repository.

## Step 1: Create an API key

Create an account or sign in to the Developer Dashboard.

Once signed in, go to the **Keys** page and generate an API key.

* Sandbox keys are free and can be used for testing
* Production keys require onboarding
* Keys are shown only once, so copy and store them securely

## Step 2: Choose a project to retire

Use the Carbonmark marketplace or the API to find a project with a visible listing price.

For project-based retirements, use [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) and note the project `key`.

In this example, we use project `ICR-112`.

## Step 3: Retrieve pricing and identify an asset price source

Before creating a quote, call `/prices` to retrieve seller listing price sources for the project you want to retire.

Use the `sourceId` value from the `/prices` response as the `asset_price_source_id` in the quote request.

Example request:

```bash
curl -G https://v1.api.carbonmark.com/prices \
  --data-urlencode "projectIds=ICR-112" \
  --header "Accept: application/json"
```

Example response (trimmed for readability):

```bash
[
  {
    "sourceId": "listing-137-0xe5a7178a0c44cac6f5a00b429b3de99016194b1494e75f4ef849a81faad41504",
    "type": "listing",
    "purchasePrice": 3.08,
    "baseUnitPrice": 2.2,
    "supply": 0.872,
    "liquidSupply": 0.872,
    "minFillAmount": 0.001,
    "listing": {
      "id": "0xe5a7178a0c44cac6f5a00b429b3de99016194b1494e75f4ef849a81faad41504",
      "creditId": {
        "vintage": 2019,
        "projectId": "ICR-112"
      },
      "token": {
        "id": "137-erc1155-0xd016b2acece65612b93cc9aee763bda0c2b0e4c0-1",
        "address": "0xd016b2acece65612b93cc9aee763bda0c2b0e4c0",
        "decimals": 18,
        "tokenStandard": "erc1155",
        "name": "ICR-112-2019",
        "isExAnte": false,
        "symbol": "ICR-112-2019",
        "tokenId": 1
      },
      "sellerId": "0x6e069bd92d6d52ee6ed79753274840469dd8ec62",
      "expiresAfter": 1894088958
    }
  },
  {
    "sourceId": "listing-137-0x79a431e9ea34117e57e2cc4cc483f6954ac295e8de540faa320dc8697f4a5a8b",
    "type": "listing",
    "purchasePrice": 3.78,
    "baseUnitPrice": 2.7,
    "supply": 19.361,
    "liquidSupply": 19.361,
    "minFillAmount": 0.001,
    "listing": {
      "id": "0x79a431e9ea34117e57e2cc4cc483f6954ac295e8de540faa320dc8697f4a5a8b",
      "creditId": {
        "vintage": 2019,
        "projectId": "ICR-112"
      },
      "token": {
        "id": "137-erc1155-0xd016b2acece65612b93cc9aee763bda0c2b0e4c0-1",
        "address": "0xd016b2acece65612b93cc9aee763bda0c2b0e4c0",
        "decimals": 18,
        "tokenStandard": "erc1155",
        "name": "ICR-112-2019",
        "isExAnte": false,
        "symbol": "ICR-112-2019",
        "tokenId": 1
      },
      "sellerId": "0x6e069bd92d6d52ee6ed79753274840469dd8ec62",
      "expiresAfter": 1897738887
    }
  }
]
```

For this example flow, we use the second listing because it has enough supply to support a 1 tonne retirement.

## Step 4: Create a retirement quote

Create a quote using the [`/quotes`](https://api.carbonmark.com/#/paths/quotes/post) endpoint.

Required fields:

* `asset_price_source_id`
* `quantity_tonnes`

Example request:

```bash
curl --request POST \
  --url https://v1.api.carbonmark.com/quotes \
  --header "Accept: application/json" \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "asset_price_source_id": "listing-137-0x79a431e9ea34117e57e2cc4cc483f6954ac295e8de540faa320dc8697f4a5a8b",
    "quantity_tonnes": 1
  }'
```

Example response (trimmed for readability):

```bash
{
  "asset_price_source_id": "listing-137-0x79a431e9ea34117e57e2cc4cc483f6954ac295e8de540faa320dc8697f4a5a8b",
  "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
  "quantity_tonnes": 1,
  "cost_usdc": 3.78,
  "consumed": 0
}
```

Save the `uuid` from the quote response. You will need it to create the order.

## Step 5: Create a retirement order

Create an order using the [`/orders`](https://api.carbonmark.com/#/paths/orders/get) endpoint.

Required fields:

* `quote_uuid`
* `beneficiary_name`
* `retirement_message`

Some registries may require additional fields. For example, certain Puro.earth retirements require `consumption_metadata`.

Example request:

```bash
curl --request POST \
  --url https://v1.api.carbonmark.com/orders \
  --header "Accept: application/json" \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "quote_uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
    "beneficiary_name": "Mother Nature",
    "beneficiary_address": "0x6fb0e62afB345951997bd5149a313df9fcB72C2d",
    "retirement_message": "Retirement of 1 tonne from ICR-112"
  }'
```

Example response (trimmed for readability):

```bash
{
  "status": "SUBMITTED",
  "transaction_hash": null,
  "polygonscan_url": null,
  "view_retirement_url": null,
  "quote": {
    "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
    "quantity_tonnes": 1,
    "cost_usdc": 3.78
  },
  "consumption_metadata": {},
  "registry_specific_data": {}
}
```

A successful order response with `status: "SUBMITTED"` means the retirement request has been accepted and is being processed.

## Step 6: Confirm the retirement is complete

To confirm completion, query `/orders` using the `quote_uuid`.

Example request:

```bash
curl -G https://v1.api.carbonmark.com/orders \
  --data-urlencode "quote_uuid=07495f6c-8b02-465d-8fdd-4bcc2d2047e9" \
  --header "Accept: application/json" \
  --header "Authorization: Bearer YOUR_API_KEY"
```

Example response (trimmed for readability)

```bash
{
  "status": "COMPLETED",
  "transaction_hash": "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
  "polygonscan_url": "https://polygonscan.com/tx/0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
  "view_retirement_url": "https://app.carbonmark.com/retirements/0x6fb0e62afB345951997bd5149a313df9fcB72C2d/33",
  "quote": {
    "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
    "quantity_tonnes": 1,
    "cost_usdc": 3.78
  }
}
```

When the order status is `COMPLETED`, the carbon credit has been retired. You can also use `polygonscan_url` or `view_retirement_url` from the order response for additional confirmation.

## Retirement receipt

Once the order is complete, use the `view_retirement_url` returned by the API to access the retirement receipt.

The receipt URL follows this pattern:

```
https://app.carbonmark.com/retirements/{beneficiaryAddress}/{number}
```

Example:

```
https://app.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33
```

The completed order will also appear in the Developer Dashboard, and the cost will be included in your monthly invoice.

## Notes

* Always use a versioned API base URL in production integrations
* Use `/prices` to resolve a valid listing `asset_price_source_id` before creating a quote
* Use `/quotes` to price the retirement
* Use `/orders` to submit the retirement
* Poll `/orders` with the `quote_uuid` until the order status is `COMPLETED`
* Some registries may require additional order metadata

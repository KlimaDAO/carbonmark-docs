---
description: Initiate carbon retirements via a REST endpoint
---

# Retire Carbon

{% hint style="warning" %}
**Note: This is a feature preview and the Retirement API is currently in beta.** The endpoints and parameters are in active development and are likely to change.
{% endhint %}

Carbonmark's Retirement API offers developers a path to initiate carbon retirements via a REST endpoint. Follow the guide below to get started:

{% hint style="success" %}
In this guide, you will learn to:

* Get access to the Carbonmark developer dashboard and get your API Key
* Find a project or index product to retire
* Identify the listing details
* Create a quote
* Create an order
* Confirm carbon credit retirement
{% endhint %}

{% hint style="info" %}
**What is the difference between retiring carbon from a seller listing and retiring carbon from a pool?**

Listings can be from an individual or organization seller (i.e. Seller Listing) or from a Carbon Pool. A project may therefore have multiple listings at different price points.

Seller listing prices are controlled by the seller. Carbon Pool pricing is dynamic and can incur slippage.
{% endhint %}

## Retire Carbon Steps

### 1. Get authenticated

[**Request access to the developer dashboard to generate your API Key before proceeding.**](https://share-eu1.hsforms.com/1\_VneTUObQZmJm4kNcRuEoQg3axk)

Once the API services agreement is signed and access is confirmed by email from our onboarding team, you can create API keys from our [`Developer Dashboard`](https://developers.carbonmark.com/auth). Click [`Don't have an account? Sign up`](https://developers.carbonmark.com/auth#auth-sign-in) to create an account using the whitelisted email address provided.

{% hint style="danger" %}
The API Key is sensitive and can be used to create costly retirements on your behalf. \
**Be careful not to expose the API Key or commit it to your repository.**\
\
Note that you can create a `sandbox key` for testing purposes.\
\
Keys are generated once and not exposed in the Developer Dashboard so you must copy them to be utilized.
{% endhint %}

### 2. Find a project to retire

Use [`carbonmark.com`](http://carbonmark.com) or [`api.carbonmark.com/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) to identify a project you’d like to retire. The `/carbonProjects` endpoints allows you to retrieve an array of carbon projects filtered by desired query parameters and returns project metadata and the best price. The Retirement API is compatible with any credit that has a visible price. Make note of the "`key/projectID`" (for example, **VCS-191**) of the project you are interested in.

You can also check out our various guides to discovering carbon projects via our REST API.

{% content-ref url="../explore-carbon-projects/" %}
[explore-carbon-projects](../explore-carbon-projects/)
{% endcontent-ref %}

### 3. Identify the listing details

Listing details for project(s) can be returned for an array of project key / project ID and other query parameters. For example, `api.carbonmark.com/prices?projectsIds={project key}&projectsIds={project key}`. See [`/prices`](https://api.carbonmark.com/#/paths/prices/get) endpoint for details.\
\
Note that a [`/prices`](https://api.carbonmark.com/#/paths/prices/get) response may include type "**listing**" (i.e. seller listing) or "**carbon\_pool**" or "**product**" depending on the query parameters used.

{% code overflow="wrap" %}
```typescript
// api.carbonmark.com/prices?projectIds=VCS-191&projectIds=VCS-1100
// sample JSON response

{
    "sourceId": "listing-137-0xa6dbf88e90c12b3bf8e6fb39d08b56ceaca14b7f26648d83d4e98e12b94b9f8a",
    "type": "listing",
    "purchasePrice": 0.9945,
    "baseUnitPrice": 0.765,
    "supply": 16.995,
    "minFillAmount": 0.001,
    "listing": {
      "id": "0xa6dbf88e90c12b3bf8e6fb39d08b56ceaca14b7f26648d83d4e98e12b94b9f8a",
      "creditId": {
        "vintage": 2008,
        "projectId": "VCS-191"
      },
      "token": {
        "id": "0xb139c4cc9d20a3618e9a2268d73eff18c496b991",
        "address": "0xb139c4cc9d20a3618e9a2268d73eff18c496b991",
        "name": "Toucan Protocol : TCO2-VCS-191-2008",
        "isExAnte": false,
        "symbol": "TCO2-VCS-191-2008",
        "tokenId": 0
      },
      "sellerId": "0xcb6222f4df04385ea08e8da2a5871131ff5f6cba"
    }
  },
  {
    "sourceId": "carbon_pool-137-0x9e9b3a0208e19c44096fdaf99c09fd4991e52e73-0-bct",
    "type": "carbon_pool",
    "purchasePrice": 0.9536202,
    "baseUnitPrice": 0.733554,
    "supply": 230432.746766008,
    "minFillAmount": 0.001,
    "carbonPool": {
      "creditId": {
        "vintage": 2012,
        "projectId": "VCS-1100"
      },
      "token": {
        "id": "0x9e9b3a0208e19c44096fdaf99c09fd4991e52e73",
        "address": "0x9e9b3a0208e19c44096fdaf99c09fd4991e52e73",
        "name": "Toucan Protocol : TCO2-VCS-1100-2012",
        "isExAnte": false,
        "symbol": "TCO2-VCS-1100-2012",
        "tokenId": null
      },
      "isDefaultPoolCredit": false,
      "poolName": "bct"
    }
  }
```
{% endcode %}

### 4. Create a retirement quote

The next step is to create a "**Quote**" for the listing you are interested in using the [`/quotes`](https://api.carbonmark.com/#/paths/quotes/post) endpoint . &#x20;

A header is required to provide the `Authorization` bearer token. The required parameters for a quote are `asset_price_source_id` and `quantity_tonnes`.

{% code overflow="wrap" %}
```typescript
const url = 'https://api.carbonmark.com/quotes';
const data = {
  asset_price_source_id: 'carbon_pool-137-0xccacc6099debd9654c6814fcb800431ef7549b10-0-bct',
  quantity_tonnes: 1
};

const headers = {
  'Accept': 'application/json',
  'Authorization': 'Bearer cm_api_d286bc5a-9980-43b7-9507-7fc013fecca8',
  'Content-Type': 'application/json'
};

fetch(url, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(data)
})
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    return response.json();
  })
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('There was a problem with the fetch operation:', error);
  });
```
{% endcode %}

This is a response example, note the `uuid` which will be required to generate an order.

{% code overflow="wrap" %}
```json
{
  "asset_price_source_id": "carbon_pool-137-0xb139c4cc9d20a3618e9a2268d73eff18c496b991-0-bct",
  "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
  "created_at": "2024-03-25T19:28:41.73536+00:00",
  "updated_at": "2024-03-25T19:28:41.73536+00:00",
  "credential_id": 93,
  "quantity_tonnes": 1,
  "cost_usdc": 0.434118,
  "consumed": 0
}
```
{% endcode %}

### 5. Create a retirement order

The next step is to create an "**Order**" for the quote generated above using the [`/orders`](https://api.carbonmark.com/#/paths/orders/post) endpoint .&#x20;

A header is required to provide the `Authorization` bearer token. The required parameters for an order request are `quote_uuid` (from the /quote response),  `beneficiary_name`, and `retirement message`.

Note that some registries have specific requirements. For example, creating an order for a [Puro.earth](https://puro.earth/) registry credit requires `consumption_metadata` to be submitted in the request.

```typescript
const url = 'https://api.carbonmark.com/orders';
const data = {
  quote_uuid: 'e632eafd-9a67-4af1-8acc-16d72f5a738f',
  beneficiary_name: 'Mother Nature',
  beneficiary_address: '0x6fb0e62afB345951997bd5149a313df9fcB72C2d',
  retirement_message: 'This is a retirement message',
  consumption_metadata: {
    country_of_consumption_code: 'string',
    consumption_period_start: 0,
    consumption_period_end: 0
  }
};

const headers = {
  'Accept': 'application/json',
  'Authorization': 'Bearer cm_api_d286bc5a-9980-43b7-9507-7fc013fecca8',
  'Content-Type': 'application/json'
};

fetch(url, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(data)
})
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    return response.json();
  })
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('There was a problem with the fetch operation:', error);
  });

```

This is a response example.

```json
{
  "created_at": "2024-03-25T22:01:23.236141+00:00",
  "updated_at": "2024-03-25T22:01:23.236141+00:00",
  "status": "SUBMITTED",
  "transaction_hash": null,
  "polygonscan_url": null,
  "view_retirement_url": null,
  "quote": {
    "asset_price_source_id": "listing-137-0x12dbdff6fa065abc002324b5f2c1546b06e0440b1906438669a492e1987690f4",
    "uuid": "07495f6c-8b02-465d-8fdd-4bcc2d2047e9",
    "created_at": "2024-03-25T22:01:23.236141+00:00",
    "updated_at": "2024-03-25T22:01:23.236141+00:00",
    "credential_id": 93,
    "quantity_tonnes": 1,
    "cost_usdc": 0.5,
    "consumed": 0
  },
  "consumption_metadata": {},
  "registry_specific_data": {}
}
```

You should receive a response containing your order details including order `status` of your order which will allow you to confirm your order has been `SUBMITTED` and will be completed shortly.

{% hint style="info" %}
The actual retirement transaction will be initiated instantly by our system, and broadcast to a blockchain network so the underlying credit is permanently retired and taken out of circulation.

It typically takes between 0.5 and 3 seconds for the retirement to be complete — this includes order creation, transaction broadcast, confirmation on the blockchain, and final retirement certificate generation.
{% endhint %}

### 5. Confirm your order is complete and view your retirement receipt

To verify your order has been completed, send a request to `https://api.carbonmark.com/orders?quote_uuid=uuid`.&#x20;

Once your order `status` is `COMPLETED`, your order is complete and the carbon credit has been retired.

You can follow the `polygonscan_url` or `view_retirement_url` URLs in the response for further confirmation that your retirement order transaction is complete.\
\
Alternatively, you can navigate back to the [Carbonmark Developer Dashboard](https://developers.carbonmark.com/dashboard/usage) and see the entry and the order status.

#### View retirement receipt

When the transaction is finalized and our system has marked the order status as “complete”, the provided URL should take you to the shareable retirement receipt.

`carbonmark.com/retirements/{beneficiaryAddress}/{number}`

For example: [https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33](https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33)

The order will appear in your developer dashboard as well.

The cost of the order will be added to your monthly invoice.

## Want to learn more?

One of our solution specialists would be happy to answer any questions you have.

* [Book an API demo](https://meetings-eu1.hubspot.com/meetings/liamellul/api-demo)
* [Contact us](https://share-eu1.hsforms.com/1\_VneTUObQZmJm4kNcRuEoQg3axk)

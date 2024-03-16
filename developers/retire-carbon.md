---
description: Initiate carbon retirements via a REST endpoint
---

# Retire Carbon

{% hint style="warning" %}
**Note: This is a feature preview and the Retirement API is currently in alpha.** These endpoints and parameters are in active development and are likely to change.
{% endhint %}

Carbonmark's Retirement API offers developers a path to initiate carbon retirements via a REST endpoint. Follow the guide below to get started:

{% hint style="success" %}
In this guide you will learn to:

* Get access to the Carbonmark developer dashboard and get your API Secret
* Find a project to retire
* Identify the listing details
* Create an order
* Confirm carbon credit retirement
{% endhint %}

{% hint style="info" %}
**What is the difference between retiring carbon from a seller listing and retiring carbon from a pool?**

Listings can be from an individual or organization seller (i.e. Seller Listing) or from a Carbon Pool. A project may therefore have multiple listings at different price points.

Seller listing prices are controlled by the seller. Carbon Pool pricing is dynamic and can incur slippage.
{% endhint %}

## Retire Carbon from a Seller Listing

### 1. Get authenticated

[**Request access to the developer dashboard to generate your API Secret before proceeding.**](https://share-eu1.hsforms.com/1\_VneTUObQZmJm4kNcRuEoQg3axk)

{% hint style="danger" %}
The API Secret is sensitive and can be used to create costly retirements on your behalf. \
**Be careful not to expose the API Secret or commit it to your repository.**
{% endhint %}

### 2. Find a project to retire

Use [carbonmark.com](http://carbonmark.com) or [`api.carbonmark.com/projects`](https://api.carbonmark.com/#/paths/projects/get) to identify a credit and price you’d like to retire. The Retirement API is compatible with any credit that has a visible price.

You can also check out our various guides to discovering carbon projects via our REST API.

{% content-ref url="explore-carbon-projects/" %}
[explore-carbon-projects](explore-carbon-projects/)
{% endcontent-ref %}

### 3. Identify the listing details

Retrieve the `listing_id` for direct seller listings from `api.carbonmark.com/projects/{credit_id}`

```typescript
// api.carbonmark.com/projects/VCS-1764-2020
// sample JSON response
{
  "name": "Reforestation And Restoration Of Degraded Mangrove Lands, Sustainable Livelihood And Community Development In Myanmar",
  "country": "Myanmar",
  "prices": [],
  "creditTokenAddress": "0xfbc5d9620b6bb729be2f72fca157054bd3da3d1a",
	/*
		 VCS-1764-2020 has a single seller listing
	   for $30/tonne, with 995 tonnes remaining.
	*/
  "listings": [
    {
      "id": "0x0fe3b2c9c9021d6ba72d7071efaad5f080ae48d447d55c87482d9b736790562a",
      "totalAmountToSell": "1000.0",
      "leftToSell": "995.0",
      "tokenAddress": "0xfbc5d9620b6bb729be2f72fca157054bd3da3d1a",
      "singleUnitPrice": "30.0",
      "minFillAmount": "1.0"
    }
  ],
  "hasSupply": true
}
```

### 4. Create an Order

The actual retirement transaction will be initiated instantly by our system, and broadcast to a blockchain network so the underlying credit is permanently retired and taken out of circulation.

It typically takes between 0.5 and 3 seconds for the retirement to be complete — this includes Order creation, transaction broadcast and confirmation on the blockchain, and final retirement certificate generation.

**Since we are creating an order for a seller listing, we will need to include the required parameters:** `listing_id` and `quantity_tonnes`.

```typescript
const response = await fetch("https://api.carbonmark.com/orders", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${process.env.CARBONMARK_API_SECRET}`,
  },
  body: JSON.stringify({
    listing_id:
      "0x0fe3b2c9c9021d6ba72d7071efaad5f080ae48d447d55c87482d9b736790562a",
    quantity_tonnes: 1.5,
  }),
});

const { id, url, status, transaction } = await response.json();
```

### View retirement receipt

When the transaction is finalized and our system has marked the order status as “complete”, the provided URL should take you to the shareable retirement receipt.

`carbonmark.com/retirements/{beneficiaryAddress}/{number}`

For example: [https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33](https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33)

The order will appear in your developer dashboard as well.

The cost of the order will be added to your monthly invoice.

## Retire Carbon from a Pool

### 1. Get authenticated

[**Request access to the developer dashboard to generate your API Secret before proceeding.**](https://share-eu1.hsforms.com/1\_VneTUObQZmJm4kNcRuEoQg3axk)

{% hint style="danger" %}
The API Secret is sensitive and can be used to create costly retirements on your behalf. \
**Be careful not to expose the API Secret or commit it to your repository.**
{% endhint %}

### 2. Find a project to retire

Use [carbonmark.com](http://carbonmark.com) or [`api.carbonmark.com/projects`](https://api.carbonmark.com/#/paths/projects/get) to identify a credit and price you’d like to retire. The Retirement API is compatible with any credit that has a visible price.



You can also check out our various guides to discovering carbon projects via our REST API.

{% content-ref url="explore-carbon-projects/" %}
[explore-carbon-projects](explore-carbon-projects/)
{% endcontent-ref %}

### 3. Identify the listing details

Retrieve the `pool` and `credit_token_address` for projects in liquidity pools from `api.carbonmark.com/projects/{credit_id}`

```typescript
// api.carbonmark.com/projects/VCS-934-2012
// sample JSON response
{
  "name": "The Mai Ndombe REDD+ Project",
  "country": "Congo, The Democratic Republic of the",
  	/*
	   VCS-934-2012 is available via Carbon Pool
	   for $1.11382/tonne, with ~9142 tonnes remaining
           at the time of writing this
           (Note: carbon pool pricing is dynamic and can incur slippage).
	*/
  "prices": [
      {
        "poolName": "nct",
        "supply": "9142.994743138417",
        "poolAddress": "0xd838290e877e0188a4a44700463419ed96c16107",
        "isPoolDefault": false,
        "projectTokenAddress": "0xf855acf79501609d982e4793f588dff69fc4c7e6",
        "singleUnitPrice": "1.113820",
        "singleUnitBuyPrice": "1.113820",
        "singleUnitSellPrice": "1.123817"
      }
  ],
  "creditTokenAddress": "0xf855acf79501609d982e4793f588dff69fc4c7e6",
  "listings": [],
  "price": "1.11382",
  "updatedAt": "1706418438",
  "hasSupply": true
}
```

### 4. Create an Order

The actual retirement transaction will be initiated instantly by our system, and broadcast to a blockchain network so the underlying credit is permanently retired and taken out of circulation.

It typically takes between 0.5 and 3 seconds for the retirement to be complete — this includes Order creation, transaction broadcast and confirmation on the blockchain, and final retirement certificate generation.

**Since we are creating an order for a carbon pool, we will need to include the required parameters:** `pool`, `projectTokenAddress`, and `quantity_tonnes`.

```typescript
const response = await fetch("https://api.carbonmark.com/orders", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${process.env.CARBONMARK_API_SECRET}`,
  },
  body: JSON.stringify({
    pool: "nct",
    credit_token_address: "0xf855acf79501609d982e4793f588dff69fc4c7e6",
    quantity_tonnes: 1.5,
  }),
});

const { id, url, status, transaction } = await response.json();
```

### View retirement receipt

When the transaction is finalized and our system has marked the order status as “complete”, the provided URL should take you to the shareable retirement receipt.

`carbonmark.com/retirements/{beneficiaryAddress}/{number}`

For example: [https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33](https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33)

The order will appear in your developer dashboard as well.

The cost of the order will be added to your monthly invoice.

## Want to learn more?

One of our solution specialists would be happy to answer any questions you have.

* [Book an API demo](https://meetings-eu1.hubspot.com/meetings/liamellul/api-demo)
* [Contact us](https://share-eu1.hsforms.com/1\_VneTUObQZmJm4kNcRuEoQg3axk)

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

### 1. Get authenticated

Create a [Carbonmark](https://carbonmark.com) account.

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

Retrieve either the `listingId` (for direct seller listings) or the `tokenAddress` (for projects in liquidity pools) from `api.carbonmark.com/projects/{credit_id}`

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

```typescript
const response = await fetch("https://api.carbonmark.com/orders", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${process.env.CARBONMARK_API_SECRET}`,
    body: JSON.stringify({
      listindId:
        "0x0fe3b2c9c9021d6ba72d7071efaad5f080ae48d447d55c87482d9b736790562a",
      quantity: 1.5,
			beneficiaryAddress: "0xABC123b819869c3216c8ea33cda38d8ed829e9f1", // optional
      maxCostCents: "4500", // safeguard: maximum cost in USD cents
    }),
  },
});

const { id, url, status, transaction } = await response.json();
```

### View retirement receipt

When the transaction is finalized and our system has marked the order status as “complete”, the provided URL should take you to the shareable retirement receipt.

`carbonmark.com/retirements/{beneficiaryAddress}/{number}`

For example: [https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33](https://www.carbonmark.com/retirements/0xab5b7b5849784279280188b556af3c179f31dc5b/33)

The order will appear in your developer dashboard as well.

The cost of the order will be added to your monthly invoice.

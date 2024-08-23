---
description: Learn how to use Postman to retire carbon via REST API
---

# Postman Quickstart Guide

{% hint style="warning" %}
**Note: This is a feature preview and the Retirement API is currently in alpha.** These endpoints and parameters are in active development and are likely to change.
{% endhint %}

Carbonmark's Retirement API offers developers a path to initiate carbon retirements via a REST endpoint. [Postman](https://www.postman.com/product/what-is-postman/) is a software tool that helps developers design, build, test and manage APIs. \
\
Follow the guide below to get started retiring carbon using Postman:

{% hint style="success" %}
In this guide, you will learn to:

* Get access to the Carbonmark developer dashboard and get your API Key
* Import Carbonmark REST API into Postman
* Create an order
* Confirm the status of your order and carbon credit retirement
{% endhint %}

{% hint style="info" %}
**What is the difference between retiring carbon from a seller listing and retiring carbon from a pool?**

Listings can be from an individual or organization seller (i.e. Seller Listing) or from a Carbon Pool. A project may therefore have multiple listings at different price points.

Seller listing prices are controlled by the seller. Carbon Pool pricing is dynamic and can incur slippage.
{% endhint %}

## Retire Carbon via Postman

### 1. Get authenticated

[**Request access to the developer dashboard to generate your API Key before proceeding.**](https://share-eu1.hsforms.com/1\_VneTUObQZmJm4kNcRuEoQg3axk)

{% hint style="danger" %}
The API Key is sensitive and can be used to create costly retirements on your behalf. \
**Be careful not to expose the API Key or commit it to your repository.**
{% endhint %}

### 2. Import Carbonmark REST API into Postman

Either, [download Postman](https://www.postman.com/downloads/) or sign into the [Postman web application](https://postman.com).

Open Postman and click the Import button. Paste this link to import Carbonmark API: [`https://api.carbonmark.com/openapi.json`](https://api.carbonmark.com/openapi.json) \
\
Select Postman Collection and click Import. _You may want to review your import settings. We recommend using 'Example' as the Parameter generation._&#x20;

_Depending on whether you select to inherit authorization in your import settings, you may need to set your API key in the request authorization tab and not the collection authorization tab._

### 3. Configure your API request

Select the newly imported Carbonmark REST API collection. Open the Authorization tab and under Type select Bearer Token from the dropdown menu. Paste your Carbonmark API Key into the Token field. _Don't forget to save by clicking the save button._

Navigate to `Submit a retirement order` in the `orders` folder. Open the Body tab and configure your retirement order with your desired variables.

_If you've imported the collection correctly, it should be pre-configured with an example; you can see other examples of order to retire from seller listings or carbon pools in our_ [_reference docs_](https://api.carbonmark.com/#/paths/orders/post)_._

### 4. Create an Order

Send your request by clicking the send button.&#x20;

You should receive a response containing your order details including order `id` and `status` of your order which will allow you to confirm your order has been `SUBMITTED` and will be completed shortly.

{% hint style="info" %}
The actual retirement transaction will be initiated instantly by our system, and broadcast to a blockchain network so the underlying credit is permanently retired and taken out of circulation.

It typically takes between 0.5 and 3 seconds for the retirement to be complete — this includes Order creation, transaction broadcast and confirmation on the blockchain, and final retirement certificate generation.
{% endhint %}

### 5. Confirm your order is complete and view your retirement receipt

To verify your order has been completed, navigate to get `Order details` in the `orders/{id}` folder. Under the params tab, enter the `id` value of your order.

Send your request by clicking the send button.&#x20;

You should receive a response indicating the current status of your order. Once your order `status` is `COMPLETED`, your order is complete and the carbon credit has been retired.

You can follow the `polygonscan_url` or `view_retirement_url` URLs in the response for further confirmation that your retirement order transaction is complete.\
\
Alternatively, you can navigate back to [Carbonmark Developer Dashboard](https://developers.carbonmark.com/dashboard/usage) and see the entry and the order status.

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

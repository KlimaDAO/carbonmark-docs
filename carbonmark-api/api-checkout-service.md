---
description: >-
  Use the Checkout Service API to create hosted checkout links for carbon credit
  purchases and retirements, then track payment and retirement status through
  the API.
---

# API Checkout Service

The [Checkout Service is a REST API](https://checkout.carbonmark.com/) that creates unique checkout links powered by Stripe. Your application sends a listing-based `asset_price_source_id`, `quantity_tonnes`, retirement metadata, and redirect URLs, and the API returns a hosted checkout URL. When the user opens that URL and completes payment, the retirement flow is initiated.

This is the fastest path for teams that want to offer carbon credit purchases and retirements without building their own payment collection flow.

## When to use the Checkout Service

The Checkout Service is a good fit when you want to:

* offer carbon credit purchases and retirements in your application
* avoid building and maintaining a custom payment flow
* use a hosted checkout experience instead of collecting payment details directly
* get to market faster with a simpler integration

## How it works

At a high level, the flow is:

1. Your application sends a checkout request with an `asset_price_source_id`, `quantity_tonnes`, retirement metadata, and redirect URLs
2. The Checkout Service returns a hosted payment URL
3. The user opens the URL and completes payment
4. Your application reads the `payment_id` from the success redirect URL
5. Your application queries `/payments/{id}` to confirm payment and retirement status

## Core endpoints

The Checkout Service currently exposes these key endpoints:

* [`POST /checkout`](https://checkout.carbonmark.com/#/paths/checkout/post) — create a hosted checkout URL
* [`POST /quotes`](https://checkout.carbonmark.com/#/paths/quotes/post) — request a retirement quote for a given asset price source and quantity
* [`GET /payments/{id}`](https://checkout.carbonmark.com/#/paths/payments-id/get) — retrieve payment details and retirement status

## Checkout request requirements

To create a checkout URL, send a request to [`POST /checkout`](https://checkout.carbonmark.com/#/paths/checkout/post) with:

* `asset_price_source_id`
* `quantity_tonnes`
* `metadata`, including:
  * `beneficiary_name`
  * `retirement_message`
* `cancel_url`
* `success_url`

The `success_url` is suffixed with `/<payment_id>`. You can use that `payment_id` to query the `/payments` endpoint and confirm the result of the transaction.

## Availability and access

The Carbonmark API Checkout Service is available for use.

Use the endpoint reference to implement the hosted checkout flow and retrieve payment status through the API.

---
description: >-
  Discover carbon credit inventory, retrieve pricing, and retire credits
  programmatically.
---

# Introduction to Carbonmark API

Carbonmark is designed for teams adding carbon credit purchasing and retirement to software products, internal tools, and customer workflows. Use the API to embed carbon credit discovery and retirement directly into your application without needing to manage credit inventory yourself.

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

## What you can do

With the Carbonmark API, you can:

* Discover available carbon credit inventory
* Retrieve real-time pricing and quotes
* Retire carbon credits programmatically
* Offset fractional quantities starting at 0.001 tCO₂
* Access public retirement records and certificates
* Test your integration for free using sandbox API keys

## Supported networks

The Carbonmark API is multi-network aware and supports carbon credit discovery, pricing, orders, and retirements across Polygon and Base.

<table><thead><tr><th width="147.15625">Network</th><th width="121.73828125">Status</th><th>Supported activity</th></tr></thead><tbody><tr><td>Polygon</td><td>Supported</td><td>Carbon credit discovery, pricing, orders, retirements, and public retirement records</td></tr><tr><td>Base</td><td>Supported</td><td>Klima Protocol carbon class liquidity, pricing, retirements, and public retirement records</td></tr></tbody></table>

Network availability may vary by credit, listing, and liquidity source. API responses include network-aware fields so your integration can determine which chain a price, order, or retirement is associated with.

For transaction links, use the chain-agnostic explorer fields returned by the API instead of network-specific explorer URL fields.

## Common use cases

The API is commonly used to:

* Add carbon retirement to checkout or transaction flows
* Add carbon features to fintech or consumer applications
* Support internal climate, procurement, or reporting workflows
* Integrate carbon retirement into platforms, marketplaces, or enterprise systems

## How it works

The typical integration flow is:

{% stepper %}
{% step %}
### Identify available credits or products
{% endstep %}

{% step %}
### Retrieve pricing in real time
{% endstep %}

{% step %}
### Submit a retirement transaction
{% endstep %}

{% step %}
### Receive confirmation and retirement documentation
{% endstep %}
{% endstepper %}

You do not need to pre-purchase or hold inventory to get starte&#x64;**.**

## Getting started

To complete your first integration:

1. Create an account or sign in with your existing Carbonmark account in the [Developer Dashboard](https://developers.carbonmark.com/login).
2. Generate a free sandbox API key
3. Follow the [Quickstart](carbonmark-api/quickstart.md) guide to complete your first test retirement
4. Review the API reference for available endpoints
5. [Contact Carbonmark](https://share-eu1.hsforms.com/1RWJWvyrHT1C_an4cZOHH3gfhhlr) when you are ready for production access and onboarding

{% content-ref url="carbonmark-api/quickstart.md" %}
[quickstart.md](carbonmark-api/quickstart.md)
{% endcontent-ref %}

---
description: Explore carbon market data, select and retire carbon programatically.
---

# Introduction to Carbonmark API

**Carbonmark** is a peer-to-peer platform for listing, selling and retiring carbon credits.&#x20;

The **Carbonmark Application Programming Interface (API)** allows you to programatically access the growing supply of verified credits on the platform, automating the procurement process for your application.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Benefits of using the Carbonmark API include:

* Real-time quoting and pricing.
* Instant transaction settlement complete with certificate.
* Ability to offset fractional quantities, e.g. 0.8 tCO2 or 1.6 tCO2.
* No need to carry inventory. Simply route orders through the API and receive an invoice at end of month.&#x20;

## Getting started with Carbonmark API

Get access and complete your first retirement with the QuickStart guide below.

{% content-ref url="developers/quickstart.md" %}
[quickstart.md](developers/quickstart.md)
{% endcontent-ref %}

## Carbonmark API Reference Documentation

You can find the latest API Reference docs of the Carbonmark REST API at [api.carbonmark.com/](https://api.carbonmark.com/#/).&#x20;

{% hint style="info" %}
The root `api.carbonmark.com` domain is always running the latest version of the code on the `main` branch of [the monorepo](https://github.com/KlimaDAO/klimadao/tree/main/carbonmark-api).\
\
As mentioned above, any breaking changes and hotfixes will be tagged with a version number in the monorepo.
{% endhint %}

These are generated from an OpenAPI JSON spec that is auto-generated from our Fastify schemas. The raw JSON spec is not kept in source control-- it can be accessed at [api.carbonmark.com/openapi.json](https://api.carbonmark.com/openapi.json).

Fastify schemas are co-located with the respective route handlers, except for the root OpenAPI config, which is located in [src/plugins/open-api.ts](https://github.com/KlimaDAO/klimadao/blob/staging/carbonmark-api/src/plugins/open-api.ts). For more info, see the @fastify/swagger plugin docs.

## Code

You can find the code repository for the Carbonmark REST API in the this monorepo - [each version number can be found under tags](https://github.com/KlimaDAO/klimadao/tags).

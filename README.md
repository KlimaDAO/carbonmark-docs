---
description: >-
  Learn to explore carbon market data, acquire credits, and retire carbon via
  Carbonmark API
---

# Introduction to Carbonmark API

Are you a developer? Get access and get started with the quickstart guide below.

{% content-ref url="developers/quickstart.md" %}
[quickstart.md](developers/quickstart.md)
{% endcontent-ref %}

**Looking to explore digital carbon market data?** Get started [exploring carbon projects via REST API](developers/explore-carbon-projects/).

**Retire carbon now and pay by invoice within 15 days.** Try our [Retirement API](developers/retire-carbon.md). _(This is a feature preview and the Retirement API is currently in alpha.)_

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

## About Carbonmark

Carbonmark is the Universal Carbon Marketplace – an open-source, open carbon credit market for every organization in the world seeking to acquire, trade, or retire carbon seamlessly.&#x20;

With a user-friendly marketplace, diverse credit options, and competitive pricing, Carbonmark prioritizes user experience and offers instant settlement and valuable data insights. Carbonmark offers project developers and carbon asset holders streamlined access to the Digital Carbon Market. Sellers can easily list their carbon assets with specific prices in an open marketplace alongside existing liquidity pools, ensuring fair and competitive pricing for both buyers and sellers.

Launched with zero fees for both buyers and sellers, Carbonmark has been developed as a means to enable greater access to the opportunities of the Digital Carbon Market. Our mission is to build the largest, most accessible carbon marketplace, providing the digital infrastructure the Voluntary Carbon Market needs to fulfill its role in helping the world achieve net-zero carbon emissions over the coming decades.

{% embed url="https://carbonmark.com" %}

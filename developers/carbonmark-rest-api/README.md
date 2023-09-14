---
description: Explore Carbonmark - View assets, prices, supply, activity and more
---

# Carbonmark REST API

You can use the Carbonmark REST API to view assets, prices, supply, activity and more.

### Reference documentation

You can find the latest API Reference docs of the Carbonmark REST API at [api.carbonmark.com/](https://api.carbonmark.com/#/).&#x20;

{% hint style="info" %}
The root `api.carbonmark.com` domain is always running the latest version of the code on the `main` branch of [the monorepo](https://github.com/KlimaDAO/klimadao/tree/main/carbonmark-api).\
\
As mentioned above, any breaking changes and hotfixes will be tagged with a version number in the monorepo.
{% endhint %}

These are generated from an OpenAPI JSON spec that is auto-generated from our Fastify schemas. The raw JSON spec is not kept in source control-- it can be accessed at [api.carbonmark.com/openapi.json](https://api.carbonmark.com/openapi.json).

Fastify schemas are co-located with the respective route handlers, except for the root OpenAPI config, which is located in [src/plugins/open-api.ts](https://github.com/KlimaDAO/klimadao/blob/staging/carbonmark-api/src/plugins/open-api.ts). For more info, see the @fastify/swagger plugin docs.

### Code

You can find the code repository for the Carbonmark REST API in the KlimaDAO monorepo - [each version number can be found under tags](https://github.com/KlimaDAO/klimadao/tags).

<table data-full-width="false"><thead><tr><th>Version</th><th>Link</th></tr></thead><tbody><tr><td>V1.1</td><td><a href="https://github.com/KlimaDAO/klimadao/tree/carbonmark-api/v1.1.0">Link to repo</a></td></tr><tr><td>V1</td><td><a href="https://github.com/KlimaDAO/klimadao/releases/tag/carbonmark-api%2Fv1">Link to repo</a></td></tr></tbody></table>


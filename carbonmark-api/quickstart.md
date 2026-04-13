---
description: >-
  Complete your first Carbonmark API integration by generating a sandbox key and
  submitting a test retirement.
---

# Quickstart

## Latest stable version

[https://v18.api.carbonmark.com](https://v18.api.carbonmark.com)

## Use your preferred API client

The examples in these docs use `curl`, but you can follow the same workflow in any API client, including Postman, Insomnia, Bruno, or your own HTTP tooling.

If your client supports OpenAPI imports, you can import the Carbonmark OpenAPI spec and authenticate using your API key as a Bearer token.

The workflow remains the same:

1. Retrieve a listing price source from `/prices`
2. Create a quote with `/quotes`
3. Create an order with `/orders`
4. Confirm completion by querying `/orders` with the `quote_uuid`

## Getting started

1. Create an account or sign in with your existing Carbonmark account in the [Developer Dashboard](https://developers.carbonmark.com/login).
2. Once logged in, visit the [Keys](https://developers.carbonmark.com/dashboard/keys) page to generate a free sandbox API key.
3. Review the [API reference](https://api.carbonmark.com/) and guides for available endpoints and workflows.
4. [Contact Carbonmark](https://share-eu1.hsforms.com/1RWJWvyrHT1C_an4cZOHH3gfhhlr) when you are ready for production access and onboarding.

{% content-ref url="versioning-and-release-process/" %}
[versioning-and-release-process](versioning-and-release-process/)
{% endcontent-ref %}

{% content-ref url="retire-carbon.md" %}
[retire-carbon.md](retire-carbon.md)
{% endcontent-ref %}

## Explore projects

You can use the Carbonmark REST API to view assets, prices, supply, and other variables.

{% content-ref url="explore-carbon-projects/" %}
[explore-carbon-projects](explore-carbon-projects/)
{% endcontent-ref %}

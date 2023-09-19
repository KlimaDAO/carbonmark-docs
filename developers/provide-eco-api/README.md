# Provide ECO API

Any application – whether it is your own, or a part of SAP BTP – can become a gateway for organizational sustainability.

Provide ECO API (sometimes referred to as Carbonmark API) enables developers to tap into instant, fractionalized, auditable digital carbon retirements.

Provide ECO API never requires users to hold any digital assets - abstracting away blockchain network functionality and offering convenient billing and invoicing options.

**Follow the quickstart below to get started with Postman:**

{% content-ref url="quickstart.md" %}
[quickstart.md](quickstart.md)
{% endcontent-ref %}

{% hint style="info" %}
For further documentation from Provide can be found in the [Provide ECO API Resource Hub](https://github.com/provideplatform/eco-api-resources/).&#x20;
{% endhint %}

#### API Flow overview

1. **Setup**
   1. Authorize Access Token via Basic Auth
   2. List organizations
   3. Generate long-dated token
   4. Get access token from refresh token
   5. List Vaults
   6. Get Vault wallet details
2. **Generate Retirement Transaction Parameters. Choose a request type that best fits your requirement**
   1. Retire By Specific Carbon Project
   2. Retire By Given Dollar Amount
   3. General Retirement
3. **Sign transaction with Vault**
4. **Broadcast retirement transaction**


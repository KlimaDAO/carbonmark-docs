# Puro Bridge Integration Guide

This guide is adopted from Toucan Protocol’s reference documentation. Toucan Protocol is a third party partner that Carbonmark utilizes for interfacing with the [Puro.Earth](https://puro.earth/) registry.

Please note that ongoing updates will be made to this documentation as updates are made to Puro.Earth’s registry interface and Toucan Protocol’s credit transfer infrastructure.

### About Toucan

[Toucan](https://toucan.earth/) builds technology to accelerate climate finance. Their product suite consists of bridging, liquidity, and carbon data solutions.

### About Puro.Earth

Puro.earth certifies suppliers based on the [Puro Standard](https://puro.earth/puro-standard-carbon-removal-credits). Removal is independently verified and CO2 Removal Certificates (CORCs) are issued through the [Puro Registry](https://registry.puro.earth/carbon-sequestration/retirements). CORCs require at least 100 years of permanence.

### Toucan Puro Bridge V1

You **must have a Puro account** in order to use the bridge. A Puro platform membership gives access to the Puro Registry and allows you to purchase CORCs directly from suppliers. This membership costs €900 ([learn more](https://puro.earth/fees)) and will expand your permitted activities.

The Toucan Puro Bridge V1 enables our partners to **tokenize**, **detokenize**, and **retire** CO2 Removal Certificates (CORCs) in the Puro Registry.

Please note, some of these actions are subject to current limitations. Toucan will continue to collaborate with Puro’s engineering team to improve flexibility in the future. Should you have questions about Toucan’s technology, please `email: support@toucan.earth`.

## Import Credits to Carbonmark

Below you will find a comprehensive, step-by-step guide designed to facilitate streamlined access to CORCs on Puro.Earth’s registry.

### Step 1: Prepare Your Trader Account

If you’ve already done this, please skip to **Step 2: On-Ramp Assets**.

Each bridge user must be onboarded as a recognized “Trader” within Toucan’s Sales Channel in the Puro API. The first time you use our bridge, Toucan will first ask for your information in order to prepare for the process.

1. Go to the [Toucan Puro Bridge Trader Onboarding Form](https://toucanprotocol.typeform.com/to/NuMht1wy).
2.  Fill in the necessary details, which will include information about your organization

    and a prompt to accept the bridge’s Terms and Conditions.
3. Go to **Step2: On-Ramp Assets**. In requesting that your credits are on-ramped to the Toucan Sales Channel, Puro will create your Trader account.

{% hint style="info" %}
**Important note:** Your request to create a trader account can only be initiated by Puro, and therefore requires **First Time Users** (Puro account holders who have not yet on- ramped any assets to Toucan’s Sales Channel) to be completed.
{% endhint %}

### Step 2: On-Ramp Assets

On-ramp your assets using the email template below. If you don’t yet have a Trader account in Toucan’s Sales Channel, Puro has stated that they will initiate the request to set up your Trader account when they receive this email.

#### First Time Users&#x20;

For Puro account holders who **have not yet** on-ramped any assets to Toucan’s Sales Channel.

> **To:** tech-support@puro.earth&#x20;
>
> **CC:** nora@toucan.earth
>
> **Subject:** On-ramp assets to Toucan’s Sales Channel and initiate request to set up Trader account
>
> **Body:** Please on-ramp the following assets to Toucan’s Sales Channel and, in the process, initiate a request to set up our organization’s Trader account within the Toucan Sales Channel that Toucan can then approve:
>
> _Credit owner details_
>
> * Name of the organization (if you currently own the credits, this will be your organization)
> * Address
> * Business ID
>
> _Sales channel details_
>
> * Name of the organization: Toucan Protocol Association
> * Business ID: CHE-381.295.616
>
> _Other details_
>
> * Account number credits will be transferred **from**
> * Credit ID number (for each batch)
> * Amount of credits (for each batch)
> * **Set automatic transaction approval on**

#### Returning Users

For Puro account holders **who have already** on-ramped assets to Toucan’s Sales Channel.

> **To:** tech-support@puro.earth&#x20;
>
> **CC:** nora@toucan.earth
>
> **Subject:** On-ramp assets to Toucan’s Sales Channel
>
> **Body:** Please on-ramp the following assets to Toucan’s Sales Channel:
>
> _Credit owner details_
>
> * Name of the organization (if you currently own the credits, this will be your organization)
> * Address
> * Business ID
>
> _Sales channel details_
>
> * Name of the organization: Toucan Protocol Association
> * Business ID: CHE-381.295.616
>
> _Other details_
>
> * Account number credits will be transferred **from**
> * Credit ID number (for each batch)
> * Amount of credits (for each batch)
> * **Set automatic transaction approval on**

{% hint style="info" %}
**Important note for both users:** You must turn on automatic transaction approval to use the bridge.
{% endhint %}

### Step 3: Transfer Onto Carbonmark's Platform

1. **Submit Tokenization Request:** For V1, Toucan will initiate the tokenization of CORCs once you submit the [request form](https://toucanprotocol.typeform.com/to/Rg46Snpq). Later upgrades will allow partners with a Trader account to independently initiate tokenization via Toucan from Carbonmark’s UI. [This video demonstrates how to fill out the tokenization request form](https://www.loom.com/share/deaac9b07b774793ba07f1a573671b2d).
2. **Toucan Initiates Tokenization:** Upon receiving your tokenization request, Toucan will initiate the tokenization process and automatically convert your CORCs into tokenized assets.
3. **TCO2 Tokens Deposited:** The off-chain CORCs will be batch-minted into an ERC721 NFT, which will then be fractionalized into ERC20 TCO2s and deposited into the on-chain wallet that you specified in the request form (note, this essentially refers to the representation of Puro.Earth credits on our platform).

{% hint style="info" %}
**Important note:** Toucan will request an account ID (i.e. _a wallet address_) for depositing the Puro credits into your Carbonmark account. This account ID can be located from your[`Profile`](https://www.carbonmark.com/login) page on Carbonmark's platform.
{% endhint %}

## Export Credits from Carbonmark to your Puro Account

### Step 1: Submit Detokenization Request

This works in a similar manner to retirement; however, if you want to detokenize Puro Credits on Carbonmark which you didn’t previously transfer onto our platform yourself, please consult your Carbonmark point of contact before proceeding.

## Support

Should you encounter any issues or have questions, we are here to assist. The most direct channels for support are:

* Reach out to your designated Carbonmark account manager directly via email.
* Email Toucan Protocol at `support@toucan.earth` for general inquiries and technical assistance.

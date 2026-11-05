---
description: >-
  Choose how you pay for production retirements, set up your account, and get a
  production API key.
---

# Billing and production access

Sandbox keys are free and never billed. Production retirements are paid for in one of two ways: **prepaid**, where you add funds by card before you retire, or **postpaid**, where you are invoiced monthly under an agreement with Carbonmark.

## Choose a billing option

You choose a billing option on the **Billing** page of the [Developer Dashboard](https://developers.carbonmark.com/dashboard/billing).

|                 | Prepaid                                                                               | Postpaid                                          |
| --------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------- |
| How you pay     | Add funds by card in the Developer Dashboard. Retirements are paid from your balance. | Monthly invoice, arranged with Carbonmark sales.  |
| Production keys | Self-serve once business setup is complete and your first payment is credited.        | Available after Carbonmark approves your account. |
| Good fit for    | Getting to production quickly without a sales conversation.                           | Larger or contracted volumes.                     |

To change your billing option after you have chosen one, [contact Carbonmark](https://share-eu1.hsforms.com/1RWJWvyrHT1C_an4cZOHH3gfhhlr).

## Set up prepaid billing

### Step 1: Choose Prepaid

Sign in to the [Developer Dashboard](https://developers.carbonmark.com/login), open **Billing**, and select **Prepaid**.

### Step 2: Complete business setup

Enter your business details:

* Company name
* Legal entity name
* Company website
* Country
* How you will use the API

You can update your business details later from the Billing page.

### Step 3: Add funds

Choose an amount and select **Purchase**. Review the processing fee and the total card charge, then select **Continue to Checkout** to pay by card.

Your balance is updated once the payment is confirmed. Returning from checkout does not by itself mean the funds have been added; the Billing page shows the payment status.

### Step 4: Create a production key

Open **Keys**, create a key, and select **Production**. Set a weekly spending limit for the key. The dashboard shows the highest limit your plan allows.

Keys are shown only once, so copy and store yours securely.

{% hint style="info" %}
You can create sandbox keys and test your integration at any time, before or after choosing a billing option. Sandbox retirements never use your balance.
{% endhint %}

## Adding funds

* **Amounts.** The minimum purchase is $50. Your plan's maximum per purchase is shown on the Billing page.
* **Processing fee.** A card processing fee is added to each purchase and shown before you pay. Your balance increases by the amount you chose; the fee is not added to your balance.
* **Card verification.** Purchases with a total card charge above $1,000 require extra verification from your card issuer (3D Secure). Cards issued in some countries may not be accepted.
* **Pending purchases.** A purchase that is waiting for payment counts toward your limits until it is paid or closes unpaid. If you close the checkout tab, select **Continue to Checkout** in Purchase History to finish paying.
* **Receipts.** Every payment and its receipt appears in Purchase History on the Billing page.

Purchase History shows one of these statuses for each payment:

| Status            | Meaning                                                             |
| ----------------- | ------------------------------------------------------------------- |
| Starting checkout | The purchase has been created and checkout is being prepared.       |
| Awaiting payment  | Checkout is open and waiting for payment.                           |
| Adding credits    | Payment is confirmed and the funds are being added to your balance. |
| Completed         | Payment is confirmed and the funds are in your balance.             |
| Not paid          | Checkout closed without payment. Nothing was charged.               |
| Needs attention   | Something about the payment needs review. Contact support.          |

## How your balance is used

When you submit an order with a production key, the order's cost (`cost_usdc` from the quote) is reserved from your available balance.

* If the retirement completes, the reserved amount is charged.
* If the order fails, the reservation is released back to your balance.
* Creating a quote does not reserve funds. Only orders do.
* Sandbox keys never reserve or charge your balance.

**Available balance** is your funded balance minus amounts reserved for orders still in progress. The dashboard shows a notice when your available balance falls below $50.

## Limits

Prepaid accounts have spending and funding limits. Your current limits and how much of each you have used are shown on the Dashboard and Billing pages.

| Limit              | What it controls                                             |
| ------------------ | ------------------------------------------------------------ |
| Spend limit        | Total retirement spend across all keys over a rolling period |
| Key spend limit    | Retirement spend for one key over a rolling 7 days           |
| Maximum per top-up | The largest single purchase                                  |
| Top-up limit       | Total purchases over a rolling period                        |
| Maximum balance    | The most your balance can hold, including pending purchases  |

Spend limits free up gradually as older spending moves out of the rolling period. Adding funds does not reset spend limits.

To raise a limit, select **Request a limit increase** on the Billing page.

## Account restrictions

If a payment is refunded, disputed, or needs review, Carbonmark may restrict your account. While your account is restricted, production requests can return `PREPAID_RESTRICTED`. The Billing page explains the restriction and what to do next.

## Next steps

* [Errors](errors.md): handle balance and limit errors in your integration
* [Retire Carbon](retire-carbon.md): the full retirement workflow

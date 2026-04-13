---
description: >-
  Use the Checkout Service API to create hosted payment links that let users
  purchase and retire carbon credits without building a custom payment flow.
---

# API Checkout Service (beta)

Use the Checkout Service API to create hosted payment links that let users purchase and retire carbon credits without building a custom payment flow.

The Checkout Service is a REST API that creates unique checkout links powered by Stripe. Your application provides a credit and quantity, and the API returns a hosted payment URL. When the user opens that URL and completes payment, the retirement flow is initiated.

This is the fastest path for teams that want to offer carbon credit purchases and retirements without implementing their own payment processing experience.

## When to use the Checkout Service

The Checkout Service is a good fit when you want to:

* offer carbon credit purchases and retirements in your application
* avoid building and maintaining a custom payment flow
* use a hosted checkout experience instead of collecting payment details directly
* get to market faster with a simpler integration

## How it works

At a high level, the flow is:

1. Your application sends a request with the credit and quantity to retire
2. The Checkout Service returns a unique hosted payment URL
3. The user opens the URL and completes payment
4. The retirement flow is initiated after successful payment

## Availability and access

The Carbonmark API Checkout Service is currently in beta and is not generally available.

If you are interested in using the Checkout Service, contact the Carbonmark team to discuss availability, onboarding, and integration requirements.

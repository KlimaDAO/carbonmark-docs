# v20 (latest stable)

### Status

<mark style="color:red;">`Live`</mark> August 17, 2026

`Updated` November 9, 2026: Prepaid Billing

### Summary

This API release extends marketplace support to Base alongside Polygon: teams now hold multi-chain wallets, and Base listings, quotes, orders, holdings (including native USDC and carbon credits), and Circle wallets are fully supported. Holdings can also be fetched for multiple wallet addresses in a single request, and Toucan certificates now work on both chains, with native ICR certificates supported.

The November 9, 2026 update adds prepaid billing: API customers can add funds by card in the Developer Dashboard and create production keys without waiting for approval. It changes `POST /quotes` and `POST /orders` for production keys on prepaid accounts only. Postpaid accounts and sandbox keys are unchanged.

### Release notes

#### **General**

* Extended Base support: Base marketplace listings, holdings, listing quotes/orders, and Circle wallets
* Teams now support multi-chain wallets (Polygon + Base) instead of a single wallet
* Holdings can be fetched for multiple wallet addresses in one request, including Base native USDC and Base carbon credits
* Toucan certificates work across Polygon and Base; native ICR certificates are supported
* Prepaid billing (November 9, 2026): add funds by card and create production keys from the Developer Dashboard. See [Billing and production access](../billing-and-production-access.md)

#### **Endpoints removed**

* None

#### **Endpoints updated**

**GET** `/holdings/:address`

* Now also returns Base chain holdings (native USDC and carbon credits)
* \[⚠️BREAKING CHANGE] Polygon bridged USDC (`usdc`) holdings are no longer returned — only native USDC per chain

**GET** `/listings`

* Now returns listings from both Polygon and Base marketplaces
* Added `sellerWallets` query parameter (array of addresses)
* \[⚠️DEPRECATED] `sellerWallet` is deprecated; prefer `sellerWallets`

**GET** `/listings/:id`

* Added optional `network` query parameter (`polygon` | `base`; defaults to searching both)

**GET** `/teams/:id`

* Added optional `poll` query parameter (`true` syncs wallets states)

**POST** `/quotes`

* Supports Base marketplace listing `asset_price_source_id` values (e.g. `listing-8453-<listingId>`)
* Prepaid accounts (production keys): returns an error with `code` and `recovery` fields when your available balance or a spend limit does not cover the quote, for example `402 PREPAID_INSUFFICIENT_BALANCE`
* Prepaid accounts (production keys): insufficient liquidity returns `503 EXECUTION_UNAVAILABLE`

**POST** `/orders`

* Supports Base marketplace listings
* Prepaid accounts (production keys): orders reserve the quote's cost from your available balance and are charged when the retirement completes; failed orders release the reservation
* Added optional `Idempotency-Key` header (1 to 200 characters). For production keys on prepaid accounts, retrying with the same key returns the original order instead of creating a second one. If omitted, the `quote_uuid` is used
* Prepaid accounts (production keys): new error responses with `code` and `recovery` fields, including `402 PREPAID_INSUFFICIENT_BALANCE`, `409 PREPAID_ACCOUNT_SPEND_LIMIT`, `409 PREPAID_KEY_SPEND_LIMIT` and `409 IDEMPOTENCY_CONFLICT`. Expired or used quotes return `400 PREPAID_QUOTE_INVALID`. See [Errors](../errors.md)

**GET** `/retirements/:id/certificate`

* Certificate generation supports Base Toucan and native ICR retirements

#### **Endpoints added**

**GET** `/holdings`

* Get holdings for one or more wallet addresses via the `addresses` query parameter
* Returns Polygon and Base native USDC plus carbon credit holdings

#### **New optional query parameters**

* None

### Migration Path

* **Listings**
  * Replace the `sellerWallet` parameter with `sellerWallets`
* **Prepaid billing**
  * No changes needed for postpaid accounts or sandbox keys
  * On prepaid accounts, send an `Idempotency-Key` header with each order and handle errors by their `code`

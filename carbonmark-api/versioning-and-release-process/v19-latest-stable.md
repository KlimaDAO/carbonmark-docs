# v20 (latest stable)

### Status

<mark style="color:red;">`Live`</mark> August 17, 2026

### Summary

This API release extends marketplace support to Base alongside Polygon: teams now hold multi-chain wallets, and Base listings, quotes, orders, holdings (including native USDC and carbon credits), and Circle wallets are fully supported. Holdings can also be fetched for multiple wallet addresses in a single request, and Toucan certificates now work on both chains, with native ICR certificates supported.

### Release notes

#### **General**

* Extended Base support: Base marketplace listings, holdings, listing quotes/orders, and Circle wallets
* Teams now support multi-chain wallets (Polygon + Base) instead of a single wallet
* Holdings can be fetched for multiple wallet addresses in one request, including Base native USDC and Base carbon credits
* Toucan certificates work across Polygon and Base; native ICR certificates are supported

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

**POST** `/orders`

* Supports Base marketplace listings

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

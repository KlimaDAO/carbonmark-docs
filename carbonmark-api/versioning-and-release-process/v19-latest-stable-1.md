# v19 (deprecated)

### Status

<mark style="color:red;">`Deprecated`</mark> August 17, 2026; will become <mark style="color:red;">`Retired`</mark> November 17, 2026

### Summary

This stable release adds Klima Protocol carbon liquidity and prices to the Carbonmark API which is now multi-network aware supporting Polygon and Base.

### Release notes

#### **General**

* Added retire support for Klima Protocol carbon class credits.
* Removed legacy Polygon-specific explorer URL fields from order and retirement payload models, standardizing on chain-agnostic explorer URLs.
* Updated API info limits to be chain-specific instead of a single shared value.

#### **Endpoints removed**

* None

#### **Endpoints updated**

**GET** `/info`

* \[⚠️BREAKING CHANGE] Response shape changed:
  * Removed `MAX_USDC`
  * Added `POLYGON_MAX_USDC`
  * Added `BASE_MAX_USDC`

**POST** `/orders` ; **GET** `/orders` ; **GET** `/orders/:id`

* \[⚠️BREAKING CHANGE] Removed `polygonscan_url` from order response model
* Use `on_chain_explorer_url` instead for both Polygon and Base transactions

**GET** `/retirements` ; **GET** `/retirements/:id`

* \[⚠️BREAKING CHANGE] Removed `polygonscanUrl` from retirement response model
* Use `onChainExplorerUrl` instead for both Polygon and Base transactions

#### **Endpoints added**

* None

#### **New optional query parameters**

* GET `/carbonProjects`
  * assetPriceType
* GET `/carbonProjects/:id`
  * assetPriceType
* `GET /prices`
  * minPriceUSD
  * assetPriceType

The `assetPriceType` filter lets you narrow results to projects with specific pricing sources. Accepted values are `listing` (seller listings on Carbonmark) and `klimaprotocol` (Klima protocol pool prices).

### Migration Path

* **Order response**
  * If you were using `polygonscan_url`, migrate to `on_chain_explorer_url`
* **Retirement response**
  * If you were using `polygonscanUrl`, migrate to `onChainExplorerUrl`
* `/info`
  * Replace reads of `MAX_USDC` with chain-specific handling:
    * `POLYGON_MAX_USDC` for Polygon retirements
    * `BASE_MAX_USDC` for Base retirements

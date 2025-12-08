# v18 (latest stable)

### Status

<mark style="color:red;">`Live`</mark>  December 8, 2025

### Summary

This stable release streamlines the API around seller listing based orders, unifies retirement IDs across chains, and adds read-only retirement query support on Base. It removes legacy carbon pools and product order types to simplify integration going forward.

### Release notes

#### **General**

* Removed support for carbon pool and product orders - the API now exclusively supports seller listing based orders.
* Updated retirement endpoints to use a unified retirement ID format that works across chains.
* Introduced support for Base network chain read only retirement queries (no support for performing retirements yet).

#### **Endpoints updated**

**GET** `/retirements`

* Now searches both Polygon and Base chain retirements, returning results from both chains.

**GET** `/retirements/:beneficiaryAddress/:retirementIndex`

* \[⚠️ BREAKING CHANGE] This endpoint has been replaced by **GET** `/retirements/:id`. The new endpoint uses a unified retirement ID format (e.g., `84532-0x1ee2facc42147cc6f6cd1fcdca4e3748d2b41b089fafac46515318bbce4f633c-0`) that works across chains. See [#endpoints-added](v18-latest-stable.md#endpoints-added "mention") for more info.

**GET** `/retirements/:beneficiaryAddress/:retirementIndex/certificate`

* \[⚠️ BREAKING CHANGE] This endpoint has been replaced by **GET** `/retirements/:id/certificate`. See [#endpoints-added](v18-latest-stable.md#endpoints-added "mention") for more info.

**GET** `/retirements/:beneficiaryAddress/:retirementIndex/provenance`

* \[⚠️ BREAKING CHANGE] This endpoint has been replaced by **GET** `/retirements/:id/provenance`. See  [#endpoints-added](v18-latest-stable.md#endpoints-added "mention") for more info.

**GET** `/prices`

* \[⚠️ BREAKING CHANGE] Removed the following query parameters:
  * `productIds` - Products are no longer supported
  * `hasAssetPriceType` - Only listing prices are now returned
  * `isDefaultCredit` - Carbon pools are no longer supported
* The endpoint now exclusively returns prices from listing created by sellers.

**POST** `/quotes`

* \[⚠️ BREAKING CHANGE] Simplified request body - now only accepts `asset_price_source_id` and `quantity_tonnes` .
* Removed deprecated parameters: `listing_id`, `pool`, `credit_token_address` .
* Removed support for carbon pool and product quotes - only listing quotes are supported.

**POST** `/orders`

* \[⚠️ BREAKING CHANGE] Now only supports listing orders. Carbon pool and product orders are no longer supported.
* Requests with non-listing `asset_price_source_id` values will return a 400 error.

**GET** `/carbonProjects`

* Updated to return only listing prices.

#### **Endpoints added**

**GET** `/retirements/:id`

* Get retirement details using a unified retirement ID format that works across Polygon and Base chains.
* The retirement ID format is: `{chainId}-{transactionHash}-{retirementIndex}` (e.g., `84532-0x1ee2facc42147cc6f6cd1fcdca4e3748d2b41b089fafac46515318bbce4f633c-0`).
* This replaces the previous `/retirements/:beneficiaryAddress/:retirementIndex` endpoint.

**GET** `/retirements/:id/certificate`

* Get retirement certificate using a unified retirement ID.
* This replaces the previous `/retirements/:beneficiaryAddress/:retirementIndex/certificate` endpoint.

**GET** `/retirements/:id/provenance`

* Get retirement provenance records using a unified retirement ID.
* This replaces the previous `/retirements/:beneficiaryAddress/:retirementIndex/provenance` endpoint.

#### Endpoints removed

**GET** `/products`

* \[⚠️ BREAKING CHANGE] This endpoint has been removed. Products are no longer supported.

**GET** `/products/:id`

* \[⚠️ BREAKING CHANGE] This endpoint has been removed. Products are no longer supported.

**GET** `/products/:id/stats`

* \[⚠️ BREAKING CHANGE] This endpoint has been removed. Products are no longer supported.

### Migration Path

* **Products endpoints**
  * If you were using **GET** `/products`, **GET** `/products/:id`, or **GET** `/products/:id/stats`, these endpoints have been removed. Products are no longer supported as a retirement source. You should use listing-based orders instead.
* **Retirement endpoints**
  * If you were using **GET** `/retirements/:beneficiaryAddress/:retirementIndex`, you should now use **GET** `/retirements/:id` .
  * If you were using **GET** `/retirements/:beneficiaryAddress/:retirementIndex/certificate`, you should now use **GET** `/retirements/:id/certificate` .
  * If you were using **GET** `/retirements/:beneficiaryAddress/:retirementIndex/provenance`, you should now use **GET** `/retirements/:id/provenance` .
  * The retirement ID can be obtained from the **GET** `/retirements` endpoint, which returns retirements with their IDs.
* **Prices endpoint**
  * If you were filtering by `productIds`, `hasAssetPriceType`, or `isDefaultCredit`, these parameters have been removed. The endpoint now only returns listing prices.
* **Quotes endpoint**
  * If you were using deprecated parameters like `listing_id`, `pool`, or `credit_token_address`, you should now use `asset_price_source_id` .
  * Carbon pool and product quotes are no longer supported - only listing quotes are available.

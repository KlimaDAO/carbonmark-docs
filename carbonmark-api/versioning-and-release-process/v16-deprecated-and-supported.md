# v16 (deprecated & supported)

### Status

`Deprecated` June 4, 2025

Supported until September 4, 2025.

### Summary

The v16 release of the Carbonmark API introduces enhanced query parameters, streamlined models, and improved filtering capabilities across key endpoints. While some attributes and endpoints have been deprecated or removed, new features like wallet-specific queries and retirement certificate generation simplify and expand functionality. Migration guidance is provided to ensure a smooth transition.

### Release notes

#### **Endpoints updated**

* **GET** `/carbonProjects` endpoint:
  * Added a **registry** and **isDefaultProject** query parameters.
  * Added a **satelliteImage** attribute.
* **GET** `/holdings` endpoint:
  * Added a **minAmountTonnes** query parameter.
  * The **amount** attribute of the Holding model is now returned as a number.
* **GET** `/listings` endpoint:
  * Added a **sellerWallet**, **projectIds, tokenIdentifier, active** and **ids** query parameters to improve the filtering capabilities.
  * Removed the **batches** and **batchPrices** attributes.
  * Deprecated the **symbol**, **tokenId** and **tokenStandard** attributes in favour of a **token** object.
  * Added a **creditId** attribute.
* **GET** `/prices` endpoint:
  * Added a **isDefaultCredit** query parameter.
* **GET** `/retirements` endpoint:
  * Removed the **retireeProfile** attribute.
  * Added a **asyncRetirement** attribute.
* **GET** `/activities` endpoint:
  * Added a **walletAddress** query parameter.
* **GET** `/teams` endpoint:
  * Added a **wallet\_address** and **beneficiary\_address** query parameters.
  * The **wallet** attribute is replaced by a **wallets** attribute which contains an array of the team’s wallet ids.
* **Token model**
  * The **decimals** and **tokenStandard** attributes have been added to the model.

#### **Endpoints added**

* **GET** `/holdings/:address`
  * Returns the holdings of the wallet at the given address.
* **GET** `/retirements/:beneficiaryAddress/:retirementIndex/certificate`
  * Generates a certificate for the given retirement.
* **GET** `/retirements/:beneficiaryAddress/:retirementIndex/status`
  * Returns the indexing information of the given retirement.
* **GET** `/users/:handle`
  * Returns of the wallet at the given address.
* **PUT** `/teams/:id/:pin`
  * Creates a challenge to create or update a team’s PIN.
* **POST** `/wallets`
  * Creates a challenge to create a team’s wallet.
* **POST** `/wallets/:uuid/:transactions`
  * Creates a challenge to perform a transaction with the given wallet.
* **GET** `/wallets/:uuid`
  * Returns information for the given wallet.
* **GET** `/wallets/:uuid/:allowances`
  * Returns token allowances of the given wallet.
* **GET** `/wallets/:uuid/:transactions`
  * Returns transactions performed with the given wallet.

#### **Endpoints removed**

* **GET** `/users/:walletOrHandle`
* **POST** `/users`
* **PUT** `/users/:wallet`

### Migration Path

* **Retirement API**
  * No breaking changes were made to the retirement API (`/quotes` and `/orders` endpoints).
* **Web migration**
  * For entities that created a profile on Carbonmark, you could get information about them by using the `GET /users/:walletOrHandle` endpoint. In this new version of the API you should use the `GET /teams` endpoints.
    * Example: [https://v16.api.carbonmark.com/teams?wallet\_address=0xf3de051d4183b1a9a1258c9815cedb0ac725a65f](https://v16.api.carbonmark.com/teams?wallet_address=0xf3de051d4183b1a9a1258c9815cedb0ac725a65f)
* **Other breaking changes**
  * If you were using the `GET /holdings` endpoint make sure that you take into account that the amount attribute is now a number.
  * If you were using the **retireeProfile** information from the Retirement model; you would need to perform an additional query to the `GET /teams` endpoint (see Web migration).
  * If you were using the **batches and batchesPrices** attributes of the Listing model, [please contact us](https://share-eu1.hsforms.com/1RWJWvyrHT1C_an4cZOHH3gfhhlr).

### References

* The Token model: [https://v16.api.carbonmark.com/#/schemas/Token](https://v16.api.carbonmark.com/#/schemas/Token)

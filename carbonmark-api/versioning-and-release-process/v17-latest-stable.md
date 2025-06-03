# v17 (latest stable)

### Status

<mark style="color:red;">`Live`</mark> June 4, 2025

### Summary

v17 is primarily a technical release to improve the stability and performance of order processing.

Quality of life features and endpoint refactoring have also been included. Migration guidance is provided to ensure a smooth transition.

### Release notes

#### **General**

* Removed <mark style="color:red;">`network`</mark> query parameter from the following endpoints. Default network is <mark style="color:red;">`Polygon`</mark>.
  * **GET** `/activities`
  * **GET** `/carbonProjects`
  * **GET** `/carbonProjects/:id`
  * **GET** `/categories`
  * **GET** `/countries`
  * **GET** `/listings/:id`
  * **GET** `/listings`
  * **GET** `/purchases/:id`
  * **GET** `/purchases`
  * **GET** `/prices`
  * **GET** `/retirements`
  * **GET** `/retirements/:id`
  * **GET** `/retirements/:id/provenance`
  * **GET** `/vintages`

#### **Endpoints updated**

* **GET** `/carbonProjects`
  * Added pagination
  * Added a **block\_long\_description** field that contains a project description in **PortableText** format
* **GET** `/carbonProjects/:id`
  * Added a **block\_long\_description** field that contains a project description in **PortableText** format

#### **Endpoints added**

* **GET** `/assessors`
  * Returns the list of known project assessors (related to project verification and validation)
* **GET** `/developers`
  * Returns the list of known project developers

#### **Endpoints refactored**

* Multiple retirements can occur in the same transaction. To take this into account we need to locate the retirements using their beneficiary addresses and retirement indexes:
  * **GET** `/retirements/:hash` ⇒ **GET** `/retirements/:beneficiaryAddress/:retirementIndex`
  * **GET** `/retirements/:hash/provenance` ⇒ **GET** `/retirements/:beneficiaryAddress/:retirementIndex/provenance`

### Migration Path

* **Retirement API**
  * No breaking changes were made to the retirement API (`/quotes` and `/orders` endpoints).
* **Retirement endpoints**
  * If you were using the **GET** `/retirements/:hash` and **GET** `/retirements/:hash/provenance` you should now use the new endpoints:
    * **GET `/retirements/:beneficiaryAddress/:retirementIndex`**&#x20;
    * **GET** `/retirements/:beneficiaryAddress/:retirementIndex/provenance`
  * If you only have the transaction hash you can fetch the beneficiaryAddress and the retirementIndex by using the **GET /retirements** endpoint
    * Example: [https://v17.api.carbonmark.com/retirements?hash=0x7b63b79d25c76b6f179360ebd5f9f3c2435d3fdf2dda7e1d3ab43cf4f90e4a04](https://v17.api.carbonmark.com/retirements?hash=0x7b63b79d25c76b6f179360ebd5f9f3c2435d3fdf2dda7e1d3ab43cf4f90e4a04)

### References

* The Token model: [https://v17.api.carbonmark.com/#/schemas/Token](https://v17.api.carbonmark.com/#/schemas/Token)

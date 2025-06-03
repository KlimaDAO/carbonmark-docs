# v17 (latest stable)

### Status

<mark style="color:red;">`Live`</mark> June 4, 2025

### Summary

v17 is primarily focused on improving the stability and performance of order processing. It includes a few breaking changes from v16 (highlighted below).

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

**GET** `/carbonProjects`

* \[⚠️BREAKING CHANGE] Response entries are now paginated
* Added a **block\_long\_description** field that contains a project description in **PortableText** format

**GET** `/carbonProjects/:id`

* Added a **block\_long\_description** field that contains a project description in **PortableText** format

#### **Endpoints added**

**GET** `/assessors`

* Returns the list of known project assessors (related to project verification and validation)

**GET** `/developers`

* Returns the list of known project developers

**GET** `/retirements/:beneficiaryAddress/:retirementIndex`

* Get retirement details for a completed retirement order. This replaces the endpoint that was present in previous versions (see [Endpoints removed](v17-latest-stable.md#endpoints-removed) below)

**GET** `/retirements/:beneficiaryAddress/:retirementIndex/provenance`

* Get the credit provenance history leading up to a completed retirement. This replaces the `/provenance` endpoint that was present in previous versions (see [Endpoints removed](v17-latest-stable.md#endpoints-removed) below)

#### **Endpoints removed**

**GET** `/retirements/:hash`

* \[⚠️BREAKING CHANGE] This endpoint was removed because a transaction hash may now include more than one retirement. It has been replaced by `/retirements/:beneficiaryAddress/:retirementIndex`. See [Endpoints added ](v17-latest-stable.md#endpoints-added)for more info. See [migration path](v17-latest-stable.md#migration-path) for alternatives.

**GET** `/retirements/:hash/provenance`

* \[⚠️BREAKING CHANGE] This endpoint was removed because a transaction hash may now include more than one retirement. It has been replaced by `/retirements/:beneficiaryAddress/:retirementIndex`. See [Endpoints added ](v17-latest-stable.md#endpoints-added)for more info. See [migration path](v17-latest-stable.md#migration-path) for alternatives.

### Migration Path

* **Carbon Projects endpoint**
  * The [`/carbonProjects`](https://v17.api.carbonmark.com/#/paths/carbonProjects/get) endpoint is now paginated. Your UI or system may need to be refactored to consume pages, rather than a complete list of projects. The project data is now provided in the **items** attribute of the response.
* **Retirement endpoints**
  * If you were using the **GET** `/retirements/:hash` and **GET** `/retirements/:hash/provenance` you should now use the new endpoints:
    * **GET** `/retirements/:beneficiaryAddress/:retirementIndex`
    * **GET** `/retirements/:beneficiaryAddress/:retirementIndex/provenance`
  * If you only have the transaction hash you can still use the hash to filter on the **GET** `/retirements` endpoint, which returns a list.
    * Example using hash to filter:\
      [https://v17.api.carbonmark.com/retirements?hash=0x7b63b79d25c76b6f179360ebd5f9f3c2435d3fdf2dda7e1d3ab43cf4f90e4a04](https://v17.api.carbonmark.com/retirements?hash=0x7b63b79d25c76b6f179360ebd5f9f3c2435d3fdf2dda7e1d3ab43cf4f90e4a04)

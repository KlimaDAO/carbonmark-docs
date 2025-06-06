# v15 (retired)

### Status

`Retired` April 29th, 2025

No longer supported.

### Release notes

* in the `/users` endpoint the token object attributes have been aligned:
  * address is added
  * decimals is removed
* in the `/prices` endpoint the carbonCredit attribute that existed in the listing and carbon\_pool objects have been split into a token and a creditId objects. in addition a symbol attribute was added to the token object (for consistency with the token object in the `/users` endpoint)
* in the `/prices` the product object now contains the corresponding token object
* the `/prices` endpoint now expects an arrays instead of a string for the vintage arguments
* in the `/retirements` and `/retirements/:id` endpoints the credit attribute has been split into a token and a creditId objects. in addition a symbol attribute was added to the token object (for consistency with the token object in the `/users` endpoint)
* the id, asset\_id, erc1155\_token\_id, listing\_id, pool\_name attribute are removed from the `/quotes` endpoints, they are replaced by the asset\_price\_source\_id attribute
* the id attribute is removed from the `/orders` endpoints
* the `/carbonProject` endpoint now expects arrays instead of strings for the following arguments: country, category, vintage, sdg

### References

* The Token model: [https://v15.api.carbonmark.com/#/schemas/Token](https://v15.api.carbonmark.com/#/schemas/Token)

---
description: >-
  Use the Carbonmark API to retrieve available country values and return carbon
  projects that match a selected country.
---

# Find carbon projects by country

This page shows the basic workflow. For full parameter definitions and response schemas, see the API reference for each endpoint.

### Step 1: Retrieve available countries

Call the `/countries` endpoint to return the list of valid country values you can use when filtering carbon projects.

```bash
curl --request GET \
  --url https://api.carbonmark.com/countries \
  --header 'Accept: application/json'
```

Example response:

```json
[
  { "id": "Brazil" },
  { "id": "Bulgaria" },
  { "id": "China" },
  { "id": "Congo" },
  { "id": "Ecuador" },
  { "id": "India" }
]
```

### Step 2: Filter projects by country

Once you have a valid country value, pass it to the `/carbonProjects` endpoint.

You can also combine `country` with other supported filters such as:

* `category`
* `vintage`
* project name
* project description

The example below returns renewable energy projects in India with a 2012 vintage.

```bash
curl -G https://api.carbonmark.com/carbonProjects \
  --data-urlencode "country=India" \
  --data-urlencode "category=Renewable Energy" \
  --data-urlencode "vintage=2012" \
  --header 'Accept: application/json'
```

Example response:

```json
[
  {
    "key": "VCS-1160",
    "projectID": "1160",
    "name": "6.5 MW Cogeneration Project in Akbarpur, Punjab",
    "country": "India",
    "registry": "VCS",
    "vintages": ["2012", "2013", "2014"],
    "price": "1.3441532",
    "hasSupply": true
  },
  {
    "key": "VCS-1293",
    "projectID": "1293",
    "name": "Suzlon 10.4 MW Wind Power Project",
    "country": "India",
    "registry": "VCS",
    "vintages": ["2011", "2012"],
    "price": "1.449392",
    "hasSupply": true
  }
]
```

For many workflows, the `key` and `projectID` fields are enough to display results or request full project details.

### Step 3: Retrieve a single project by key

If you need the full details for a specific project, call `/carbonProjects/{key}` using the project `key` returned in the search response.

```bash
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-274 \
  --header 'Accept: application/json'
```

Example response:

```json
{
  "key": "VCS-274",
  "projectID": "274",
  "country": "India",
  "registry": "VCS",
  "name": "Hanuman Ganga Hydro (4.95 MW) Plant at Uttarakhand",
  "price": "1.3429052",
  "hasSupply": true,
  "vintages": ["2010", "2011", "2012", "2013", "2014", "2015"]
}
```

### Notes

* Use `/countries` first to avoid passing unsupported country values.
* Use `/carbonProjects` when you want a filtered list of matching projects.
* Use `/carbonProjects/{key}` when you need full details for a specific project.
* Refer to the API reference for the complete list of supported parameters and response fields.

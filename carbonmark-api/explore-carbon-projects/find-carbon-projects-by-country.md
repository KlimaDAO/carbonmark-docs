---
description: >-
  Use the Carbonmark API to retrieve available country values and return carbon
  projects that match a selected country.
---

# Find carbon projects by country

This page shows the basic workflow. For full parameter definitions and response schemas, see the [API reference](https://api.carbonmark.com/) for each endpoint.

## Step 1: Retrieve available countries

Call the [`/countries`](https://api.carbonmark.com/#/paths/countries/get) endpoint to return the list of valid country values you can use when filtering carbon projects.

```bash
curl --request GET \
  --url https://api.carbonmark.com/countries \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

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

## Step 2: Filter projects by country

Once you have a valid country value, pass it to the [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint.

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

Example response (trimmed for readability):

```json
[
  {
    "key": "VCS-498",
    "projectID": "498",
    "name": "Grid-Connected Wind Electricity Generation Project In Tamil Nadu, India",
    "country": "India",
    "region": "Asia",
    "registry": "VCS",
    "methodologies": [
      {
        "id": "AMS-ID",
        "category": "Renewable Energy",
        "name": "Grid connected renewable electricity generation"
      }
    ],
    "vintages": ["2012"],
    "price": "0.91",
    "hasSupply": true,
    "stats": {
      "totalBridged": 26236,
      "totalRetired": 300.0021,
      "totalSupply": 25922.899
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/498"
  },
  {
    "key": "VCS-1578",
    "projectID": "1578",
    "name": "15 MW Solar Photovoltaic Power Project At Gujarat",
    "country": "India",
    "region": "Asia",
    "registry": "VCS",
    "methodologies": [
      {
        "id": "AMS-ID",
        "category": "Renewable Energy",
        "name": "Grid connected renewable electricity generation"
      }
    ],
    "vintages": ["2012"],
    "price": "0.91",
    "hasSupply": true,
    "stats": {
      "totalBridged": 1936,
      "totalRetired": 1300,
      "totalSupply": 636
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1578"
  }
]
```

For many workflows, the `key` and `projectID` fields are enough to display results or request full project details.

## Step 3: Retrieve a single project by key

If you need the full details for a specific project, call [`/carbonProjects/{key}`](https://api.carbonmark.com/#/paths/carbonProjects-id/get) using the project `key` returned in the search response.

```bash
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-274 \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```json
{
  "key": "VCS-274",
  "projectID": "274",
  "name": "Hanuman Ganga Hydro (4.95 MW) Plant At Uttarakhand",
  "country": "India",
  "region": "Asia",
  "registry": "VCS",
  "methodologies": [
    {
      "id": "AMS-ID",
      "category": "Renewable Energy",
      "name": "Grid connected renewable electricity generation"
    }
  ],
  "vintages": [],
  "price": "0",
  "hasSupply": false,
  "stats": {
    "totalBridged": 39492,
    "totalRetired": 2.0021,
    "totalSupply": 0
  },
  "sustainableDevelopmentGoals": ["7", "8", "9", "13"],
  "url": "https://registry.verra.org/app/projectDetail/VCS/274"
}
```

## Notes

* Use `/countries` first to avoid passing unsupported country values.
* Use `/carbonProjects` when you want a filtered list of matching projects.
* Use `/carbonProjects/{key}` when you need full details for a specific project.
* Refer to the API reference for the complete list of supported parameters and response fields.

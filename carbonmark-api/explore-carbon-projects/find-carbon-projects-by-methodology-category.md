---
description: >-
  Use the Carbonmark API to retrieve available methodology categories and return
  carbon projects that match a selected category.
---

# Find carbon projects by methodology category

Use the Carbonmark API to retrieve available methodology categories and return carbon projects that match a selected category.

This page shows the basic workflow. For full parameter definitions and response schemas, see the [API reference](https://api.carbonmark.com/) for each endpoint.

### Step 1: Retrieve available methodology categories

Call the [`/categories`](https://api.carbonmark.com/#/paths/categories/get) endpoint to return the list of valid methodology category values you can use when filtering carbon projects.

```bash
curl --request GET \
  --url https://api.carbonmark.com/categories \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
[
  { "id": "Agriculture" },
  { "id": "Biochar" },
  { "id": "Blue Carbon" },
  { "id": "Energy Efficiency" },
  { "id": "Forestry" },
  { "id": "Industrial Processing" },
  { "id": "Other" },
  { "id": "Renewable Energy" },
  { "id": "Waste Disposal" }
]
```

### Step 2: Filter projects by methodology category

Once you have a valid methodology category value, pass it to the [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint.

You can also combine `category` with other supported filters such as:

* country
* vintage
* project name
* project description

The example below returns projects in the `Forestry` methodology category from Indonesia.

```bash
curl -G https://api.carbonmark.com/carbonProjects \
  --data-urlencode "country=Indonesia" \
  --data-urlencode "category=Forestry" \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
[
  {
    "key": "VCS-674",
    "projectID": "674",
    "name": "Rimba Raya Biodiversity Reserve Project",
    "country": "Indonesia",
    "region": "Oceania",
    "registry": "VCS",
    "methodologies": [
      {
        "id": "VM0004",
        "category": "Forestry",
        "name": "Methodology for Conservation Projects that Avoid Planned Land Use Conversion in Peat Swamp Forests"
      }
    ],
    "vintages": [],
    "price": "0",
    "hasSupply": false,
    "stats": {
      "totalBridged": 229030,
      "totalRetired": 208614.55942326694,
      "totalSupply": 0
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/674"
  },
  {
    "key": "VCS-1477",
    "projectID": "1477",
    "name": "Katingan Peatland Restoration And Conservation Project",
    "country": "Indonesia",
    "region": "Asia",
    "registry": "VCS",
    "methodologies": [
      {
        "id": "VM0007",
        "category": "Forestry",
        "name": "REDD+ Methodology Framework (REDD+MF)"
      }
    ],
    "vintages": [],
    "price": "0",
    "hasSupply": false,
    "stats": {
      "totalBridged": 0,
      "totalRetired": 0,
      "totalSupply": 0
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1477"
  }
]
```

For many workflows, the `key` and `projectID` fields are enough to display results or request full project details.

### Step 3: Retrieve a single project by key

If you need the full details for a specific project, call `/carbonProjects/{key}` using the project `key` returned in the search response.

```bash
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-674 \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
{
  "key": "VCS-674",
  "projectID": "674",
  "name": "Rimba Raya Biodiversity Reserve Project",
  "country": "Indonesia",
  "region": "Oceania",
  "registry": "VCS",
  "methodologies": [
    {
      "id": "VM0004",
      "category": "Forestry",
      "name": "Methodology for Conservation Projects that Avoid Planned Land Use Conversion in Peat Swamp Forests"
    }
  ],
  "vintages": [],
  "price": "0",
  "hasSupply": false,
  "stats": {
    "totalBridged": 229030,
    "totalRetired": 208614.55942326694,
    "totalSupply": 0
  },
  "sustainableDevelopmentGoals": [
    "1", "2", "3", "4", "5", "6", "7", "8",
    "9", "10", "11", "12", "13", "14", "15", "16", "17"
  ],
  "url": "https://registry.verra.org/app/projectDetail/VCS/674"
}
```

### Notes

* Use `/categories` first to avoid passing unsupported methodology category values.
* Use `/carbonProjects` when you want a filtered list of matching projects.
* Use `/carbonProjects/{key}` when you need full details for a specific project.
* Refer to the API reference for the complete list of supported parameters and response fields.

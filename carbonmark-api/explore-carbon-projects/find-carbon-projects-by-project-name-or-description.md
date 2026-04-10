---
description: >-
  Use the Carbonmark API to search carbon projects by project name or
  description using a text query.
---

# Find carbon projects by project name or description

Use the Carbonmark API to search carbon projects by project name or description using a text query.

This page shows the basic workflow. For full parameter definitions and response schemas, see the [API reference](https://api.carbonmark.com/) for each endpoint.

### Step 1: Search projects using a text query

Call the [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint with the `search` query parameter to return projects whose name or description matches a text string.

The example below searches for projects containing the string `REDD+`.

```bash
curl -G https://api.carbonmark.com/carbonProjects \
  --data-urlencode "search=REDD+" \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
[
  {
    "key": "VCS-844",
    "projectID": "844",
    "name": "Madre De Dios Amazon REDD+ Project",
    "country": "Peru",
    "region": "Latin America",
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
    "url": "https://registry.verra.org/app/projectDetail/VCS/844"
  },
  {
    "key": "VCS-1811",
    "projectID": "1811",
    "name": "Jari/Pará REDD+ Project",
    "country": "Brazil",
    "region": "Latin America",
    "registry": "VCS",
    "methodologies": [
      {
        "id": "VM0015",
        "category": "Forestry",
        "name": "Methodology for Avoided Unplanned Deforestation"
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
    "url": "https://registry.verra.org/app/projectDetail/VCS/1811"
  }
]
```

For many list and search workflows, `key`, `projectID`, `name`, `registry`, `country`, `price`, and `hasSupply` are the most useful fields to start with.

### Step 2: Retrieve a single project by key

If you need the full details for a specific project, call [`/carbonProjects/{key}`](https://api.carbonmark.com/#/paths/carbonProjects-id/get) using the project `key` returned in the search response.

```bash
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-1052 \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
{
  "key": "VCS-1052",
  "projectID": "1052",
  "name": "North Pikounda REDD+",
  "country": "Congo, The Democratic Republic of the",
  "region": "Africa",
  "registry": "VCS",
  "methodologies": [
    {
      "id": "VM0011",
      "category": "Forestry",
      "name": "Methodology for Calculating GHG Benefits from Preventing Planned Degradation"
    }
  ],
  "vintages": ["2012"],
  "price": "1.82",
  "hasSupply": true,
  "stats": {
    "totalBridged": 55532,
    "totalRetired": 46621.89476754726,
    "totalSupply": 452.999
  },
  "sustainableDevelopmentGoals": ["13", "15"],
  "url": "https://registry.verra.org/app/projectDetail/VCS/1052"
}
```

### Notes

* Use the `search` query parameter to match text in project names and descriptions.
* Use `/carbonProjects` when you want a filtered list of matching projects.
* Use `/carbonProjects/{key}` when you need full details for a specific project.
* Refer to the API reference for the complete list of supported parameters and response fields.












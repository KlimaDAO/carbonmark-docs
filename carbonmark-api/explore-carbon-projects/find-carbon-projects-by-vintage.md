---
description: >-
  Use the Carbonmark API to retrieve available vintages and return carbon
  projects that match a selected vintage.
---

# Find carbon projects by vintage

Use the Carbonmark API to retrieve available vintages and return carbon projects that match a selected vintage.

This page shows the basic workflow. For full parameter definitions and response schemas, see the [API reference](https://api.carbonmark.com/) for each endpoint.

### Step 1: Retrieve available vintages

Call the [`/vintages`](https://api.carbonmark.com/#/paths/vintages/get) endpoint to return the list of valid vintage values you can use when filtering carbon projects.

```bash
curl --request GET \
  --url https://api.carbonmark.com/vintages \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
[
  "2006",
  "2007",
  "2008",
  "2009",
  "2010",
  "2011",
  "2012",
  "2013",
  "2014",
  "2015",
  "2016",
  "2017",
  "2018",
  "2019",
  "2020"
]
```

### Step 2: Filter projects by vintage

Once you have a valid vintage value, pass it to the [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint.

You can also combine `vintage` with other supported filters such as:

* country
* category
* project name
* project description

The example below returns projects with a 2020 vintage.

```bash
curl -G https://api.carbonmark.com/carbonProjects \
  --data-urlencode "vintage=2020" \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
[
  {
    "key": "VCS-1547",
    "projectID": "1547",
    "name": "5MW Biomass Based Cogeneration Project At Sainsons",
    "country": "India",
    "region": "Asia",
    "registry": "VCS",
    "methodologies": [
      {
        "id": "ACM0006",
        "category": "Other",
        "name": "Electricity and heat generation from biomass"
      }
    ],
    "vintages": ["2020"],
    "price": "1.4",
    "hasSupply": true,
    "stats": {
      "totalBridged": 3100,
      "totalRetired": 667.831,
      "totalSupply": 0
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1547"
  },
  {
    "key": "ICR-349",
    "projectID": "349",
    "name": "Forestal Río Aquidabán",
    "country": "Paraguay",
    "region": "South America",
    "registry": "ICR",
    "methodologies": [
      {
        "id": "AR-ACM0003",
        "category": "Forestry",
        "name": "Afforestation and reforestation of lands except wetlands"
      }
    ],
    "vintages": ["2020"],
    "price": "22.834",
    "hasSupply": true,
    "stats": {
      "totalBridged": 255379,
      "totalRetired": 961.886,
      "totalSupply": 67992.856
    },
    "url": "https://www.carbonregistry.com/projects/forestal-rio-aquidaban-349?tab=overview"
  }
]
```

For many workflows, the `key` and `projectID` fields are enough to display results or request full project details.

### Step 3: Retrieve a single project by key

If you need the full details for a specific project, call `/carbonProjects/{key}` using the project `key` returned in the search response.

```bash
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-1418 \
  --header 'Accept: application/json'
```

Example response (trimmed for readability):

```bash
{
  "key": "VCS-1418",
  "projectID": "1418",
  "name": "Renewable Energy Project By LNB Group",
  "country": "India",
  "region": "Asia",
  "registry": "VCS",
  "methodologies": [
    {
      "id": "ACM0002",
      "category": "Renewable Energy",
      "name": "Grid-connected electricity generation from renewable sources"
    }
  ],
  "vintages": [],
  "price": "0",
  "hasSupply": false,
  "stats": {
    "totalBridged": 2,
    "totalRetired": 1.1895,
    "totalSupply": 0
  },
  "sustainableDevelopmentGoals": ["7", "8", "9", "13"],
  "url": "https://registry.verra.org/app/projectDetail/VCS/1418"
}
```

### Notes

* Use `/vintages` first to avoid passing unsupported vintage values.
* Use `/carbonProjects` when you want a filtered list of matching projects.
* Use `/carbonProjects/{key}` when you need full details for a specific project.
* Refer to the API reference for the complete list of supported parameters and response fields.

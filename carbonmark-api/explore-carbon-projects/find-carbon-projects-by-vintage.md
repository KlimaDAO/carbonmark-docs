# Find carbon projects by vintage

You can search for carbon projects listed on Carbonmark by vintage.

{% hint style="info" %}
Each API endpoint will link to its reference documentation where you can create sample requests in other programming languages as well as enter the parameters you desire.
{% endhint %}

First, retrieve an array of the vintages of available carbon projects by calling the [`/vintages`](https://api.carbonmark.com/#/paths/vintages/get) endpoint. Here's a sample of a request:

```shell
curl --request GET \
  --url https://api.carbonmark.com/vintages \
  --header 'Accept: application/json'
```

You'll receive a JSON response containing an array with available vintages like this:

```json
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

Select your desired vintage(s). Next, we'll call the [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint with our desired query parameters.

In this example, we'll search for projects with a 2020 vintage. Here's the example request:

```bash
curl --request GET \
  --url 'https://api.carbonmark.com/carbonProjects?vintage=2020' \
  --header 'Accept: application/json'
```

The response you receive will contain a list of all carbon projects that fit your query parameters. Below is our example response for two projects:

```json
[
  {
    "key": "VCS-1418",
    "projectID": "1418",
    "name": "Renewable Energy Project By LNB Group",
    "methodologies": [
      {
        "id": "ACM0002",
        "category": "Renewable Energy",
        "name": "Grid-connected electricity generation from renewable sources"
      }
    ],
    "vintages": [
      "2020"
    ],
    "registry": "VCS",
    "updatedAt": "1717675704",
    "country": "India",
    "region": "Asia",
    "price": "1.3884013",
    "stats": {
      "totalBridged": 2,
      "totalRetired": 1.1895,
      "totalSupply": 0.8105,
      "totalListingsSupply": 0,
      "totalPoolsSupply": 0.8105
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "7",
      "8",
      "9",
      "13"
    ],
    "description": "The proposed project activity involves the installation of Wind and Solar Power Projects. The total installed capacity of the project is 22.20 MW; which involves operation of 18 Wind Turbine Generators (WTGs) with total capacity of 22.20 MW & 1 Solar power plants with total capacity of 5.00 MW located at different states; Rajasthan and Maharashtra in India.",
    "long_description": "The proposed project involves the installation of wind and solar power projects in India.\n\nKey Highlights:\n- The total installed capacity of the project is 22.20 MW.\n- The project includes 18 wind turbine generators with a total capacity of 22.20 MW.\n- The project also includes 1 solar power plant with a total capacity of 5.00 MW.\n- The wind and solar power projects are located in two different states, Rajasthan and Maharashtra.\n\nThis project is important as it will increase the production of clean energy in India, reducing the country's reliance on non-renewable energy sources. The use of wind and solar power will also contribute to the reduction of greenhouse gas emissions, helping to combat climate change. Additionally, the project will create job opportunities in the renewable energy industry, stimulating economic growth in the region.",
    "short_description": "This project involved the installation of Wind and Solar Power Projects across Rajasthan and Maharashtra in India. With a combined capacity of 27.20 MW, it includes 18 Wind Turbine Generators and 1 Solar power plant.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [71.35, 26.56]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1418",
    "images": [],
    "puroBatchTokenId": ""
  },
  {
    "key": "VCS-1497",
    "projectID": "1497",
    "name": "VCS Grouped Project For Renewable Power Generation By Essel Mining And Industries Limited",
    "methodologies": [
      {
        "id": "ACM0002",
        "category": "Renewable Energy",
        "name": "Grid-connected electricity generation from renewable sources"
      },
      {
        "id": "AMS-ID",
        "category": "Renewable Energy",
        "name": "Grid connected renewable electricity generation"
      },
      {
        "id": "AMS-IF",
        "category": "Other",
        "name": "Renewable electricity generation for captive use and mini-grid"
      }
    ],
    "vintages": [
      "2020",
      "2016"
    ],
    "registry": "VCS",
    "updatedAt": "1705288365",
    "country": "India",
    "region": "Asia",
    "price": "1.3884013",
    "stats": {
      "totalBridged": 942,
      "totalRetired": 0.772,
      "totalSupply": 941.228,
      "totalListingsSupply": 0,
      "totalPoolsSupply": 941.228
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "7",
      "8",
      "9",
      "13"
]
```

The information from this response such as "key" and "projectID" may be enough for your needs.

You can retrieve the full details of a carbon project by its using the project key; [`api.carbonmark.com/carbonProjects/{id}`](https://api.carbonmark.com/#/paths/carbonProjects-id/get). For example:

```sh
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-1418 \
  --header 'Accept: application/json'
```

The response will contain the full carbon project details:

```json
{
  "key": "VCS-1418",
  "projectID": "1418",
  "registry": "VCS",
  "country": "India",
  "name": "Renewable Energy Project By LNB Group",
  "methodologies": [
    {
      "id": "ACM0002",
      "category": "Renewable Energy",
      "name": "Grid-connected electricity generation from renewable sources"
    }
  ],
  "region": "Asia",
  "description": "The proposed project activity involves the installation of Wind and Solar Power Projects. The total installed capacity of the project is 22.20 MW; which involves operation of 18 Wind Turbine Generators (WTGs) with total capacity of 22.20 MW & 1 Solar power plants with total capacity of 5.00 MW located at different states; Rajasthan and Maharashtra in India.",
  "short_description": "This project involved the installation of Wind and Solar Power Projects across Rajasthan and Maharashtra in India. With a combined capacity of 27.20 MW, it includes 18 Wind Turbine Generators and 1 Solar power plant.",
  "long_description": "The proposed project involves the installation of wind and solar power projects in India.\n\nKey Highlights:\n- The total installed capacity of the project is 22.20 MW.\n- The project includes 18 wind turbine generators with a total capacity of 22.20 MW.\n- The project also includes 1 solar power plant with a total capacity of 5.00 MW.\n- The wind and solar power projects are located in two different states, Rajasthan and Maharashtra.\n\nThis project is important as it will increase the production of clean energy in India, reducing the country's reliance on non-renewable energy sources. The use of wind and solar power will also contribute to the reduction of greenhouse gas emissions, helping to combat climate change. Additionally, the project will create job opportunities in the renewable energy industry, stimulating economic growth in the region.",
  "url": "https://registry.verra.org/app/projectDetail/VCS/1418",
  "images": [],
  "location": {
    "type": "Feature",
    "geometry": {
      "type": "Point",
      "coordinates": [71.35, 26.56]
    }
  },
  "stats": {
    "totalBridged": 2,
    "totalRetired": 1.1895,
    "totalListingsSupply": 0,
    "totalPoolsSupply": 0.8105,
    "totalSupply": 0.8105
  },
  "puroBatchTokenId": "",
  "price": "1.3914992",
  "updatedAt": "1717675704",
  "hasSupply": true,
  "vintages": [
    "2020"
  ],
  "sustainableDevelopmentGoals": [
    "7",
    "8",
    "9",
    "13"
  ],
  "listings": []
}
```

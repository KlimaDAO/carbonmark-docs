# Find index products

You can retrieve the list of index products by calling the [`/products`](https://api.carbonmark.com/#/paths/products/get) endpoint. Currently within Carbonmark, Moss (MCO2) and Coorest (CCO2) are supported products.

Here is a sample of a request:

```sh
curl --request GET \
  --url https://api.carbonmark.com/products \
  --header 'Accept: application/json'
```

You'll receive a JSON response containing an array with product ids and prices that looks like this:

{% code overflow="wrap" %}
```json
[
  {
    "id": "mco2",
    "name": "Moss Carbon Credit",
    "short_description": "MOSS is an environmental platform that curates carbon projects within its index product, MCO2. The MCO2 product includes a variety of nature-based projects originating in the LatAm region. ",
    "long_description": "MOSS is an environmental platform simplifying carbon compensation by curating nature-based projects within its MCO2 index product. MCO2 tokens represent carbon mitigation efforts from projects in the LatAm region, particularly focusing on the Amazon rainforest.\n\nKey Highlights:\n- The Agrocortex project, part of MCO2, aims to prevent unplanned deforestation across 186,219 hectares of pristine rainforest.\n- Over 30 years, Agrocortex will help reduce over 14 million tonnes of CO2 emissions.\n- The project area is rich in biodiversity, with surveys identifying 345 bird species, representing 18% of Brazil's bird population.\n- MOSS's projects support both environmental and social co-benefits, promoting sustainable development.\n\nMOSS's MCO2 index enables businesses and individuals to offset their carbon footprint while contributing to meaningful conservation efforts in the Amazon, fostering long-term environmental and social impacts.",
    "sustainableDevelopmentGoals": [
      "13",
      "15"
    ],
    "assetPrices": [
      {
        "sourceId": "product-137-mco2",
        "type": "product",
        "purchasePrice": 0.6115928,
        "baseUnitPrice": 0.470456,
        "supply": 71602.76368849153,
        "minFillAmount": 0.001,
        "product": {
          "id": "mco2",
          "token": {
            "id": "0xaa7dbd1598251f856c12f63557a4c4397c253cea",
            "address": "0xaa7dbd1598251f856c12f63557a4c4397c253cea",
            "name": "Moss Earth : MCO2",
            "isExAnte": false,
            "symbol": "MCO2",
            "tokenId": null
          }
        }
      }
    ],
    "projectIds": [
      "VCS-875",
      "VCS-844",
      "VCS-1654",
      "VCS-1686",
      "VCS-1147",
      "VCS-332",
      "VCS-1811"
    ],
    "registry": "VCS",
    "url": "https://mco2token.moss.earth/",
    "coverImage": {
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/13d802cb67786545a69c9fb02e6305d5fe43991f-3044x1712.png",
      "caption": ""
    },
    
    ]
  },
  {
    "id": "cco2",
    "name": "Coorest Forestry Carbon Credit",
    "short_description": "Coorest is a blockchain platform that issues CCO2 tokens, representing 1 kg of CO2 captured by trees. Using satellite imagery and Chainlink oracles, it verifies carbon sequestration in real-time. NFTrees allow users to track absorption, with projects certified by UN auditors. Coorest supports reforestation and biodiversity while ensuring transparency.",
    "long_description": "Coorest is a blockchain-based platform simplifying carbon compensation by making it accessible, transparent, and verifiable. The platform issues CCO2 tokens, where each token represents one kilogram of CO2 captured by trees that adhere to the Coorest Carbon Standard. Using Chainlink's oracle and satellite imagery, Coorest ensures CCO2 tokens are only issued when corresponding biomass is sequestered. By tokenizing trees as NFTs (NFTrees), Coorest offers users the ability to track carbon absorption in real-time.\n\nKey Highlights:\n- Coorest Trees generate CO2 units, enabling long-term carbon offset.\n- Blockchain ensures immutable records of compensation.\n- Projects undergo strict onboarding and verification processes and are certified by United Nations-accredited auditors.\n- Supports global reforestation and biodiversity co-benefits.\n- Immediate carbon offset options are available via purchase.\n\nCoorest's approach to carbon offset markets fosters environmental responsibility and trust by integrating cutting-edge technology with global decarbonization goals.",
    "sustainableDevelopmentGoals": [
      "13"
    ],
    "assetPrices": [
      {
        "sourceId": "product-137-cco2",
        "type": "product",
        "purchasePrice": 20.662954,
        "baseUnitPrice": 15.89458,
        "supply": 2547.3618273871543,
        "minFillAmount": 0.001,
        "product": {
          "id": "cco2",
          "token": {
            "id": "0x82b37070e43c1ba0ea9e2283285b674ef7f1d4e2",
            "address": "0x82b37070e43c1ba0ea9e2283285b674ef7f1d4e2",
            "name": "Coorest : CCO2",
            "isExAnte": false,
            "symbol": "CCO2",
            "tokenId": null
          }
        }
      }
    ],
    "projectIds": [],
    "registry": "CCS",
    "url": "https://coorest.io/carbon-standard",
    "coverImage": {
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/1cf1581ae5659ffb69d289cd9332ca7d52f95ab9-2500x736.png",
      "caption": ""
    },
    ]
  }
]
```
{% endcode %}

You can retrieve the full details of a carbon index product by its using the project key; [`api.carbonmark.com/products/{id}`](https://api.carbonmark.com/#/paths/products-id/get). For example:

```bash
curl --request GET \
  --url https://api.carbonmark.com/products/cco2 \
  --header 'Accept: application/json'
```

The JSON response will look like the following:

{% code overflow="wrap" %}
```json
{
  "id": "cco2",
  "name": "Coorest Forestry Carbon Credit",
  "short_description": "Coorest is a blockchain platform that issues CCO2 tokens, representing 1 kg of CO2 captured by trees. Using satellite imagery and Chainlink oracles, it verifies carbon sequestration in real-time. NFTrees allow users to track absorption, with projects certified by UN auditors. Coorest supports reforestation and biodiversity while ensuring transparency.",
  "long_description": "Coorest is a blockchain-based platform simplifying carbon compensation by making it accessible, transparent, and verifiable. The platform issues CCO2 tokens, where each token represents one kilogram of CO2 captured by trees that adhere to the Coorest Carbon Standard. Using Chainlink's oracle and satellite imagery, Coorest ensures CCO2 tokens are only issued when corresponding biomass is sequestered. By tokenizing trees as NFTs (NFTrees), Coorest offers users the ability to track carbon absorption in real-time.\n\nKey Highlights:\n- Coorest Trees generate CO2 units, enabling long-term carbon offset.\n- Blockchain ensures immutable records of compensation.\n- Projects undergo strict onboarding and verification processes and are certified by United Nations-accredited auditors.\n- Supports global reforestation and biodiversity co-benefits.\n- Immediate carbon offset options are available via purchase.\n\nCoorest's approach to carbon offset markets fosters environmental responsibility and trust by integrating cutting-edge technology with global decarbonization goals.",
  "sustainableDevelopmentGoals": [
    "13"
  ],
  "assetPrices": [
    {
      "sourceId": "product-137-cco2",
      "type": "product",
      "purchasePrice": 19.1513699,
      "baseUnitPrice": 14.731823,
      "supply": 2547.36182738715,
      "minFillAmount": 0.001,
      "product": {
        "id": "cco2",
        "token": {
          "id": "0x82b37070e43c1ba0ea9e2283285b674ef7f1d4e2",
          "address": "0x82b37070e43c1ba0ea9e2283285b674ef7f1d4e2",
          "name": "Coorest : CCO2",
          "isExAnte": false,
          "symbol": "CCO2",
          "tokenId": null
        }
      }
    }
  ],
  "projectIds": [],
  "registry": "CCS",
  "url": "https://coorest.io/carbon-standard",
  "coverImage": {
    "url": "https://cdn.sanity.io/images/l6of5nwi/production/1cf1581ae5659ffb69d289cd9332ca7d52f95ab9-2500x736.png",
    "caption": ""
  }
}
```
{% endcode %}








# Find carbon projects by methodology category

You can search for carbon projects listed on Carbonmark by methodology category.

{% hint style="info" %}
Each API endpoint will link to its reference documentation where you can create sample requests in other programming languages as well as enter the parameters you desire.
{% endhint %}

The [`/categories`](https://api.carbonmark.com/#/paths/categories/get) endpoint provides a list of all methodology categories used to delineate every project in the marketplace.&#x20;

{% hint style="info" %}
A carbon project may belong to one or more methodology categories.
{% endhint %}

Here's a sample of a request:

```bash
curl --request GET \
  --url https://api.carbonmark.com/categories \
  --header 'Accept: application/json'
```

You'll receive a JSON response containing an array with category ids like this:

```json
[
  {
    "id": "Blue Carbon"
  },
  {
    "id": "Forestry"
  },
  {
    "id": "Other"
  },
  {
    "id": "Renewable Energy"
  },
  {
    "id": "Industrial Processing"
  },
  {
    "id": "Energy Efficiency"
  },
  {
    "id": "Other Nature Based"
  },
]
```

Select your methodology category. Next, we'll call the [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint with our desired query parameters.&#x20;

In this example, we'll search for carbon projects in the Forestry methodology category from Indonesia:

```sh
curl --request GET \
  --url 'https://api.carbonmark.com/carbonProjects?country=Indonesia&category=Forestry' \
  --header 'Accept: application/json'
```

The response you receive will contain a list of all carbon projects that fit your query parameters. Below we've listed an example response:

{% code overflow="wrap" %}
```json
[
  {
    "key": "VCS-674",
    "projectID": "674",
    "name": "Rimba Raya Biodiversity Reserve Project",
    "methodologies": [
      {
        "id": "VM0004",
        "category": "Forestry",
        "name": "Methodology for Conservation Projects that Avoid Planned Land Use Conversion in Peat Swamp Forests"
      }
    ],
    "vintages": [
      "2014"
    ],
    "registry": "VCS",
    "updatedAt": "1706418438",
    "country": "Indonesia",
    "region": "Oceania",
    "price": "1.1499488",
    "stats": {
      "totalBridged": 229030,
      "totalRetired": 35673.1271932669,
      "totalSupply": 190629.276094733,
      "totalListingsSupply": 0,
      "totalPoolsSupply": 190629.276094733
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "1",
      "2",
      "3",
      "4",
      "5",
      "6",
      "7",
      "8",
      "9",
      "10",
      "11",
      "12",
      "13",
      "14",
      "15",
      "16",
      "17"
    ],
    "description": "The Rimba Raya Biodiversity Reserve Project, an initiative by InfiniteEARTH, aims to reduce Indonesia’s emissions by preserving some 64,000 hectares of tropical peat swamp forest. This area, rich in biodiversity including the endangered Bornean orangutan, was slated by the Provincial government to be converted into four palm oil estates. Located on the southern coast of Borneo in the province of Central Kalimantan, the project is also designed to protect the integrity of the adjacent world-renowned Tanjung Puting National Park, by creating a physical buffer zone on the full extent of the ~90km eastern border of the park.",
    "long_description": "The Rimba Raya Biodiversity Reserve Project, a noble initiative by InfiniteEARTH, is committed to preserving Indonesia's tropical peat swamp forest and reducing the country's emissions. This forest spans over 64,000 hectares and is home to a diverse range of flora and fauna, including the endangered Bornean orangutan. The project was proposed to counter the Provincial government's plan of converting this area into four palm oil estates. The project is located on the south coast of Borneo in the province of Central Kalimantan and aims to create a physical buffer zone to protect the adjacent Tanjung Puting National Park, which is a world-renowned park.\n\nKey Highlights:\n- Preserves over 64,000 hectares of tropical peat swamp forest.\n- Protects the endangered Bornean orangutan and the rich biodiversity of the area.\n- Creates a physical buffer zone to safeguard the integrity of the Tanjung Puting National Park.\n \nThis project is significant as it not only preserves the rich biodiversity of the area but also helps in mitigating climate change by reducing emissions. The tropical peat swamp forest is known to store large amounts of carbon, and the preservation of this area will help in keeping the carbon locked in the soil, thus preventing it from being released into the atmosphere. The Rimba Raya Biodiversity Reserve Project is a perfect example of how environmental conservation can go hand-in-hand with sustainable development.",
    "short_description": "The Rimba Raya Biodiversity Reserve Project by InfiniteEARTH preserves 64,000 hectares of tropical peat swamp forest in Indonesia, protecting endangered species such as the Bornean orangutan and preventing the conversion of the area into palm oil estates. Located in Central Kalimantan, the project also safeguards the integrity of the adjacent Tanjung Puting National Park by creating a physical buffer zone on the park's eastern border.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [112.299244, -3.106367]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/674",
    "images": [
      {
        "url": "https://cdn.sanity.io/images/l6of5nwi/production/323445f147077f43496bfa2d39c2e9f56a449798-800x460.jpg",
        "caption": ""
      },
      {
        "url": "https://cdn.sanity.io/images/l6of5nwi/production/2e6a6dff56c7864924b06701c03f697551ce6db2-800x460.jpg",
        "caption": ""
      },
      {
        "url": "https://cdn.sanity.io/images/l6of5nwi/production/80bb85b9d9a8914c5e736d7c7328eb056f0a9dd5-800x460.jpg",
        "caption": ""
      },
      {
        "url": "https://cdn.sanity.io/images/l6of5nwi/production/d351f270d2bdb7d9ac7ba981bee079ced2620278-800x460.jpg",
        "caption": ""
      },
      {
        "url": "https://cdn.sanity.io/images/l6of5nwi/production/7eed7562b3fb79ebaf352615e49a18020baa99b1-800x460.jpg",
        "caption": ""
      },
      {
        "url": "https://cdn.sanity.io/images/l6of5nwi/production/730b52b15a99e2f68c526373dafcebc5da01d425-800x460.jpg",
        "caption": ""
      }
    ],
    "puroBatchTokenId": ""
  }
]
```
{% endcode %}

The information from this response such as "key" and "projectID" may be enough for your needs.

You can retrieve the full details of a carbon project by its using the project key; [`api.carbonmark.com/carbonProjects/{id}`](https://api.carbonmark.com/#/paths/carbonProjects-id/get). For example:

```sh
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-674 \
  --header 'Accept: application/json'
```

The response will contain the full carbon project details:

{% code overflow="wrap" %}
```json
{
  "key": "VCS-674",
  "projectID": "674",
  "registry": "VCS",
  "country": "Indonesia",
  "name": "Rimba Raya Biodiversity Reserve Project",
  "methodologies": [
    {
      "id": "VM0004",
      "category": "Forestry",
      "name": "Methodology for Conservation Projects that Avoid Planned Land Use Conversion in Peat Swamp Forests"
    }
  ],
  "region": "Oceania",
  "description": "The Rimba Raya Biodiversity Reserve Project, an initiative by InfiniteEARTH, aims to reduce Indonesia’s emissions by preserving some 64,000 hectares of tropical peat swamp forest. This area, rich in biodiversity including the endangered Bornean orangutan, was slated by the Provincial government to be converted into four palm oil estates. Located on the southern coast of Borneo in the province of Central Kalimantan, the project is also designed to protect the integrity of the adjacent world-renowned Tanjung Puting National Park, by creating a physical buffer zone on the full extent of the ~90km eastern border of the park.",
  "short_description": "The Rimba Raya Biodiversity Reserve Project by InfiniteEARTH preserves 64,000 hectares of tropical peat swamp forest in Indonesia, protecting endangered species such as the Bornean orangutan and preventing the conversion of the area into palm oil estates. Located in Central Kalimantan, the project also safeguards the integrity of the adjacent Tanjung Puting National Park by creating a physical buffer zone on the park's eastern border.",
  "long_description": "The Rimba Raya Biodiversity Reserve Project, a noble initiative by InfiniteEARTH, is committed to preserving Indonesia's tropical peat swamp forest and reducing the country's emissions. This forest spans over 64,000 hectares and is home to a diverse range of flora and fauna, including the endangered Bornean orangutan. The project was proposed to counter the Provincial government's plan of converting this area into four palm oil estates. The project is located on the south coast of Borneo in the province of Central Kalimantan and aims to create a physical buffer zone to protect the adjacent Tanjung Puting National Park, which is a world-renowned park.\n\nKey Highlights:\n- Preserves over 64,000 hectares of tropical peat swamp forest.\n- Protects the endangered Bornean orangutan and the rich biodiversity of the area.\n- Creates a physical buffer zone to safeguard the integrity of the Tanjung Puting National Park.\n \nThis project is significant as it not only preserves the rich biodiversity of the area but also helps in mitigating climate change by reducing emissions. The tropical peat swamp forest is known to store large amounts of carbon, and the preservation of this area will help in keeping the carbon locked in the soil, thus preventing it from being released into the atmosphere. The Rimba Raya Biodiversity Reserve Project is a perfect example of how environmental conservation can go hand-in-hand with sustainable development.",
  "url": "https://registry.verra.org/app/projectDetail/VCS/674",
  "images": [
    {
      "caption": "",
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/323445f147077f43496bfa2d39c2e9f56a449798-800x460.jpg"
    },
    {
      "caption": "",
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/2e6a6dff56c7864924b06701c03f697551ce6db2-800x460.jpg"
    },
    {
      "caption": "",
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/80bb85b9d9a8914c5e736d7c7328eb056f0a9dd5-800x460.jpg"
    },
    {
      "caption": "",
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/d351f270d2bdb7d9ac7ba981bee079ced2620278-800x460.jpg"
    },
    {
      "caption": "",
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/7eed7562b3fb79ebaf352615e49a18020baa99b1-800x460.jpg"
    },
    {
      "caption": "",
      "url": "https://cdn.sanity.io/images/l6of5nwi/production/730b52b15a99e2f68c526373dafcebc5da01d425-800x460.jpg"
    }
  ],
  "location": {
    "type": "Feature",
    "geometry": {
      "type": "Point",
      "coordinates": [112.299244, -3.106367]
    }
  },
  "stats": {
    "totalBridged": 229030,
    "totalRetired": 35673.1271932669,
    "totalListingsSupply": 0,
    "totalPoolsSupply": 190629.276094733,
    "totalSupply": 190629.276094733
  },
  "puroBatchTokenId": "",
  "price": "1.1499488",
  "updatedAt": "1706418438",
  "hasSupply": true,
  "vintages": [
    "2014"
  ],
  "sustainableDevelopmentGoals": [
    "1",
    "2",
    "3",
    "4",
    "5",
    "6",
    "7",
    "8",
    "9",
    "10",
    "11",
    "12",
    "13",
    "14",
    "15",
    "16",
    "17"
  ],
  "listings": []
}
```
{% endcode %}

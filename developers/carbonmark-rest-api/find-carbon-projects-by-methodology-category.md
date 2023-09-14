# Find carbon projects by methodology category

You can search for carbon projects listed on Carbonmark by methodology category.

{% hint style="info" %}
Note: a carbon project may belong to one or more methodology categories.
{% endhint %}

The [Categories endpoint](https://api.carbonmark.com/#/paths/categories/get) provides a list of all methodology categories used to delineate every project in the marketplace. Here's a sample of a request:

{% code title="Request" %}
```bash
curl --request GET \
  --url https://api.carbonmark.com/categories \
  --header 'Accept: application/json'
```
{% endcode %}

You'll receive a JSON response containing an array with category ids like this:

{% code title="JSON Response" %}
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
  {
    "id": "Agriculture"
  }
]
```
{% endcode %}

Select your desired methodology category. Next, we'll call the Projects endpoint with our desired query parameters. The [List Projects](https://api.carbonmark.com/#/paths/projects/get) endpoint allows you to retrieve an array of carbon projects filtered by category, country, vintage, project names, and project descriptions.&#x20;

In this example, we'll search for carbon projects in the Forestry methodology category from Indonesia:

{% code title="Request" %}
```sh
curl --request GET \
  --url 'https://api.carbonmark.com/projects?country=Indonesia&category=Forestry' \
  --header 'Accept: application/json'
```
{% endcode %}

The response you may receive will contain a list of all carbon projects that fit your query parameters. Below we've listed an example response:

{% code title="JSON Response" %}
```json
[
  {
    "description": "The Rimba Raya Biodiversity Reserve Project, an initiative by InfiniteEARTH, aims to reduce Indonesia’s emissions by preserving some 64,000 hectares of tropical peat swamp forest. This area, rich in biodiversity including the endangered Bornean orangutan, was slated by the Provincial government to be converted into four palm oil estates. Located on the southern coast of Borneo in the province of Central Kalimantan, the project is also designed to protect the integrity of the adjacent world-renowned Tanjung Puting National Park, by creating a physical buffer zone on the full extent of the ~90km eastern border of the park.",
    "short_description": "The Rimba Raya Biodiversity Reserve Project by InfiniteEARTH preserves 64,000 hectares of tropical peat swamp forest in Indonesia, protecting endangered species such as the Bornean orangutan and preventing the conversion of the area into palm oil estates. Located in Central Kalimantan, the project also safeguards the integrity of the adjacent Tanjung Puting National Park by creating a physical buffer zone on the park's eastern border.",
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
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          112.299244,
          -3.106367
        ]
      }
    },
    "vintage": "2014",
    "projectAddress": "0x62896f42cf1371b268db56e50d67c34f3eb1ad7a",
    "registry": "VCS",
    "updatedAt": "1694476791",
    "category": {
      "id": "Forestry"
    },
    "country": {
      "id": "Indonesia"
    },
    "price": "1.594209",
    "listings": null,
    "id": "0x62896f42cf1371b268db56e50d67c34f3eb1ad7a",
    "isPoolProject": true
  }
]
```
{% endcode %}

**The information from this response such as project address may be enough for your needs.**

To view the full details of a project, you can call the [Project Details](https://api.carbonmark.com/#/paths/projects-id/get) endpoint. Your request must combine the carbon project's key or projectID with the desired vintage year. For our example above, this would be VCS-674 and 2014 combined as **VCS-674-2014**. Here's the example request:

{% code title="Request" %}
```sh
curl --request GET \
  --url https://api.carbonmark.com/projects/VCS-674-2014 \
  --header 'Accept: application/json'
```
{% endcode %}

The response will contain the full carbon project details including available supply, listings, prices, and more:

{% code title="JSON Response" %}
```json
{"country":"Indonesia","description":"The Rimba Raya Biodiversity Reserve Project, an initiative by InfiniteEARTH, aims to reduce Indonesia’s emissions by preserving some 64,000 hectares of tropical peat swamp forest. This area, rich in biodiversity including the endangered Bornean orangutan, was slated by the Provincial government to be converted into four palm oil estates. Located on the southern coast of Borneo in the province of Central Kalimantan, the project is also designed to protect the integrity of the adjacent world-renowned Tanjung Puting National Park, by creating a physical buffer zone on the full extent of the ~90km eastern border of the park.","key":"VCS-674","registry":"VCS","url":"https://registry.verra.org/app/projectDetail/VCS/674","name":"Rimba Raya Biodiversity Reserve Project","methodologies":[{"id":"VM0004","category":"Forestry","name":"Methodology for Conservation Projects that Avoid Planned Land Use Conversion in Peat Swamp Forests"}],"long_description":"The Rimba Raya Biodiversity Reserve Project, a noble initiative by InfiniteEARTH, is committed to preserving Indonesia's tropical peat swamp forest and reducing the country's emissions. This forest spans over 64,000 hectares and is home to a diverse range of flora and fauna, including the endangered Bornean orangutan. The project was proposed to counter the Provincial government's plan of converting this area into four palm oil estates. The project is located on the south coast of Borneo in the province of Central Kalimantan and aims to create a physical buffer zone to protect the adjacent Tanjung Puting National Park, which is a world-renowned park.\n\nKey Highlights:\n- Preserves over 64,000 hectares of tropical peat swamp forest.\n- Protects the endangered Bornean orangutan and the rich biodiversity of the area.\n- Creates a physical buffer zone to safeguard the integrity of the Tanjung Puting National Park.\n \nThis project is significant as it not only preserves the rich biodiversity of the area but also helps in mitigating climate change by reducing emissions. The tropical peat swamp forest is known to store large amounts of carbon, and the preservation of this area will help in keeping the carbon locked in the soil, thus preventing it from being released into the atmosphere. The Rimba Raya Biodiversity Reserve Project is a perfect example of how environmental conservation can go hand-in-hand with sustainable development.","projectID":"674","location":{"type":"Feature","geometry":{"type":"Point","coordinates":[112.299244,-3.106367]}},"price":"1.594209","prices":[{"poolName":"nct","supply":"29190.797760819158","poolAddress":"0xD838290e877E0188a4A44700463419ED96c16107","isPoolDefault":false,"projectTokenAddress":"0x62896f42cf1371b268db56e50d67c34f3eb1ad7a","singleUnitPrice":"1.594209"}],"isPoolProject":true,"images":[{"caption":null,"url":"https://cdn.sanity.io/images/l6of5nwi/production/323445f147077f43496bfa2d39c2e9f56a449798-800x460.jpg"},{"caption":null,"url":"https://cdn.sanity.io/images/l6of5nwi/production/2e6a6dff56c7864924b06701c03f697551ce6db2-800x460.jpg"},{"caption":null,"url":"https://cdn.sanity.io/images/l6of5nwi/production/80bb85b9d9a8914c5e736d7c7328eb056f0a9dd5-800x460.jpg"},{"caption":null,"url":"https://cdn.sanity.io/images/l6of5nwi/production/d351f270d2bdb7d9ac7ba981bee079ced2620278-800x460.jpg"},{"caption":null,"url":"https://cdn.sanity.io/images/l6of5nwi/production/7eed7562b3fb79ebaf352615e49a18020baa99b1-800x460.jpg"},{"caption":null,"url":"https://cdn.sanity.io/images/l6of5nwi/production/730b52b15a99e2f68c526373dafcebc5da01d425-800x460.jpg"}],"activities":[{"id":"0x32874e203a76c4b627f3d88211f139338650572966d5298f97f5d369f7b4de55DeletedListing","amount":null,"previousAmount":null,"price":null,"previousPrice":null,"timeStamp":"1680034721","activityType":"DeletedListing","seller":{"id":"0xc0215dead8a5b4835545bfb668a065ca4b8cad6c"},"buyer":null},{"id":"0xeb400be18944305af4602568d2375e1511f8326437456efa8897da7e810f02c2CreatedListing","amount":"1.0","previousAmount":null,"price":"100.0","previousPrice":null,"timeStamp":"1680033933","activityType":"CreatedListing","seller":{"id":"0xc0215dead8a5b4835545bfb668a065ca4b8cad6c"},"buyer":null}],"listings":[],"vintage":"2014","stats":{"totalBridged":229030,"totalRetired":6106.922101380842,"totalSupply":218467.17085161916}}
```
{% endcode %}

# Find carbon projects by project name or description

You can search for carbon project names and descriptions for a string of text. This can be useful if you're looking for a specific project by name or want to find projects which contain specific keywords in their description.

The [List Projects](https://api.carbonmark.com/#/paths/projects/get) endpoint allows you to retrieve an array of carbon projects filtered by category, country, vintage, project names, and project descriptions. In this example, we'll search for carbon projects containing the string `REDD+`(encoded for the URL as `REDD%2B`):

{% code title="Request" %}
```sh
curl --request GET \
  --url 'https://api.carbonmark.com/projects?search=REDD%2B' \
  --header 'Accept: application/json'
```
{% endcode %}

The response you may receive will contain a list of all carbon projects that fit your query parameters. Below we've listed an example of two of the carbon projects from the response:

{% code title="JSON Response" %}
```json
[
  {
    "description": "The North Pikounda REDD+ Project (the Project) is a reducing emission from deforestation and degradation plus conservation and sustainable forestry (REDD+) project designed to protect 92,530 hectares (ha) of unlogged native Congolese forest, legally designated as a selective logging concession. The anticipated selective logging would have been undertaken on the dry lands, consisting on an area of 55,950 ha.   The main activity of the North Pikounda REDD+ Project is the cancelation of the planned degradation and deforestation activities and the decision to instead protect the forest area, while maintaining and protecting the biodiversity of the area.",
    "short_description": "The North Pikounda REDD+ Project protects 92,530 hectares of unlogged native Congolese forest from deforestation and degradation. The project cancels the planned selective logging and focuses on maintaining and protecting the biodiversity of the area.",
    "key": "VCS-1052",
    "projectID": "1052",
    "name": "North Pikounda REDD+",
    "methodologies": [
      {
        "id": "VM0011",
        "category": "Forestry",
        "name": "Methodology for Calculating GHG Benefits from Preventing Planned Degradation"
      }
    ],
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          16.181197,
          0.824273
        ]
      }
    },
    "vintage": "2012",
    "projectAddress": "0x463de2a5c6e8bb0c87f4aa80a02689e6680f72c7",
    "registry": "VCS",
    "updatedAt": "1693692945",
    "category": {
      "id": "Forestry"
    },
    "country": {
      "id": "Congo"
    },
    "price": "1.594209",
    "listings": null,
    "id": "0x463de2a5c6e8bb0c87f4aa80a02689e6680f72c7",
    "isPoolProject": true
  },
  {
    "description": "REDD Project to stop deforestation within private parcels amounting to  135, 105 Ha at the edge of the deforestation frontier in Brazil. The project will generate multiple climate, social, and biodiversity benefits.",
    "short_description": "This project aimed to stop deforestation within private parcels in Brazil, covering 135,105 hectares at the edge of the deforestation frontier. It generated various benefits for the climate, social welfare, and biodiversity.",
    "key": "VCS-981",
    "projectID": "981",
    "name": "Pacajai REDD+ Project",
    "methodologies": [
      {
        "id": "VM0015",
        "category": "Forestry",
        "name": "Methodology for Avoided Unplanned Deforestation"
      }
    ],
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -51.188205,
          -2.893955
        ]
      }
    },
    "vintage": "2012",
    "projectAddress": "0x65d96f0d45606016e30c97ee039775de9722a7d2",
    "registry": "VCS",
    "updatedAt": "1689693940",
    "category": {
      "id": "Forestry"
    },
    "country": {
      "id": "Brazil"
    },
    "price": "1.594209",
    "listings": null,
    "id": "0x65d96f0d45606016e30c97ee039775de9722a7d2",
    "isPoolProject": true
  }
]
```
{% endcode %}

**The information from this response such as project address may be enough for your needs.**

To view the full details of a project, you can call the [Project Details](https://api.carbonmark.com/#/paths/projects-id/get) endpoint. Your request must combine the carbon project's key or projectID with the desired vintage year. For our example above, this would be VCS-1052 and 2012 combined as **VCS-1052-2012**. Here's the example request:

{% code title="Request" %}
```sh
curl --request GET \
  --url https://api.carbonmark.com/projects/VCS-1052-2012 \
  --header 'Accept: application/json'
```
{% endcode %}

The response will contain the full carbon project details including available supply, listings, prices, and more:

{% code title="JSON Response" %}
```json
{"country":"Congo","description":"The North Pikounda REDD+ Project (the Project) is a reducing emission from deforestation and degradation plus conservation and sustainable forestry (REDD+) project designed to protect 92,530 hectares (ha) of unlogged native Congolese forest, legally designated as a selective logging concession. The anticipated selective logging would have been undertaken on the dry lands, consisting on an area of 55,950 ha.   The main activity of the North Pikounda REDD+ Project is the cancelation of the planned degradation and deforestation activities and the decision to instead protect the forest area, while maintaining and protecting the biodiversity of the area.","key":"VCS-1052","registry":"VCS","url":"https://registry.verra.org/app/projectDetail/VCS/1052","name":"North Pikounda REDD+","methodologies":[{"id":"VM0011","category":"Forestry","name":"Methodology for Calculating GHG Benefits from Preventing Planned Degradation"}],"long_description":"The North Pikounda REDD+ Project is a conservation and sustainable forestry initiative that aims to reduce emissions from deforestation and degradation. It covers an area of 92,530 hectares of unlogged native Congolese forest, legally designated as a selective logging concession. The project is a critical step in protecting the environment and preserving biodiversity in the region.\n\nKey Highlights:\n- The project is focused on canceling planned degradation and deforestation activities in the area.\n- It aims to protect 92,530 hectares of native forest and maintain the biodiversity of the region.\n- Selective logging was originally planned for the dry lands, which covers 55,950 hectares of the area.\n- The project is expected to reduce carbon emissions by preventing deforestation and degradation activities.\n\nThe North Pikounda REDD+ Project is essential in the fight against climate change. By protecting the forest area, the project not only preserves the biodiversity of the region but also prevents the release of carbon emissions into the atmosphere. The project is a prime example of how conservation and sustainable forestry practices can help combat global warming and its devastating effects.","projectID":"1052","location":{"type":"Feature","geometry":{"type":"Point","coordinates":[16.181197,0.824273]}},"price":"0.486285","prices":[{"poolName":"bct","supply":"0.04683854825459054","poolAddress":"0x2f800db0fdb5223b3c3f354886d907a671414a7f","isPoolDefault":false,"projectTokenAddress":"0x463de2a5c6e8bb0c87f4aa80a02689e6680f72c7","singleUnitPrice":"0.486285"},{"poolName":"nct","supply":"116.14738837478383","poolAddress":"0xD838290e877E0188a4A44700463419ED96c16107","isPoolDefault":false,"projectTokenAddress":"0x463de2a5c6e8bb0c87f4aa80a02689e6680f72c7","singleUnitPrice":"1.594209"}],"isPoolProject":true,"images":[],"activities":[{"id":"0xc72eb2eff8fd7d0bc3910e3457148ba066f3f148622a5e1fd626474212c4e608CreatedListing","amount":"1.0","previousAmount":null,"price":"3.0","previousPrice":null,"timeStamp":"1680255333","activityType":"CreatedListing","seller":{"id":"0x34100194ea0024b5bb8805c859089c6ba8a8ce5c"},"buyer":null}],"listings":[],"vintage":"2012","stats":{"totalBridged":55532,"totalRetired":15029.68507465425,"totalSupply":40052.65833134575}}
```
{% endcode %}

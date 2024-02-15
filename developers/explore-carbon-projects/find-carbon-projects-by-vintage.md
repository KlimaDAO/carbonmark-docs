# Find carbon projects by vintage

You can search for carbon projects listed on Carbonmark by vintage.

{% hint style="info" %}
Each API endpoint will link to its reference documentation where you can create sample requests in other programming languages as well as enter the parameters you desire.
{% endhint %}

First, retrieve an array of the vintages of available carbon projects by calling the [Vintages endpoint](https://api.carbonmark.com/#/paths/vintages/get). Here's a sample of a request:

{% code title="Request" %}
```shell
curl --request GET \
  --url https://api.carbonmark.com/vintages \
  --header 'Accept: application/json'
```
{% endcode %}

You'll receive a JSON response containing an array with available vintages like this:

{% code title="JSON Response" %}
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
{% endcode %}

Select your desired vintage. Next, we'll call the Projects endpoint with our desired query parameters. The [List Projects](https://api.carbonmark.com/#/paths/projects/get) endpoint allows you to retrieve an array of carbon projects filtered by category, country, vintage, project names, and project descriptions.

In this example, we'll search for forestry carbon projects with a 2017 vintage. Here's the example request:

{% code title="Request" %}
```bash
curl --request GET \
  --url 'https://api.carbonmark.com/projects?category=Forestry&vintage=2017' \
  --header 'Accept: application/json'
```
{% endcode %}

The response you may receive will contain a list of all carbon projects that fit your query parameters. Below is our example response:

{% code title="JSON Response" %}
```json
[
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
    "vintage": "2017",
    "projectAddress": "0xeaa9938076748d7edd4df0721b3e3fe4077349d3",
    "registry": "VCS",
    "updatedAt": "1687274584",
    "category": {
      "id": "Forestry"
    },
    "country": {
      "id": "Brazil"
    },
    "price": "1.594209",
    "listings": null,
    "id": "0xeaa9938076748d7edd4df0721b3e3fe4077349d3",
    "isPoolProject": true
  }
]
```
{% endcode %}

**The information from this response such as project address may be enough for your needs.**

To view the full details of a project, you can call the [Project Details](https://api.carbonmark.com/#/paths/projects-id/get) endpoint. Your request must combine the carbon project's key or projectID with the desired vintage year. For our example above, this would be VCS-981 and 2017 combined as **VCS-981-2017**. Here's the example request:

{% code title="Request" %}
```sh
curl --request GET \
  --url https://api.carbonmark.com/projects/VCS-981-2017 \
  --header 'Accept: application/json'
```
{% endcode %}

The response will contain the full carbon project details including available supply, listings, prices, and more:

{% code title="JSON Response" %}
```json
{"country":"Brazil","description":"REDD Project to stop deforestation within private parcels amounting to  135, 105 Ha at the edge of the deforestation frontier in Brazil. The project will generate multiple climate, social, and biodiversity benefits.","key":"VCS-981","registry":"VCS","url":"https://registry.verra.org/app/projectDetail/VCS/981","name":"Pacajai REDD+ Project","methodologies":[{"id":"VM0015","category":"Forestry","name":"Methodology for Avoided Unplanned Deforestation"}],"long_description":"A REDD Project to Halt Deforestation in Brazil\n\nIn the heart of Brazil, a REDD project has been implemented to prevent deforestation on private parcels of land spanning 135,105 hectares on the brink of the deforestation frontier. This project aims to produce various advantages for the environment, society, and biodiversity.\n\nKey Highlights:\n- The project is designed to protect over 135,000 hectares of land from deforestation, safeguarding the habitat of endangered species and preserving the natural beauty of the region.\n- By stopping deforestation, the project also prevents the release of carbon emissions into the atmosphere, thus mitigating the effects of climate change.\n- The implementation of this project has created job opportunities for the local community and fostered sustainable development in the region.\n\nThis REDD project is of critical importance as it tackles some of the biggest challenges facing Brazil and the world today. Deforestation has a significant impact on climate change, biodiversity loss, and the livelihoods of local communities. By preserving these private parcels of land, the project is not only protecting vital ecosystems but also contributing to the global effort to combat climate change.","projectID":"981","location":{"type":"Feature","geometry":{"type":"Point","coordinates":[-51.188205,-2.893955]}},"price":"0.816631","prices":[{"poolName":"nbo","supply":"26202.320013669174","poolAddress":"0x6BCa3B77C1909Ce1a4Ba1A20d1103bDe8d222E48","isPoolDefault":false,"projectTokenAddress":"0x7dbeebf8c2356ff8c53e41928c9575054a6f331b","singleUnitPrice":"0.816631"},{"poolName":"nct","supply":"1324.1371686595394","poolAddress":"0xD838290e877E0188a4A44700463419ED96c16107","isPoolDefault":false,"projectTokenAddress":"0x7dbeebf8c2356ff8c53e41928c9575054a6f331b","singleUnitPrice":"1.594209"}],"isPoolProject":true,"images":[],"activities":[{"id":"0x480b353fd6930cb073969e47d98dd2ec966efeabf9698c0deec0a300ce0c9dd0CreatedListing","amount":"8.325","previousAmount":null,"price":"1.18","previousPrice":null,"timeStamp":"1686100816","activityType":"CreatedListing","seller":{"id":"0x6973fb6c01d32fba636be2c8a25aa7ef6baa1731"},"buyer":null},{"id":"0x4e399ba3c952377af8384cf5489095d37b3c73ebc30762e1d62da8b6c8a6a0f9Purchase","amount":"1.0","previousAmount":null,"price":"3.0","previousPrice":null,"timeStamp":"1685106207","activityType":"Purchase","seller":{"id":"0xacfa271476d0f6999f9c8fa44c29f17dba7a07fe"},"buyer":{"id":"0x6476edf98bdc5ee62f9d4e4bf8932d69ed685483"}},{"id":"0x2aeea9afc5f6e2cb58c830d49ffbd4d2d5e74dc32aa76dd1925563fa4303e604CreatedListing","amount":"8.0","previousAmount":null,"price":"1.6","previousPrice":null,"timeStamp":"1684841758","activityType":"CreatedListing","seller":{"id":"0x2b5b488c766586f0b170333694bf1fe2b703748f"},"buyer":null},{"id":"0xb639316b551d717e001973cd47b040c6c97cbb4e6b26b46891a5d64e2edc67a2DeletedListing","amount":null,"previousAmount":null,"price":null,"previousPrice":null,"timeStamp":"1682888596","activityType":"DeletedListing","seller":{"id":"0x2b5b488c766586f0b170333694bf1fe2b703748f"},"buyer":null},{"id":"0x5e9720e7e5dd2d69ebdae858bf90555ccfe4757aadc7782c0633d9c1697d5c10CreatedListing","amount":"8.0","previousAmount":null,"price":"2.0","previousPrice":null,"timeStamp":"1682688533","activityType":"CreatedListing","seller":{"id":"0x2b5b488c766586f0b170333694bf1fe2b703748f"},"buyer":null},{"id":"0x5c21e8fef2719b8f065e89555117fb8f2d14e65faa039c4fd5a5ad50f945bb84Purchase","amount":"1.0","previousAmount":null,"price":"3.0","previousPrice":null,"timeStamp":"1681926090","activityType":"Purchase","seller":{"id":"0xacfa271476d0f6999f9c8fa44c29f17dba7a07fe"},"buyer":{"id":"0x6476edf98bdc5ee62f9d4e4bf8932d69ed685483"}},{"id":"0x029171e73726f6484f0e77e2b679f3c923f14d024e8df1c1152d5fe6d6e20892CreatedListing","amount":"34.0","previousAmount":null,"price":"2.1","previousPrice":null,"timeStamp":"1681083124","activityType":"CreatedListing","seller":{"id":"0x6c4bff4b6a355ec56e7945682eebc973558572a9"},"buyer":null},{"id":"0x51a4999488f68edc0ad4c30aedd4392ad02f7acc306faacaea004ffa0073b79cCreatedListing","amount":"2.0","previousAmount":null,"price":"3.0","previousPrice":null,"timeStamp":"1680625116","activityType":"CreatedListing","seller":{"id":"0xacfa271476d0f6999f9c8fa44c29f17dba7a07fe"},"buyer":null}],"listings":[],"vintage":"2017","stats":{"totalBridged":102697,"totalRetired":9355.452403117048,"totalSupply":89133.76759688296}}
```
{% endcode %}

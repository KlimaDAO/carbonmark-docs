# Find carbon projects by country

You can search for carbon projects listed on Carbonmark by country.

{% hint style="info" %}
Each API endpoint will link to its reference documentation where you can create sample requests in other programming languages as well as enter the parameters you desire.
{% endhint %}

First, retrieve an array containing the countries that carbon projects originate from by calling the [Countries endpoint](https://api.carbonmark.com/#/paths/countries/get). Here's a sample of a request:

{% code title="Request" %}
```sh
curl --request GET \
  --url https://api.carbonmark.com/countries \
  --header 'Accept: application/json'
```
{% endcode %}

You'll receive a JSON response containing an array with country ids that looks like this:

{% code title="JSON Response" %}
```json
[
  {
    "id": "Brazil"
  },
  {
    "id": "Bulgaria"
  },
  {
    "id": "China"
  },
  {
    "id": "Congo"
  },
  {
    "id": "Ecuador"
  },
  {
    "id": "India"
  },
  {
]
```
{% endcode %}

From these countries, select your choice of country. Next, we'll call the Projects endpoint with our desired query parameters. The [List Projects](https://api.carbonmark.com/#/paths/projects/get) endpoint allows you to retrieve an array of carbon projects filtered by category, country, vintage, project names, and project descriptions.&#x20;

In this example, we'll search for renewable energy carbon projects originating from India with a 2012 vintage. Here's the example request:

{% code title="Request" %}
```sh
curl --request GET \
  --url 'https://api.carbonmark.com/projects?country=India&category=Renewable+Energy&vintage=2012' \
  --header 'Accept: application/json'
```
{% endcode %}

The response you may receive will contain a list of all carbon projects that fit your query parameters. Below we've listed an example of two of the carbon projects from the response:

{% code title="JSON Response" %}
```json
[
  {
    "description": "The project activity consists of the construction of small hydro project, the total installed capacity being 4.95 MW to generate clean electricity using the energy of the flowing stream. The project is a run-of- the- river type with minimum environmental impacts. The technology or electricity generation process using hydro resources is converting the potential energy available in the stream flowing from higher altitudes into mechanical energy using hydro turbines and then to electrical energy using alternators. The project activity will provide and sell electricity to the state electricity utility (Uttarakhand Power Corporation Limited (UPCL)) through NEWNE grid, thus reducing dependence on fossil fuels and thereby reducing CO2 emissions. Electricity shall be evacuated through 33 KV line to the nearest 33 KV sub-station at Hanuman Chatti Sub Station.",
    "short_description": "This project involves the construction of a small hydro facility with a 4.95 MW capacity, generating clean electricity from a flowing stream. The project uses run-of-the-river technology with minimal environmental impact, converting potential energy into mechanical and then electrical energy. By providing and selling electricity to the state electricity utility, this project reduces dependence on fossil fuels and CO2 emissions.",
    "key": "VCS-274",
    "projectID": "274",
    "name": "Hanuman Ganga Hydro (4.95 MW) Plant At Uttarakhand",
    "methodologies": [
      {
        "id": "AMS-ID",
        "category": "Renewable Energy",
        "name": "Grid connected renewable electricity generation"
      }
    ],
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          78.405556,
          30.930556
        ]
      }
    },
    "vintage": "2012",
    "projectAddress": "0x0970d3b3bc5046b315f7cfa5108ddb2460d8d0c8",
    "registry": "VCS",
    "updatedAt": "1693229316",
    "category": {
      "id": "Renewable Energy"
    },
    "country": {
      "id": "India"
    },
    "price": "0.485202",
    "listings": null,
    "id": "0x0970d3b3bc5046b315f7cfa5108ddb2460d8d0c8",
    "isPoolProject": true
  },
  {
    "description": "Mytrah Vayu (Pennar) Private Limited (MVPPL) has set up 63 MW wind power project in the state of Andhra Pradesh in India. The project activity comprises of 30 number Wind Turbine Generators (WTG’s) with a capacity of 2.1 MW each.The technology is indigenous and no technology transfer is taking place. The project activity is a zero emissions wind based power generation project. The technology doesn’t involve any fossil fuel usage and hence there are no emissions associated with the project. The S88 model WEGs are supplied by Suzlon Energy Limited (SEL), a subsidiary of the Suzlon group.",
    "short_description": "Mytrah Vayu (Pennar) Private Limited set up a 63 MW wind power project in Andhra Pradesh, India. The project consists of 30 Wind Turbine Generators, each with a capacity of 2.1 MW, and uses Suzlon Energy Limited's indigenous S88 model WEGs, emitting zero emissions as it does not involve any fossil fuel usage or technology transfer.",
    "key": "VCS-1214",
    "projectID": "1214",
    "name": "Vajrakarur Wind Power Project In Andhra Pradesh",
    "methodologies": [
      {
        "id": "ACM0002",
        "category": "Renewable Energy",
        "name": "Grid-connected electricity generation from renewable sources"
      }
    ],
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          77.291387,
          15.00111
        ]
      }
    },
    "vintage": "2012",
    "projectAddress": "0x13f27510cbdc287bf02f3eff604d3624553c73c4",
    "registry": "VCS",
    "updatedAt": "1635936940",
    "category": {
      "id": "Renewable Energy"
    },
    "country": {
      "id": "India"
    },
    "price": "0.485202",
    "listings": null,
    "id": "0x13f27510cbdc287bf02f3eff604d3624553c73c4",
    "isPoolProject": true
  },
]
```
{% endcode %}

**The information from this response such as project address may be enough for your needs.**

To view the full details of a project, you can call the [Project Details](https://api.carbonmark.com/#/paths/projects-id/get) endpoint. Your request must combine the carbon project's key or projectID with the desired vintage year. For our example above, this would be VCS-274 and 2012 combined as **VCS-274-2012.** Here's the example request:

{% code title="Request" %}
```shell
curl --request GET \
  --url https://api.carbonmark.com/projects/VCS-274-2012 \
  --header 'Accept: application/json'
```
{% endcode %}

The response will contain the full carbon project details including available supply, listings, prices, and more:

{% code title="JSON Response" %}
```json
{"country":"India","description":"The project activity consists of the construction of small hydro project, the total installed capacity being 4.95 MW to generate clean electricity using the energy of the flowing stream. The project is a run-of- the- river type with minimum environmental impacts. The technology or electricity generation process using hydro resources is converting the potential energy available in the stream flowing from higher altitudes into mechanical energy using hydro turbines and then to electrical energy using alternators. The project activity will provide and sell electricity to the state electricity utility (Uttarakhand Power Corporation Limited (UPCL)) through NEWNE grid, thus reducing dependence on fossil fuels and thereby reducing CO2 emissions. Electricity shall be evacuated through 33 KV line to the nearest 33 KV sub-station at Hanuman Chatti Sub Station.","key":"VCS-274","registry":"VCS","url":"https://registry.verra.org/app/projectDetail/VCS/274","name":"Hanuman Ganga Hydro (4.95 MW) Plant At Uttarakhand","methodologies":[{"id":"AMS-ID","category":"Renewable Energy","name":"Grid connected renewable electricity generation"}],"long_description":"This carbon project involves the construction of a small hydro project with a total installed capacity of 4.95 MW to generate clean electricity using the energy of a flowing stream. The project is a run-of-the-river type, which has a minimal environmental impact. The electricity generation process using hydro resources converts the potential energy available in the stream flowing from higher altitudes into mechanical energy using hydro turbines, and then into electrical energy using alternators. \n\nKey Highlights:\n- The project will provide and sell electricity to the state electricity utility, Uttarakhand Power Corporation Limited (UPCL), through NEWNE grid.\n- The project activity reduces dependence on fossil fuels and thereby reduces CO2 emissions.\n- The electricity will be evacuated through a 33 KV line to the nearest 33 KV sub-station at Hanuman Chatti Sub Station.\n\nThis project is important because it provides a renewable source of energy that does not harm the environment. Additionally, this project reduces the reliance on fossil fuels and helps to decrease CO2 emissions. It is an excellent example of how renewable energy can be harnessed to generate electricity in an environmentally sustainable way.","projectID":"274","location":{"type":"Feature","geometry":{"type":"Point","coordinates":[78.405556,30.930556]}},"price":"0.485202","prices":[{"poolName":"bct","supply":"7439.9999","poolAddress":"0x2f800db0fdb5223b3c3f354886d907a671414a7f","isPoolDefault":false,"projectTokenAddress":"0x0970d3b3bc5046b315f7cfa5108ddb2460d8d0c8","singleUnitPrice":"0.485202"}],"isPoolProject":true,"images":[],"activities":[],"listings":[],"vintage":"2012","stats":{"totalBridged":7442,"totalRetired":2.0001,"totalSupply":7439.9999}}
```
{% endcode %}


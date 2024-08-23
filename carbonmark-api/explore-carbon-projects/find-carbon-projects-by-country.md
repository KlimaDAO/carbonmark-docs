# Find carbon projects by country

You can search for carbon projects listed on Carbonmark by country.

{% hint style="info" %}
Each API endpoint will link to its reference documentation where you can create sample requests in other programming languages as well as enter the parameters you desire.
{% endhint %}

First, retrieve an array containing the countries that carbon projects originate from by calling the [`/Countries`](https://api.carbonmark.com/#/paths/countries/get) endpoint. Here's a sample of a request:

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

From these countries, select your choice of country. Next, we'll call the [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint with our desired query parameters. The endpoint allows you to retrieve an array of carbon projects filtered by category, country, vintage, project names, and project descriptions.&#x20;

In this example, we'll search for renewable energy carbon projects originating from India with a 2012 vintage. Here's the example request:

{% code title="Request" %}
```sh
curl --request GET \
  --url 'https://api.carbonmark.com/carbonProjects?country=India&category=Renewable+Energy&vintage=2012' \
  --header 'Accept: application/json'
```
{% endcode %}

The response you may receive will contain a list of all carbon projects that match your query parameters. Below we've listed an example of two of the carbon projects from the response:

{% code title="JSON Response" %}
```json
[
  {
    "key": "VCS-1160",
    "projectID": "1160",
    "name": "6.5 MW Cogeneration Project In Akbarpur, Punjab",
    "methodologies": [
      {
        "id": "AMS-IC",
        "category": "Renewable Energy",
        "name": "Thermal energy production with or without electricity"
      }
    ],
    "vintages": [
      "2014",
      "2012",
      "2013"
    ],
    "registry": "VCS",
    "updatedAt": "1705288365",
    "country": "India",
    "region": "Asia",
    "price": "1.3441532",
    "stats": {
      "totalBridged": 22396,
      "totalRetired": 0.01,
      "totalSupply": 22395.99,
      "totalListingsSupply": 0,
      "totalPoolsSupply": 22395.99
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "7",
      "8",
      "9",
      "13"
    ],
    "description": "The project activity is being implemented in the Sangrur district in Punjab at the textile unit of Gillanders Arbuthnot & Co. Ltd (hereafter known as GACL). The proposed project activity involves installation of a cogeneration plant comprising of one rice husk fired AFBC boiler with steam generation capacity of 34 TPH at 66 kg/cm2 (g) pressure and 495° C temperature and a 6.5 MW multistage extraction cum condensing steam turbine generator.",
    "long_description": "Located in the Sangrur district of Punjab, the Gillanders Arbuthnot & Co. Ltd (GACL) textile unit is implementing a project activity that will help reduce their carbon footprint. The project involves the installation of a cogeneration plant, which utilizes rice husks as fuel to generate steam and electricity.\n\nKey Highlights:\n- The cogeneration plant includes a rice husk fired AFBC boiler with a steam generation capacity of 34 TPH at 66 kg/cm2 (g) pressure and 495° C temperature.\n- Additionally, a 6.5 MW multistage extraction cum condensing steam turbine generator will be installed to convert the steam into electricity.\n- The project aims to reduce greenhouse gas emissions by replacing the use of fossil fuels with renewable energy sources.\n- By generating electricity on-site, the project will also reduce the unit's reliance on grid electricity.\n- The project will create job opportunities both during construction and operation phases.\n\nThis project is important because it demonstrates the potential for businesses to transition to more sustainable energy sources. By utilizing rice husks, a renewable waste product, as fuel, the project will help reduce greenhouse gas emissions and promote sustainable practices in the textile industry. In addition to its environmental benefits, the project will also create job opportunities and reduce the unit's reliance on grid electricity.",
    "short_description": "A cogeneration plant was installed at the textile unit of Gillanders Arbuthnot & Co. Ltd in the Sangrur district of Punjab. This plant includes a rice husk fired AFBC boiler and a multi-stage extraction cum condensing steam turbine generator, with the capacity to generate 34 TPH of steam at 66 kg/cm2 pressure and 495° C temperature, and 6.5 MW of electricity.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [75.366664, 31.5]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1160",
    "images": [],
    "puroBatchTokenId": ""
  },
  {
    "key": "VCS-1203",
    "projectID": "1203",
    "name": "Suzlon 8.40 MW Wind Power Project",
    "methodologies": [
      {
        "id": "AMS-ID",
        "category": "Renewable Energy",
        "name": "Grid connected renewable electricity generation"
      }
    ],
    "vintages": [
      "2011",
      "2012"
    ],
    "registry": "VCS",
    "updatedAt": "1705288365",
    "country": "India",
    "region": "Africa",
    "price": "1.3441532",
    "stats": {
      "totalBridged": 2112,
      "totalRetired": 0,
      "totalSupply": 2112,
      "totalListingsSupply": 0,
      "totalPoolsSupply": 2112
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "7",
      "8",
      "9",
      "13"
    ],
    "description": "Kishangarh Hi-tech Textile Park Ltd. (KHTPL) has awarded a contract for 8.4 MW wind power project to world’s third leading and India’s largest wind turbines manufacturer, Suzlon Energy Limited (SEL). Kishangarh Hi-tech Textile Park Ltd.(KHTPL) will sign-up an Energy Wheeling Agreement (EWA) with Rajasthan Rajya Vidyut Prasaran Nigam Limited and the power generated will be wheeled for utilization at KHTPL’s state-of-the-art hi-tech integrated textile park being developed at Kishangarh (Dist. Ajmer) in Rajasthan.  The project was registered at UNFCCC (Ref. No. 7804) on 19-October-2012 and the details of the same can be viewed on   https://cdm.unfccc.int/Projects/DB/LRQA%20Ltd1350651308.1/view.",
    "long_description": "Kishangarh Hi-tech Textile Park Ltd. (KHTPL) has taken a giant leap towards sustainable energy by contracting Suzlon Energy Limited (SEL), the world's third-largest and India's largest wind turbine manufacturer, to install an 8.4 MW wind power project. This project is part of KHTPL's commitment to promote renewable energy sources and reduce greenhouse gas emissions. \n\nKey Highlights:\n- SEL, the world’s third-largest and India’s largest wind turbines manufacturer, has been awarded the contract for 8.4 MW wind power project.\n- An Energy Wheeling Agreement (EWA) will be signed between KHTPL and Rajasthan Rajya Vidyut Prasaran Nigam Limited to utilize the power generated at the hi-tech integrated textile park being developed at Kishangarh (Dist. Ajmer) in Rajasthan.\n- The project was registered at UNFCCC (Ref. No. 7804) on 19-October-2012.\n- The details of the project can be viewed on https://cdm.unfccc.int/Projects/DB/LRQA%20Ltd1350651308.1/view.\n- The wind power project is expected to significantly reduce greenhouse gas emissions and promote the usage of renewable energy sources.\n\nBy utilizing wind power, KHTPL is leading the way in reducing their carbon footprint and promoting sustainable energy practices. This project not only benefits the environment but also promotes economic development in Rajasthan. The Energy Wheeling Agreement ensures that the power generated by the wind turbines will be used efficiently, making the project a win-win for both the local community and the environment.",
    "short_description": "Kishangarh Hi-tech Textile Park Ltd. partnered with Suzlon Energy Limited to construct an 8.4 MW wind power project. The energy generated will be utilized at their state-of-the-art integrated textile park in Rajasthan. This project was registered with the UNFCCC in 2012.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [70.636386, 27.179444]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1203",
    "images": [],
    "puroBatchTokenId": ""
  }
]
```
{% endcode %}

**The information from this response such as "key" and "projectID" may be enough for your needs.**

You can retrieve the full details of a carbon project by its using the project key; [`api.carbonmark.com/carbonProjects/{id}`](https://api.carbonmark.com/#/paths/carbonProjects-id/get). For example:

{% code title="Request" %}
```shell
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-274 \
  --header 'Accept: application/json'
```
{% endcode %}

The response will contain the full carbon project details:

{% code title="JSON Response" %}
```json
{
  "key": "VCS-274",
  "projectID": "274",
  "registry": "VCS",
  "country": "India",
  "name": "Hanuman Ganga Hydro (4.95 MW) Plant At Uttarakhand",
  "methodologies": [
    {
      "id": "AMS-ID",
      "category": "Renewable Energy",
      "name": "Grid connected renewable electricity generation"
    }
  ],
  "region": "Asia",
  "description": "The project activity consists of the construction of small hydro project, the total installed capacity being 4.95 MW to generate clean electricity using the energy of the flowing stream. The project is a run-of- the- river type with minimum environmental impacts. The technology or electricity generation process using hydro resources is converting the potential energy available in the stream flowing from higher altitudes into mechanical energy using hydro turbines and then to electrical energy using alternators. The project activity will provide and sell electricity to the state electricity utility (Uttarakhand Power Corporation Limited (UPCL)) through NEWNE grid, thus reducing dependence on fossil fuels and thereby reducing CO2 emissions. Electricity shall be evacuated through 33 KV line to the nearest 33 KV sub-station at Hanuman Chatti Sub Station.",
  "short_description": "This project involves the construction of a small hydro facility with a 4.95 MW capacity, generating clean electricity from a flowing stream. The project uses run-of-the-river technology with minimal environmental impact, converting potential energy into mechanical and then electrical energy. By providing and selling electricity to the state electricity utility, this project reduces dependence on fossil fuels and CO2 emissions.",
  "long_description": "This carbon project involves the construction of a small hydro project with a total installed capacity of 4.95 MW to generate clean electricity using the energy of a flowing stream. The project is a run-of-the-river type, which has a minimal environmental impact. The electricity generation process using hydro resources converts the potential energy available in the stream flowing from higher altitudes into mechanical energy using hydro turbines, and then into electrical energy using alternators. \n\nKey Highlights:\n- The project will provide and sell electricity to the state electricity utility, Uttarakhand Power Corporation Limited (UPCL), through NEWNE grid.\n- The project activity reduces dependence on fossil fuels and thereby reduces CO2 emissions.\n- The electricity will be evacuated through a 33 KV line to the nearest 33 KV sub-station at Hanuman Chatti Sub Station.\n\nThis project is important because it provides a renewable source of energy that does not harm the environment. Additionally, this project reduces the reliance on fossil fuels and helps to decrease CO2 emissions. It is an excellent example of how renewable energy can be harnessed to generate electricity in an environmentally sustainable way.",
  "url": "https://registry.verra.org/app/projectDetail/VCS/274",
  "images": [],
  "location": {
    "type": "Feature",
    "geometry": {
      "type": "Point",
      "coordinates": [78.405556, 30.930556]
    }
  },
  "stats": {
    "totalBridged": 39492,
    "totalRetired": 2.0021,
    "totalListingsSupply": 0,
    "totalPoolsSupply": 39489.9979,
    "totalSupply": 39489.9979
  },
  "puroBatchTokenId": "",
  "price": "1.3429052",
  "updatedAt": "1705288365",
  "hasSupply": true,
  "vintages": [
    "2015",
    "2011",
    "2012",
    "2014",
    "2016",
    "2010"
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
{% endcode %}

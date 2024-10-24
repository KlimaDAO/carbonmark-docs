# Find carbon projects by project name or description

You can search for carbon project names and descriptions for a string of text. This can be useful if you're looking for a specific project by name or want to find projects which contain specific keywords in their description.

{% hint style="info" %}
Each API endpoint will link to its reference documentation where you can create sample requests in other programming languages as well as enter the parameters you desire.
{% endhint %}

The [`/carbonProjects`](https://api.carbonmark.com/#/paths/carbonProjects/get) endpoint allows you to retrieve an array of carbon projects filtered by various query parameters, including text based searches. In this example, we'll search for carbon projects containing the string `REDD+` (encoded for the URL as `REDD%2B`):

```sh
curl --request GET \
  --url 'https://api.carbonmark.com/carbonProjects?search=REDD%2B' \
  --header 'Accept: application/json'
```

The response you receive will contain a list of all carbon projects that fit your query parameters. For example:

{% code overflow="wrap" %}
```json
[
  {
    "key": "VCS-1566",
    "projectID": "1566",
    "name": "REDD+ Project Resguardo Indigena Unificado Selva De Mataven (Riu Sm)",
    "methodologies": [
      {
        "id": "VM0007",
        "category": "Forestry",
        "name": "REDD+ Methodology Framework (REDD+MF)"
      }
    ],
    "vintages": [
      "2019"
    ],
    "registry": "VCS",
    "updatedAt": "1721548741",
    "country": "Colombia",
    "region": "Latin America",
    "price": "1.0985",
    "stats": {
      "totalBridged": 45000,
      "totalRetired": 1755.6449766527,
      "totalSupply": 5660.0370333473,
      "totalListingsSupply": 10,
      "totalPoolsSupply": 5650.0370333473
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "13",
      "15"
    ],
    "description": "REDD+ Project Resguardo Indígena Unificado–Selva de Mataven (REDD+ RIU-SM) aims to develop a participatory process to achieve the establishment of an integrated management system of forests and lands of the reserve, to ensure its sustainability and to mitigate threats of its conservation, particularly avoiding deforestation through the implementation of a REDD+ Project (Reducing Emissions from Deforestation and Forest Degradation + conserving carbon stocks, sustainable management of forests and enhancement of forest reserves in developing countries) that allows providing compensation payments for ecosystem services. The technology corresponds to a REDD Project in accordance with standards established by the VCS. Specifically an activity “Avoiding Unplanned Deforestation and Degradation (AUDD)”. The Indigenous Reservation is located east of the high plain Orinoco Colombian region in the transition belt between the savannas of the Orinoco and the Amazon forests, in Department of Vichada",
    "long_description": "The REDD+ Project Resguardo Indígena Unificado–Selva de Mataven (REDD+ RIU-SM) is a participatory project that aims to establish an integrated management system of forests and lands in the Indigenous Reservation of Selva de Mataven. This project ensures the sustainability of the reserve and helps mitigate threats to its conservation, including deforestation. The implementation of the REDD+ Project (Reducing Emissions from Deforestation and Forest Degradation + conserving carbon stocks, sustainable management of forests and enhancement of forest reserves in developing countries) provides compensation payments for ecosystem services. \n\nKey Highlights:\n- The project uses a REDD+ Project in accordance with the standards established by the VCS.\n- The project focuses on avoiding unplanned deforestation and degradation (AUDD).\n- The Indigenous Reservation is located in the transition belt between the savannas of the Orinoco and the Amazon forests in the Department of Vichada.\n- The project is participatory, ensuring the involvement of local communities in the management of the reserve. \n\nThe REDD+ RIU-SM project is of great importance because it helps to preserve the Indigenous Reservation of Selva de Mataven, which is a crucial ecosystem in the transition belt between the Orinoco savannas and the Amazon forests. Deforestation in this area would result in the loss of biodiversity and carbon sequestration potential. The project ensures that the Indigenous Reservation is managed sustainably and provides compensation for ecosystem services, which benefits the local communities who depend on the forest for their livelihoods.",
    "short_description": "The REDD+ Project Resguardo Indígena Unificado-Selva de Mataven (REDD+ RIU-SM) is a participatory project that aims to establish an integrated management system for forests and lands, ensuring their sustainability and mitigating threats to conservation. The project uses the REDD+ approach to provide compensation payments for ecosystem services, specifically for 'Avoiding Unplanned Deforestation and Degradation (AUDD)' in the Indigenous Reservation of Vichada, Colombia. The project adheres to the standards established by the VCS.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [-68.393333, 4.244444]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1566",
    "images": [],
    "puroBatchTokenId": ""
  },
  {
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
    "vintages": [
      "2012"
    ],
    "registry": "VCS",
    "updatedAt": "1706418438",
    "country": "Congo",
    "region": "Africa",
    "price": "1.1499488",
    "stats": {
      "totalBridged": 55532,
      "totalRetired": 15033.8937675473,
      "totalSupply": 40496.6062324527,
      "totalListingsSupply": 0,
      "totalPoolsSupply": 40496.6062324527
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "13",
      "15"
    ],
    "description": "The North Pikounda REDD+ Project (the Project) is a reducing emission from deforestation and degradation plus conservation and sustainable forestry (REDD+) project designed to protect 92,530 hectares (ha) of unlogged native Congolese forest, legally designated as a selective logging concession. The anticipated selective logging would have been undertaken on the dry lands, consisting on an area of 55,950 ha.   The main activity of the North Pikounda REDD+ Project is the cancelation of the planned degradation and deforestation activities and the decision to instead protect the forest area, while maintaining and protecting the biodiversity of the area.",
    "long_description": "The North Pikounda REDD+ Project is a conservation and sustainable forestry initiative that aims to reduce emissions from deforestation and degradation. It covers an area of 92,530 hectares of unlogged native Congolese forest, legally designated as a selective logging concession. The project is a critical step in protecting the environment and preserving biodiversity in the region.\n\nKey Highlights:\n- The project is focused on canceling planned degradation and deforestation activities in the area.\n- It aims to protect 92,530 hectares of native forest and maintain the biodiversity of the region.\n- Selective logging was originally planned for the dry lands, which covers 55,950 hectares of the area.\n- The project is expected to reduce carbon emissions by preventing deforestation and degradation activities.\n\nThe North Pikounda REDD+ Project is essential in the fight against climate change. By protecting the forest area, the project not only preserves the biodiversity of the region but also prevents the release of carbon emissions into the atmosphere. The project is a prime example of how conservation and sustainable forestry practices can help combat global warming and its devastating effects.",
    "short_description": "The North Pikounda REDD+ Project protects 92,530 hectares of unlogged native Congolese forest from deforestation and degradation. The project cancels the planned selective logging and focuses on maintaining and protecting the biodiversity of the area.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [16.181197, 0.824273]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/1052",
    "images": [],
    "puroBatchTokenId": ""
  },
  {
    "key": "VCS-934",
    "projectID": "934",
    "name": "The Mai Ndombe REDD+ Project",
    "methodologies": [
      {
        "id": "VM0009",
        "category": "Forestry",
        "name": "Methodology for Avoided Ecosystem Conversion"
      }
    ],
    "vintages": [
      "2015",
      "2012"
    ],
    "registry": "VCS",
    "updatedAt": "1706418438",
    "country": "Congo, The Democratic Republic of the",
    "region": "Africa",
    "price": "1.1499488",
    "stats": {
      "totalBridged": 82000,
      "totalRetired": 9021.85945328819,
      "totalSupply": 30217.4280957118,
      "totalListingsSupply": 0,
      "totalPoolsSupply": 30217.4280957118
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "13",
      "15"
    ],
    "description": "The Mai Ndombe REDD+ Project, located in western DRC, Africa, will protect 248,956 hectares of forest from industrial logging, unsustainable fuel wood extraction and slash and burn agriculture. Carbon validation will be undertaken by the Verified Carbon Standard (VCS) and major socio-economic co-benefits ensured by the Climate, Community and Biodiversity (CCB) standard. The project is developed and managed in a joint venture by forest carbon leaders ERA-Ecosystem Restoration Associates Inc. and Wildlife Works Carbon LLC. This groundbreaking project will be the first of its kind in the Congo Basin and utilizes the novel methodology developed by Wildlife Works, VM0009, 'Methodology for Avoided Deforestation' approved by the VCS in October, 2012. The project is estimated to deliver over 175MT CO2-e over 30 years.",
    "long_description": "The Mai Ndombe REDD+ Project, located in western DRC, Africa, aims to protect 248,956 hectares of forest from industrial logging, unsustainable fuel wood extraction and slash-and-burn agriculture. This project is developed and managed by ERA-Ecosystem Restoration Associates Inc. and Wildlife Works Carbon LLC, the leaders in forest carbon. \n\nKey Highlights:\n- The project will be validated by the Verified Carbon Standard (VCS).\n- The Climate, Community and Biodiversity (CCB) standard will ensure socio-economic co-benefits.\n- Utilizing the novel methodology developed by Wildlife Works, VM0009, 'Methodology for Avoided Deforestation', which was approved by the VCS in October 2012.\n- The project will be the first of its kind in the Congo Basin.\n- Over 30 years, the project is estimated to deliver more than 175 million metric tonnes of CO2-e.\n\nBy protecting the forest, the Mai Ndombe REDD+ Project will help reduce carbon emissions and preserve wildlife habitats. The project will also provide socio-economic benefits to the local communities, including job creation and access to clean water. By creating a sustainable economy, the project will help improve the living standards of the local communities while protecting the environment.",
    "short_description": "The Mai Ndombe REDD+ Project in western DRC, Africa, protects 248,956 hectares of forest from harmful activities like industrial logging and slash and burn agriculture. Carbon validation is ensured by the Verified Carbon Standard (VCS), and socio-economic co-benefits are delivered by the Climate, Community and Biodiversity (CCB) standard. Developed and managed by ERA-Ecosystem Restoration Associates Inc. and Wildlife Works Carbon LLC, the project is estimated to deliver over 175MT CO2-e over 30 years.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [18.043932, -1.957187]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/934",
    "images": [],
    "puroBatchTokenId": ""
  },
  {
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
    "vintages": [
      "2017",
      "2014",
      "2012",
      "2013"
    ],
    "registry": "VCS",
    "updatedAt": "1721352992",
    "country": "Brazil",
    "region": "Latin America",
    "price": "1.1499488",
    "stats": {
      "totalBridged": 425261,
      "totalRetired": 12820.126604618,
      "totalSupply": 374737.408457496,
      "totalListingsSupply": 977.9,
      "totalPoolsSupply": 373759.508457496
    },
    "hasSupply": true,
    "sustainableDevelopmentGoals": [
      "13",
      "15"
    ],
    "description": "REDD Project to stop deforestation within private parcels amounting to  135, 105 Ha at the edge of the deforestation frontier in Brazil. The project will generate multiple climate, social, and biodiversity benefits.",
    "long_description": "A REDD Project to Halt Deforestation in Brazil\n\nIn the heart of Brazil, a REDD project has been implemented to prevent deforestation on private parcels of land spanning 135,105 hectares on the brink of the deforestation frontier. This project aims to produce various advantages for the environment, society, and biodiversity.\n\nKey Highlights:\n- The project is designed to protect over 135,000 hectares of land from deforestation, safeguarding the habitat of endangered species and preserving the natural beauty of the region.\n- By stopping deforestation, the project also prevents the release of carbon emissions into the atmosphere, thus mitigating the effects of climate change.\n- The implementation of this project has created job opportunities for the local community and fostered sustainable development in the region.\n\nThis REDD project is of critical importance as it tackles some of the biggest challenges facing Brazil and the world today. Deforestation has a significant impact on climate change, biodiversity loss, and the livelihoods of local communities. By preserving these private parcels of land, the project is not only protecting vital ecosystems but also contributing to the global effort to combat climate change.",
    "short_description": "This project aimed to stop deforestation within private parcels in Brazil, covering 135,105 hectares at the edge of the deforestation frontier. It generated various benefits for the climate, social welfare, and biodiversity.",
    "location": {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [-51.188205, -2.893955]
      }
    },
    "url": "https://registry.verra.org/app/projectDetail/VCS/981",
    "images": [],
    "puroBatchTokenId": ""
  }
]
```
{% endcode %}

**The information from this response such as "key" and "projectID" may be enough for your needs.**

You can retrieve the full details of a carbon project by its using the project key; [`api.carbonmark.com/carbonProjects/{id}`](https://api.carbonmark.com/#/paths/carbonProjects-id/get). For example:

```sh
curl --request GET \
  --url https://api.carbonmark.com/carbonProjects/VCS-1052 \
  --header 'Accept: application/json'
```

The response will contain the full carbon project details:

{% code overflow="wrap" %}
```json
{
  "key": "VCS-1052",
  "projectID": "1052",
  "registry": "VCS",
  "country": "Congo",
  "name": "North Pikounda REDD+",
  "methodologies": [
    {
      "id": "VM0011",
      "category": "Forestry",
      "name": "Methodology for Calculating GHG Benefits from Preventing Planned Degradation"
    }
  ],
  "region": "Africa",
  "description": "The North Pikounda REDD+ Project (the Project) is a reducing emission from deforestation and degradation plus conservation and sustainable forestry (REDD+) project designed to protect 92,530 hectares (ha) of unlogged native Congolese forest, legally designated as a selective logging concession. The anticipated selective logging would have been undertaken on the dry lands, consisting on an area of 55,950 ha.   The main activity of the North Pikounda REDD+ Project is the cancelation of the planned degradation and deforestation activities and the decision to instead protect the forest area, while maintaining and protecting the biodiversity of the area.",
  "short_description": "The North Pikounda REDD+ Project protects 92,530 hectares of unlogged native Congolese forest from deforestation and degradation. The project cancels the planned selective logging and focuses on maintaining and protecting the biodiversity of the area.",
  "long_description": "The North Pikounda REDD+ Project is a conservation and sustainable forestry initiative that aims to reduce emissions from deforestation and degradation. It covers an area of 92,530 hectares of unlogged native Congolese forest, legally designated as a selective logging concession. The project is a critical step in protecting the environment and preserving biodiversity in the region.\n\nKey Highlights:\n- The project is focused on canceling planned degradation and deforestation activities in the area.\n- It aims to protect 92,530 hectares of native forest and maintain the biodiversity of the region.\n- Selective logging was originally planned for the dry lands, which covers 55,950 hectares of the area.\n- The project is expected to reduce carbon emissions by preventing deforestation and degradation activities.\n\nThe North Pikounda REDD+ Project is essential in the fight against climate change. By protecting the forest area, the project not only preserves the biodiversity of the region but also prevents the release of carbon emissions into the atmosphere. The project is a prime example of how conservation and sustainable forestry practices can help combat global warming and its devastating effects.",
  "url": "https://registry.verra.org/app/projectDetail/VCS/1052",
  "images": [],
  "location": {
    "type": "Feature",
    "geometry": {
      "type": "Point",
      "coordinates": [16.181197, 0.824273]
    }
  },
  "stats": {
    "totalBridged": 55532,
    "totalRetired": 15033.8937675473,
    "totalListingsSupply": 0,
    "totalPoolsSupply": 40496.6062324527,
    "totalSupply": 40496.6062324527
  },
  "puroBatchTokenId": "",
  "price": "1.1499488",
  "updatedAt": "1706418438",
  "hasSupply": true,
  "vintages": [
    "2012"
  ],
  "sustainableDevelopmentGoals": [
    "13",
    "15"
  ],
  "listings": []
}
```
{% endcode %}

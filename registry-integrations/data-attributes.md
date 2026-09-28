# Data Attributes

Current credit metadata includes the following attributes:

| Field             | Type                | Description                                                                                                       |
| ----------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------- |
| \_id              | string              | System record identifier (e.g. CMARK-7)                                                                           |
| \_type            | string              | Record type (project)                                                                                             |
| \_createdAt       | ISO 8601 datetime   | Record creation timestamp                                                                                         |
| \_updatedAt       | ISO 8601 datetime   | Record last-modified timestamp                                                                                    |
| id                | slug                | Public project slug                                                                                               |
| name              | string              | Project name                                                                                                      |
| description       | string              | Project narrative description                                                                                     |
| status            | string              | Project lifecycle status (e.g. Validated)                                                                         |
| type              | string              | Mitigation type (e.g. Removal)                                                                                    |
| subcategory       | string              | Activity subcategory (e.g. afforestation)                                                                         |
| registry          | string              | Registry code (e.g. CMARK)                                                                                        |
| registryProjectId | string              | Project ID within the registry                                                                                    |
| url               | string (URL)        | Link to the registry project page                                                                                 |
| projectWebsite    | string (URL)        | Project developer's public website                                                                                |
| country           | string              | Country name                                                                                                      |
| countryDetails    | reference           | Reference to country record (ISO alpha-2 key)                                                                     |
| state             | string              | Sub-national region or state                                                                                      |
| region            | string              | Continental region                                                                                                |
| geolocation       | geopoint (lat, lng) | Representative coordinate for the project                                                                         |
| sdgs              | array of string     | UN Sustainable Development Goal numbers                                                                           |
| developer         | reference           | Reference to the project developer organization                                                                   |
| assessor          | reference           | Reference to the validation/verification body                                                                     |
| standards         | array of reference  | Carbon standards the project is certified under                                                                   |
| methodologies     | array of reference  | Methodologies applied (e.g. CARBOREA-TCR)                                                                         |
| hostedDocuments   | array of hostedFile | Project documents, each with category, filename, mimetype, size, optional description, and a file asset reference |

Carbonmark is currently exploring full alignment with the Carbon Data Open Protocol (CDOP)’s Version 2.0 data schema. You can read more about CDOP here: [https://www.cdop.info/](https://www.cdop.info/).

Carbonmark's credit metadata already covers the core of CDOP's Project Approach & Details module: project name, description, registry identifiers, status, mitigation type, activity type, developer and validator references, methodology and standard references, project documentation, and SDG contributions — along with the country, region, and coordinate data that anchor CDOP's Location Details module.&#x20;

Building on that foundation, we are investigating four areas to deepen alignment: adopting CDOP's structural conventions (snake\_case naming, entity-level nesting, and time-versioned records with is\_current flags); mapping our values to CDOP's controlled vocabularies, including working with CDOP governance to add a number of registries that we work with, including our CMARK system, to the enumerated lists; extending location data with ISO 3166 and UN M49 codes and project boundary files; and surfacing credit-level data, including serial numbers, credit blocks, vintages, and issuance batches from the token layer into CDOP's Unit Description and Issuances modules, alongside structured estimations, crediting period, and permanence data captured at project onboarding. <br>


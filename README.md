# OSLOthema-myTheme

This branch should be used to make the data standard available on the [standards registry of Flanders](https://data.vlaanderen.be/standaarden/).

## Structure of this repository

This repository contains four folders to store data standard related information.

### Charter

This folder should contain the OSLO charter. Accepted file formats are Word (`.docx`) or PDF (`.pdf`).

### Descriptions

For each data standard a short description is required that will be displayed on the detail page on the standards registry. Multiple files (descriptions) are allowed, e.g. one for the application profile and one for the vocabulary. The file format must be Markdown (`.md`).

### Presentations

Presentations given by the OSLO Editors during one of the workshop can be stored in this folder. Accepted file format are Powerpoint (`.pptx`) and PDF (`.pdf`).

### Reports

After each workshop a report of that workshop is made by the OSLO Editors, which can be stored in this folder. Accepted file format are Word (`.docx`) and PDF (`.pdf`).

### ap-or-voc-config.json

This is the file that must be used to configure the standard for publication on the standards registry. If the OSLO Editor wants to publish the application profile and vocabulary on the standards registry separately, then **two** configuration files must be created.

## Content of the configuration file

### `title`

This is the name of the data standard, e.g. *Applicatieprofiel LDES* or *Vocabularium Persoon*.

### `type`

The type of the data standard. Allowed values are:
- Applicatieprofiel
- Vocabularium
- Implementatiemodel
- Technische standaard
- Standaard voor organisatorische interoperabiliteit

### `usage`

Is the usage of the data standard mandatory or recommended? Allowed values are:
- Aanbevolen (vrijwillig)
- Verplicht

### `repsonsibleOrganisation`

The name of the organisation that is responsible for the data standard.

### `responsibleOrganisationUri`

#### `name`

The name of the organisation that is responsible for the data standard

#### `uri`

The uri of the organisation that is responsible for the data standard.

To construct the URI of the organisation, the [Wegwijs application](https://wegwijs.vlaanderen.be/#/organisations) can be used to find the OVO-code (identifier of an organisation). The URI has the following structure `https://data.vlaanderen.be/id/organisatie/{OVO-code}`.

### `publicationDate`

Date on which the most recent version of the standard was published

### `descriptionFileName`

The name of the Markdown file (stored in the `descriptions` folder) that contains the description to be displayed on the standards registry.

### `specificationDocuments`

Links to the application profile(s) or vocabulary. This **must** always be an array of objects with the properties `name` and `link`.

#### Example
```json
"specificationDocument": [
    {
        "name": "Applicatieprofiel LDES",
        "link": "https://data.vlaanderen.be/doc/applicatieprofiel/ldes"
    }
]
```

### `documentation`

Additional documentation to be displayed on the detail page of the data standard, e.g. a mapping described in an Excel file or link to external specification.

This **must** always be an array of objects containing the properties `name` and `link` or `fileName`. Using the `link` means you link to an URL, while using `fileName` means you want to link to a document that was stored in the `documentation` folder.

#### Example
```json
"documentation": [
    {
        "name": "Voorbeeld van een mapping",
        "fileName": "mapping-voorbeeld.xlsx"
    },
    {
        "name": "Link naar externe spec",
        "link": "https://example.org/externalSpec"
    }
]
```

### `charter`

The OSLO charter that will be displayed on the detail page of the data standard. This **must** always be an object containing the properties `name` and `fileName`. The value of `fileName` must be the name of the document which was stored in the `charter` folder.

#### Example
```json
"charter": {
    "name": "OSLO Charter",
    "fileName": "oslo-charter.docx"
}
```

### `reports`

Reports made of the workshop to be displayed on the detail page of the data standard.

This **must** always be an array of objects containing the properties `name` and `link` or `fileName`. Using the `link` means you link to an URL, while using `fileName` means you want to link to a report that was stored in the `reports` folder.

#### Example
```json
"reports": [
    {
        "name": "Verslag workshop 1",
        "fileName": "verslag-workshop-1.docx"
    }
]
```

### `presentations`

Presentations that were used during the workshop and must be displayed on the detail page of the data standard.

This **must** always be an array of objects containing the properties `name` and `link` or `fileName`. Using the `link` means you link to an URL, while using `fileName` means you want to link to a presentation that was stored in the `presentations` folder.

#### Example
```json
"presentations": [
    {
        "name": "Presentatie workshop 1",
        "fileName": "presentatie-workshop-1.pptx"
    },
    {
        "name": "Presentatie workshop 2",
        "fileName": "presentatie-workshop-2.pptx"
    },
    {
        "name": "Presentatie workshop 3",
        "link": "https://docs.google.com/presentation/d/presentatie-workshop-3"
    }
]
```

### `dateOfRegistration`

The date on which the data standard was announced on the working group 'Data Standards'.

### `dateOfAcknowledgementByWorkingGroup`

The date on which the data standard was accepted as an acknowledged standard by the working group 'Data standards'.

### `dateOfAcknowledgementBySteeringCommittee`

The date on which the data standard was accepted as an acknowledged standard by the steering committee 'Flemish Information and ICT policy'.
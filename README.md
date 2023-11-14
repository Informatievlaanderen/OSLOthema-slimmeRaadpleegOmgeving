
# OSLO Slimme Raadpleegomgeving

De opzet van dit traject is om inhoudelijke annotaties toe te voegen aan de besluiten. Er wordt hierbij gekeken naar het toevoegen van geo-informatie (straten, wijken, …), thema-informatie en het linken van reglementen zodat een gecoördineerde versie steeds centraal beschikbaar is.

# Voorbeelden

## Locatie-gebaseerde informatie

Het AP gebruikt de eigenschap [geografischeDekking](http://purl.org/dc/terms/spatial) om locaties te linken aan een besluit(onderdeel). Het bereik hiervan is het generieke Plaats dat een plaatsnaam en geometrie voorziet, maar kan ook ingevuld worden met specifieke vormen van plaatsen zoals een adres of gebouw.

### Een straatnaam

Indien mogelijk proberen we te linken naar een basisregister.

Bijvoorbeeld, een straatnaam kan als volgt gelinkt worden:
```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# eli: http://data.europa.eu/eli/ontology# dcterms: http://purl.org/dc/terms/" property="prov:generated" typeof="besluit:Besluit" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644">
  <span property="eli:title" datatype="xsd:string">2023_CBS_08639 - OMV_2023072621 R - aanvraag omgevingsvergunning voor het verbouwen en uitbreiden van een eengezinswoning  - zonder openbaar onderzoek  - <span property="dcterms:spatial" resource="https://data.vlaanderen.be/id/straatnaam/71082">Molenstraat</span>, 9032 Gent - Vergunning</span>
</div>
```

Om de link op te halen werd het SPARQL endpoint van Digitaal Vlaanderen gebruikt met [deze query](https://query.linkeddatafragments.org/#datasources=https%3A%2F%2Fdata.vlaanderen.be%2Fsparql&query=PREFIX%20adres%3A%20%3Chttps%3A%2F%2Fdata.vlaanderen.be%2Fns%2Fadres%23%3E%0APREFIX%20prov%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fprov%23%3E%0A%0ASELECT%20distinct%20*%0AWHERE%20%7B%0A%20%20%20%20%3Fstraatnaam%20a%20adres%3AStraatnaam%20%3B%0A%20%20%20%20%20%20%20rdfs%3Alabel%20%3Fstraatnaamlabel%20%3B%0A%20%20%20%20%20%20prov%3AwasAttributedTo%20%3Fgemeente%20.%0A%20%20%20%20%0A%20%20%20%20%20%20%20%3Fgemeentenaam%20adres%3AisAfgeleidVan%20%3Fgemeente%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20rdfs%3Alabel%20%3Fgemeentelabel%20.%0A%20%20%20%20%20%20%0A%20%20%20%20%20%20FILTER%20(str(%3Fstraatnaamlabel)%20%3D%20%22Molenstraat%22)%0A%20%20%20%20%20%20FILTER%20(str(%3Fgemeentelabel)%20%3D%20%22Gent%22)%0A%7D).
Andere mogelijkheid is het gebruik van de [basisregisters Vlaanderen API](https://docs.basisregisters.vlaanderen.be/docs/api-documentation.html).

### Een adres

Bij adressen wordt gebruik gemaakt van [Adresvoorstellingen](https://data.vlaanderen.be/doc/applicatieprofiel/adresregister/#Adresvoorstelling) die op hun beurt verwijzen naar een adres in het basisregister.

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# eli: http://data.europa.eu/eli/ontology# dcterms: http://purl.org/dc/terms/ adres: https://data.vlaanderen.be/ns/adres# locn:http://www.w3.org/ns/locn#"> 
<div property="prov:generated" typeof="besluit:Besluit" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644">
  <span property="eli:title" datatype="xsd:string">2023_CBS_08639 - OMV_2023072621 R - aanvraag omgevingsvergunning voor het verbouwen en uitbreiden van een eengezinswoning  - zonder openbaar onderzoek  - <span property="dcterms:spatial" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644/adresvoorstelling/1">Molenstraat 1</span>, 9032 Gent - Vergunning</span>
</div>
<div typeof="locn:Address" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644/adresvoorstelling/1">
<span property="adres:Adresvoorstelling.huisnummer" content="1"></span>
<span property="adres:gemeentenaam" content="Gent" language="nl"></span>
<span property="locn:postcode" content="9000"></span>
<span property="locn:thoroughfare" content="Molenstraat"></span>
<span property="adres:verwijstNaar" content="https://data.vlaanderen.be/id/adres/3794238"></span>
</div>
</div>
```
Het ophalen van een adres URI op basis van gemeentenaam, straatnaam en huisnummer werd met deze [query](https://query.linkeddatafragments.org/#datasources=https%3A%2F%2Fdata.vlaanderen.be%2Fsparql&query=PREFIX%20adres%3A%20%3Chttps%3A%2F%2Fdata.vlaanderen.be%2Fns%2Fadres%23%3E%0APREFIX%20prov%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fprov%23%3E%0A%0ASELECT%20distinct%20*%0AWHERE%20%7B%0A%20%20%09%3Fadres%20adres%3AheeftStraatnaam%20%3Fstraatnaam%20%3B%0A%20%20%20%20%20%20%20%20%20%20adres%3Ahuisnummer%20%3Fhuisnummer%20.%0A%20%20%0A%20%20%20%20%3Fstraatnaam%20a%20adres%3AStraatnaam%20%3B%0A%20%20%20%20%20%20%20rdfs%3Alabel%20%3Fstraatnaamlabel%20%3B%0A%20%20%20%20%20%20prov%3AwasAttributedTo%20%3Fgemeente%20.%0A%20%20%20%20%0A%20%20%20%20%20%20%20%3Fgemeentenaam%20adres%3AisAfgeleidVan%20%3Fgemeente%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20rdfs%3Alabel%20%3Fgemeentelabel%20.%0A%20%20%20%20%20%20%0A%20%20%20%20%20%20FILTER%20(str(%3Fstraatnaamlabel)%20%3D%20%22Molenstraat%22)%0A%20%20%20%20%20%20FILTER%20(%3Fgemeentelabel%20%3D%20%22Gent%22)%0A%20%20%20%20%20%20BIND%20(str(%3Fhuisnummer)%20as%20%3Fhuisnummerstring)%0A%20%20%20%20%20%20FILTER%20(%3Fhuisnummerstring%20%3D%20%221%22)%0A%7D) gedaan.

### Combinatie van straat en huisnummers

Wanneer er meerdere huisnummers geannoteerd worden, raden we aan om elk adres apart te linken.
De adresvoorstellingen dienen apart beschreven te worden zoals hierboven getoond.

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# eli: http://data.europa.eu/eli/ontology# dcterms: http://purl.org/dc/terms/ adres: https://data.vlaanderen.be/ns/adres# locn:http://www.w3.org/ns/locn#"> 
<div property="prov:generated" typeof="besluit:Besluit" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644">
  <span property="eli:title" datatype="xsd:string">2023_CBS_08639 - OMV_2023072621 R - aanvraag omgevingsvergunning voor het verbouwen en uitbreiden van een eengezinswoning  - zonder openbaar onderzoek  -
<span property="dcterms:spatial" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644/adresvoorstelling/1">Molenstraat 1</span>
<span property="dcterms:spatial" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644/adresvoorstelling/2"></span>
<span property="dcterms:spatial" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644/adresvoorstelling/3"></span>
tem
<span property="dcterms:spatial" resource="https://data.gent.be/id/besluiten/23.0829.4481.1644/adresvoorstelling/4">10</span>
    , 9032 Gent - Vergunning</span>
</div>
</div>
```

### Een plaats met naam en geometrie

Om een bepaalde plaats aan te duiden, zoals een wijk of evenementenweide, met een naam en geometrie kunnen we Plaats van OSLO generiek gebruiken.

```
<div prefix="besluit: http://data.vlaanderen.be/ns/besluit# eli: http://data.europa.eu/eli/ontology# dcterms: http://purl.org/dc/terms/ adres: https://data.vlaanderen.be/ns/adres# locn: http://www.w3.org/ns/locn# rdfs: http://www.w3.org/2000/01/rdf-schema# geosparql: http://www.opengis.net/ont/geosparql#"> 
  <div property="prov:generated" typeof="besluit:Besluit" resource="https://data.gent.be/id/besluiten/23.0829.9225.8540">
    <span property="eli:title" datatype="xsd:string">2023_CBS_08501 - 15748/M/1 - Geluidsuitzending - op donderdag 21 september 2023 (Gentsterpop) op de
    <div property="dcterms:spatial" typeof="locn:Location" resource="https://data.gent.be/id/besluiten/23.0829.9225.8540/plaats/1">
      <span property="rdfs:label">evenementenweide van de Blaarmeersen aan de Duiklaan te Gent</span>.
      <div property="locn:geometry" typeof="locn:Geometry" resource="https://data.gent.be/id/besluiten/23.0829.9225.8540/plaats/1/geometrie">
<span property="geosparql:asWKT" content="POLYGON((3.6865100765135135 51.048086068457195,3.6878730996977533 51.04793498036966,3.6877832771278913 51.04755864782376,3.6863662849646053 51.04784147386724,3.6865100765135135 51.048086068457195))" datatype="geosparql:wktLiteral"></span>
      </div>
    </div>
  </div>
</div>
```

## Thema's

## LPDC-codes

## Gecoördineerde versies van reglementen


## Administrative information

**_Editors:_**
- first read [deze richtlijnen](https://github.com/Informatievlaanderen/OSLO-toolchain/blob/master/doc-user/README.md) .
- Use [TagsAndNotes.xlsm](https://github.com/Informatievlaanderen/OSLO-allerleiTooltjes/tree/master/EA-Excel/TagsAndNotes) 
  to edit tags in Excel. The files `*.xlsm` are included in the repo as git-ignored.

### Tags
To refer to the available git tags to create a publication point from, see the "Releases" tab of this repo.


### Branches
The organisation and meaning of the git branches are documented in this table.


| Branch | Purpose | Active (y/n) |
| ------ | --------- | ---------------- | 
| main  | Contains latest diagram | y |
| standaardenregister  | Contains artefacts for standaardenregister | y |


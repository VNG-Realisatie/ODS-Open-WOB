# API-specificatie van de Wob-verzoeken Publicatie API

## Inleiding

Eerste aanzet (0.1.0 versie) voor een API specificatie (OAS3) van de Wob-verzoeken Publicatie API. In eerste instantie is deze API bedoeld voor het onsluiten van gepubliceerde Wob-verzoeken van gemeenten en provincies naar de landelijke PLOOI-voorziening. Hieronder een paar links naar bekende OAS-viewers zodat de API-specificatie op een prettige manier gelezen kan worden:

- [ReDoc](http://redocly.github.io/redoc/?url=https://raw.githubusercontent.com/VNG-Realisatie/ODS-Open-WOB/master/oas-specificatie/openapi.yaml),

- [Swagger](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/VNG-Realisatie/ODS-Open-WOB/master/oas-specificatie/openapi.yaml),

- [YAML (broncode)](https://raw.githubusercontent.com/VNG-Realisatie/ODS-Open-WOB/master/oas-specificatie/openapi.yaml).

## Omzetting JSON naar OAS

Bij het omzetten van de JSON datadefinities naar OAS (in yaml formaat) zijn de volgende aanpassingen doorgevoerd:

- Namen van properties omgezet naar lowerCamelCase (conform [design rule  DR 1.3](https://github.com/VNG-Realisatie/API-Kennisbank/tree/master/Design%20rules#dr13-namen-van-properties-zijn-in-lowercamelcase) van VNG-Realisatie). 
- Enumeraties omgezet naar zowel lower- als snake-case (conform [design rule DR 2.4](https://github.com/VNG-Realisatie/API-Kennisbank/tree/master/Design%20rules#dd24-enumeratie-waarden-zijn-in-snake_case)).
- Het voorschrift `"format: date"` toegevoegd aan datum-velden.
- Het voorschrift `"format: uri"` toegevoegd aan URL-velden.
- Aan het veld `postcodegebied` de eis  `"maxLength: 6"` toegevoegd.

## HTTP-operaties

Er zijn HTTP-operaties toegevoegd aan de OAS:
- `GET /wob-verzoeken` operatie om Wob-verzoeken te pullen op basis van een peildatum als query parameter (`gepubliceerdVanaf` of `gewijzigdVanaf`).
- `GET /wob-verzoeken/{uuid}` operatie om te verwijzen naar een individueel Wob-verzoek.

## Opmerkingen / vragen / to do

De volgende vragen of opmerkingen moeten nog beantwoord of/en verwerkt worden in de OAS:

- Query parameters van de `GET /wob-verzoeken` operatie:
  - Type van de Query parameters `gepubliceerdVanaf` en `gewijzigdVanaf` moeten wellicht omgezet worden naar `date-time` formaat (in plaats van `date`) voor nauwkeurigere synchronisatie mogelijkheden met PLOOI. Nog even afkijken hoe ze dat bij ATOM en RSS-feeds precies doen...
  - Nu kun je alleen filteren op vanaf-datum. Mogelijk ook eind-datum toevoegen als query-parameter.
  - Je kunt nog niet filteren op verwijderde Wob-verzoeken. Je zou zeggen dat PLOOI deze functionaliteit nodig heeft om goed te kunnen synchroniseren met de bronhouders van de Wob-verzoeken.
  - Een wob-verzoek kent drie statussen: `nieuw`, `gewijzigd` en `verwijderd`. Je zou verwachten dat analoog hieraan ook de query parameters vergelijkbare namen krijgen. Dus `nieuwVanaf`, `gewijzigdVanaf` en `verwijderdVanaf`.
- Title-attributen nog toevoegen aan definities van de property-velden in het OAS-schema.
- Wellicht moeten de bijlagen van de Wob-verzoeken uiteindelijk genormaliseerd worden als zelfstandige resources in de OAS-specificatie. Daar is nog een stukje informatie-analyse voor nodig.
- Velddefinities moeten nog gecheckt worden met de bestaande Besluiten en Documenten API's van Zaakgerichtwerken, de zogenaamde ZGW API's.
- Er moet nog een volledige check worden gedaan op het voldoen aan alle design rules zowel die van de [Nederlandse API Strategie](https://publicatie.centrumvoorstandaarden.nl/api/adr/) als die van [VNG Realisatie](https://github.com/VNG-Realisatie/API-Kennisbank/tree/master/Design%20rules#design-rules-vngrealisatie).
- Wat is de reden van het groeperen van `status` en `tijdstipLaatsteWijziging` in de groep `versieInformatie`? Heeft tijdstipLaatsteWijziging alleen betrekking op de wijziging van de status (en niet op andere properties van het Wob-verzoek)?
- Wijzigt het veld `versieInformatie.tijdstipLaatsteWijziging` van het Wob-verzoek als een van de bijlagen verandert. De bijlagen hebben namelijk ook een `tijdstipLaatsteWijziging`.
- Het veld `behandelstatus` is  onduidelijk. Er is ook een statusveld bij de groep versieInformatie. Wat is het verschil en waarom heeft dit veld geen enumeratie?
- Het veld  `termijnoverschrijding` kan nog beter beschreven worden. Ik begrijp het in ieder geval niet.
- In de bijlagen zijn de velden `statusBijlage` en  `tijdstipLaatsteWijzigingBijlage` niet gegroepeerd. In Wob-verzoek wel. Waarom?
- Moet het veld `geografischGebied` geen enumeratie of referentielijst zijn?
- Het veld `coördinaten`:
  - Is hier geen internationaal formaat voor? GeoJSON of zoiets... 
  - Moeten de coördinaten niet van het type `number` zijn in plaats van `integer`, dus ook met decimalen?
- Check of het veld `geografischePositie` een internationale standaard is. Zoniet dan moeten de namen van de properties `longitude` en `lattitude` in het Nederlands conform de design rules van de Landelijke API Strategie.
- Het zou fijn zijn als er een aanvullende API door PLOOI beschikbaar wordt gesteld om de thema-velden te kunnen vullen met gestandaardiseerde waarden.

De meeste van bovenstaande opmerkingen zijn afkomstig uit de [YAML-file van de OAS](https://raw.githubusercontent.com/VNG-Realisatie/ODS-Open-WOB/master/oas-specificatie/openapi.yaml) en te herkennen aan de dubbele hash-tag ##.
### REST-API STANDAARD NEDERLANDSE SPOORWEGEN

___Deze standaard is per 2020-1-1 in gebruik genomen en wordt onderhouden door cci@ns.nl___ 

- Gebruik de voertaal en naamgeving van het relevante domeinmodel.

- Gebruik exact deze naamgevingsconventie:
  - meervoud voor **resources** (`/orders`, niet `/order`)
  
  - lowercase voor de **delen van de URL** (`/lowercase`, niet `/CamelCase` of `/UPPERCASE`.
  
  - camelCase voor **veldnamen in requests en responses** (`veldNaam`, niet `VeldNaam` of `veld_naam`)
  
  - UpperCamelCase (Pascal Case) voor **recordnamen** (`RecordNaam`, niet `recordName` of `record_naam`)
  
  - Snake case (`snake_case`) en Kebab case (`kebab-case`) zijn niet toegestaan.

- Gebruik de HTTP-methods in exact deze betekenis:
  - `POST` – **creëeren** van een resource en alle andere niet-idempotente operaties  
  
  - `PUT` – **vervangen** van een resource (strict idempotent)
  
  - `PATCH` - (gedeeltelijk) **aanpassen** van een resource (strict idempotent)
  
  - `GET` – **lezen** van een resource (strict idempotent)
  
  - `DELETE` – **verwijderen** van een resource (strict idempotent)
  
  - `OPTIONS` – de CORS pre-flight
 
- Gebruik onderstaande HTTP status codes in exact deze betekenis:
  - `102 Processing` (gegeven wanneer een asynchrone response nog steeds in behandeling is, hoort bij de `202 Accepted`, zie de NS REST design patterns)
  
  - `201 Created` (afgegeven bij de succesvolle creatie van een nieuwe resource, tegelijkertijd is ook een `Location` HTTP-header met de URL van de nieuwe resource aanwezig in de HTTP-response)
  
  - `202 Accepted` (gegeven wanneer een asynchrone response nog moet volgen, tegelijkertijd is een ook een `Location` HTTP-header met de URL van de toekomstige resource aanwezig in de HTTP-response. Hoort bij de `102 Processing`, zie de NS REST design patterns)
  
  - `204 No content` (Bewust lege response opgeleverd)
  
  - `409 Conflict` (duplicate data or invalid data state would occur)
  
  - zie https://httpstatuses.com/ 

- Gebruik UTF-8

- Gebruik ISO 8601 voor datums, zie https://www.iso.org/iso-8601-date-and-time-format.html

- Gebruik ISO 4217 voor geldcodes, zie https://www.iso.org/iso-4217-currency-codes.html 

- Gebruik ISO 3166 voor landencodes, zie https://www.iso.org/iso-3166-country-codes.html

- Controleer in elke request de `Accept-Type` HTTP-header

- Plaats in elke response de `Content-Type` HTTP-header

- Gebruik deze mime-types voor de `Content-Type` en `Accept-Type` HTTP-headers: 
  - `application/json` bij JSON payload
  
  - `application/xml` bij XML payload

- Gebruik de `Accept-Language` HTTP-header voor internationalisatie (vb. `Accept-Language: nl, en-gb;q=0.8, en;q=0.7`)

- Geef bij payload versleuteling aan in de `X-Encryption-Algorithm` HTTP-header welk algoritme is gebruikt (vb. `A128CBC-HS256`), zie https://tools.ietf.org/html/rfc7518#appendix-A.3

- Geef bij payload signering aan in de `X-Signing-Algorithm` HTTP-header welk algoritme is gebruikt (vb. `RS256`), zie https://tools.ietf.org/html/rfc7518#appendix-A.3 

- Vereis bij asynchrone requests het response adres (de webhook) in de `X-Callback-Url` HTTP-header. (vb. `X-Callback-Url: https://api.ns.nl/server/webhook/e52f0344-50f8-4f81-8d29-c6d49ef9d691`), zie de NS REST design patterns

- Ondersteuning sortering en paginering van responses (vb. `?offset=100&limit=50&order=id,-datum`) :
  - `offset` – index van eerste element in de respons
  
  - `limit` – hoeveelheid elementen in de respons
  
  - `order` – sortering, aflopende sortering prefixen met een ‘-‘

- Ondersteun maatwerk-responses d.m.v. comma-separated query-argumenten (vb. `?fields=name,address,city`) 

- Ondersteun caching met de volgende HTTP-headers:
  - `ETag` – Hash/MD5 waarde van de gehele HTTP-response. (vb. `ETag: "686897696a7c876b7e"`), zie https://en.wikipedia.org/wiki/HTTP_ETag
  
  - `Date` – Datumtijd dat de response was geleverd (in RFC1123 formaat) (vb. `Date: Sun, 06 Nov 1994 08:49:37 GMT`)
  
  - `Cache-Control` – Het maximum aantal seconden dat een response mag worden gecached. Moet `no-cache` zijn als caching niet is toegestaan (vb. `Cache-Control: 360 of Cache-Control: no-cache`)
  
  - `Expires` – De datumtijd waarop de response verloopt (in RFC1123 format). Niet aanwezig als caching niet is toegestaan (vb. `Expires: Sun, 06 Nov 1994 08:49:37 GMT`).
  
  - `Pragma` - Als Cache-Control is `no-cache` dan is staat `Pragma` HTTP-header ook op `no-cache`. Anders is de header niet aanwezig (vb. `Pragma: no-cache`)
  
  - `Last-Modified` – De datumtijd waarop de resource voor het laatst was aangepast (in RFC1123 format) (vb. `Last-Modified: Sun, 06 Nov 1994 08:49:37 GMT`)

- **Vermijd** hypermedia linking (HATEOAS), zie: https://restfulapi.net/hateoas/ 

- **Vermijd** OData, zie: https://en.wikipedia.org/wiki/Open_Data_Protocol 

- Versioneer een gehele API met een URL ‘versie’ deel

        e.g.
        https://api.example.com/1/orders
        
- Versioneer individuele resources in de `Accept` HTTP-header

        GET /jobs HTTP/1.1
        Host: api.example.com
        Accept: application/vnd.example.api+json;version=2

- Responses zijn niet ingepakt in een JSON envelop. 

    Niet zo:
     
        {
          "title": ""
        }
        
    maar zo:
    
        "title": ""

- Foutmelding volgens https://tools.ietf.org/html/rfc7807

- Responses bevatten HTTP-header: `X-Content-Type-Options: nosniff`

- Responses bevatten HTTP-header: `X-Frame-Options: deny`

- Content-headers bevatten een charset indicatie


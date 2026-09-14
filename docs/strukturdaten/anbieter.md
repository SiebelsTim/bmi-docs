# Für Anbieter

Um auf die Schulstrukturdaten-API zugreifen zu können, muss für das Angebot der OpenID-Client um den Grant Type `client_credentials` erweitert werden.
Außerdem muss der Scope `idm_api` erlaubt und abgefragt werden.

Das Angebot kann Daten aus der API abrufen, sofern

- eine Freigabe für die Schule in der [Angebots-API](../vidis/angebots-api) vorliegt.
- einzelne Gruppen oder die ganze Schule explizit zum [übertragen konfiguriert](https://strukturdaten.vidis.schule/) wurden.

Die API basiert auf der [Dienste-API von Schulconnex](https://schulconnex.de/docs/generated/openapi/dienste/schulconnex).
Die API ist unter https://ssd.vidis.schule/schulconnex/v1 erreichbar.
Die übertragenenen Felder sind abhängig vom jeweiligen Landessystem.

### Pseudonymisierung

Über diese API werden vollständigen Vor- und Nachnamen von Personen übertragen. Benutzer können über die Initialen und über das Feld `pid` identifiziert werden.
Es ist das selbe, wie der Claim `sub` beim SSO.

### Authentifizierung

Die API ist durch ein Bearer-Token geschützt.
Ein Token erhält man über den `client_credentials` Grant.
Dazu muss der Scope `idm_api` angefragt werden.
Der Autorisierungsserver ist [VIDIS](https://aai.vidis.schule/auth/realms/vidis/.well-known/openid-configuration).

### Beispielanfrage

```
GET /schulconnex/v1/personen-info?vollstaendig=personenkontexte,personen,gruppen&organisation.id=8e0289a5-b841-43ff-a394-ea1eee19c3dc HTTP/1.1
Host: ssd.vidis.schule
Authorization: Bearer $TOKEN


HTTP/1.1 200 OK
ETag: W/"1.h5vj8mcu3g"
Content-Type: application/json; charset=utf-8
```

```json
[
  {
    "pid": "5c08b773-3df8-3f36-85fb-d1dbe6ac1249",
    "person": {
      "name": {
        "initialenfamilienname": "W",
        "initialenvorname": "J"
      }
    },
    "personenkontexte": [
      {
        "id": "cd8ac299-4497-3d8d-9bd7-a3b202cf7aa7",
        "organisation": {
          "id": "8e0289a5-b841-43ff-a394-ea1eee19c3dc"
        },
        "gruppen": [
          {
            "gruppe": {
              "id": "019b0d05-52ae-7903-80a7-9b012ca39fa8",
              "orgid": "8e0289a5-b841-43ff-a394-ea1eee19c3dc",
              "bezeichnung": "5c",
              "typ": "Klasse",
              "jahrgangsstufen": ["05"],
              "faecher": [],
              "laufzeit": {
                "von": "2025-08-01",
                "bis": "2026-07-31"
              }
            }
          },
          {
            "gruppe": {
              "id": "019b03f6-b0fe-71eb-b98f-9c84d6846868",
              "orgid": "8e0289a5-b841-43ff-a394-ea1eee19c3dc",
              "bezeichnung": "Test01",
              "typ": "Kurs",
              "jahrgangsstufen": ["06", "05"],
              "faecher": [
                {
                  "bezeichnung": "Mathematik"
                }
              ],
              "laufzeit": {
                "von": "2025-08-01",
                "bis": "2026-07-31"
              }
            }
          }
        ]
      }
    ]
  },
  ...
]
```

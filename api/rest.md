# Dokumentacja interfesjów REST

## Adresy bazowe interfejsów

Środowisko produkcyjne:
* https://rit.poland.travel/api

## Pobieranie danych o podziale terytorialnym

`/provinces` - zwraca listę województw

`/districts/{province_id}` - zwraca listę powiatów, należy użyć ID województwa uzyskanego z wywołania `/province`

`/communities/{district_id}` - zwraca listę gmin, należy użyć ID powiatu uzyskanego z wywołania `/districts/{province_id}`

`/places/{community_id}` - zwraca listę miejscowośći, należy użyć ID gminy uzyskanego z wywołania `/communities/{district_id}`

```
# curl --silent --show-error https://rit.poland.travel/api/provinces | jq

[
  {
    "province_id": 1912,
    "gus_id": 2,
    "dictionary_id": 6912,
    "name": "DOLNOŚLĄSKIE"
  },
  {
    "province_id": 1913,
    "gus_id": 4,
    "dictionary_id": 6912,
    "name": "KUJAWSKO-POMORSKIE"
  }
]
  ```
`/places/search/{name}` - zwraca listę miejscowości posiadających w nazwie `name`

```
# curl --silent --show-error https://rit.poland.travel/api/places/search/Warszawa | jq
[
  {
    "place_id": 45406,
    "place": "Warszawa",
    "province_id": 1924,
    "region": "ŚWIĘTOKRZYSKIE",
    "district_id": 1686,
    "district": "buski",
    "community_id": 5301,
    "commune": "Pacanów"
  },
  {
    "place_id": 107124,
    "place": "Warszawa",
    "province_id": 1918,
    "region": "MAZOWIECKIE",
    "district_id": 1778,
    "district": "Warszawa",
    "community_id": 5856,
    "commune": "Warszawa"
  }
]
```

`/places/search/{province_id}/{name}` - zwraca listę miejscowości posiadających w nazwie `name` w województwie o podanym ID

```
# curl --silent --show-error https://rit.poland.travel/api/places/search/1924/Warszawa | jq
[
  {
    "place_id": 45406,
    "place": "Warszawa",
    "province_id": 1924,
    "region": "ŚWIĘTOKRZYSKIE",
    "district_id": 1686,
    "district": "buski",
    "community_id": 5301,
    "commune": "Pacanów"
  }
]
```

## Pobieranie danych obiektów

## Pobieranie metadanych

`/v1/metadata/languages` - zwraca obsługiwane wersje językowe

```
# curl --silent --show-error https://rit.poland.travel/api/v1/metadata/languages | jq
[
  {
    "id": "en-GB",
    "name": "Angielski (GB)"
  },
  {
    "id": "pl-PL",
    "name": "Polski"
  },
  {
    "id": "de-DE",
    "name": "Deutsch (DE-CH-AT)"
  }
]
```

`/v1/metadata/categories` - zwraca informacje o kategoriach, podkategoriach, atrybutach kategorii i inne

```
[
  {
    "id": "C004",
    "name": "Atrakcje turystyczne",
    "parent": "C002",
    "subcategories": [
      "C014",
      "C015",
      "C016",
      "C017",
      "C018",
      "C019",
      "C020",
      "C021",
      "C022",
      "C902"
    ],
    "attributes": [
      "A020",
      "A129",
      "A132"
    ],
    "translations": {
      "C004": {
        "pl-PL": "Atrakcje turystyczne",
        "en-GB": "Tourist attractions",
        "de-DE": "Toursitische Attraktionen",
        "es-ES": "Encuentra atracción turística",
        "de-AT": "Touristische Attraktionen",
        "fr-BE": "Attractions touristiques",
        "fr-FR": "Attractions touristiques",
        "he-IL": "אטרקציות תיירותיות",
        "hu-HU": "Látnivalók",
        "it-IT": "Attrazioni turistiche",
        "ja-JP": "観光スポット",
        "nl-BE": "Toeristische attracties",
        "nl-NL": "Toeristische attracties",
        "sv-SE": "Turistattraktioner",
        "ru-RU": "Туристические достопримечательности",
        "en-EN": "Tourist attractions",
        "en-US": "Tourist attractions",
        "uk-UA": "Туристичні атракції",
        "cs-CZ": "Památky",
        "da-DK": "Seværdigheder",
        "nn-NO": "Severdigheter",
        "zh-CN": "旅游胜地"
      }
    }
  }
]
```

`/v1/metadata/attributes` - zwraca informacje o atrybutach

```
[
  {
    "id": "A132",
    "name": "Czas zwiedzania",
    "description": "Wprowadź czas zwiedzania obiektu w godzinach.",
    "type": "NUMBER",
    "is_multilanguage": 0,
    "parent": null,
    "subattributes": [],
    "validation": "^(\\d{1,3}$)|(\\d{1,3}[.]\\d{1})$",
    "dictionary": null,
    "translations": {
      "A132": {
        "pl-PL": "Czas zwiedzania"
      }
    }
  },
  {
    "id": "A135",
    "name": "Miejsca noclegowe dla kampingów",
    "description": "",
    "type": "COMPLEX",
    "is_multilanguage": 0,
    "parent": null,
    "subattributes": [
      "A092",
      "A093",
      "A094"
    ],
    "validation": null,
    "dictionary": null,
    "translations": {
      "A135": {
        "pl-PL": "Miejsca noclegowe dla kampingów"
      }
    }
  },
]
```

`/v1/metadata/dictionaries` - zwraca informacje o danych słownikowych

```
[
  {
    "id": "D065",
    "name": "Rodzaj wypożyczalni",
    "is_multilanguage": 1,
    "items": [
      {
        "id": 2385,
        "order": 0
      },
      {
        "id": 2386,
        "order": 1
      },
      {
        "id": 2387,
        "order": 2
      },
      {
        "id": 2388,
        "order": 3
      }
    ],
    "translations": {
      "D065": {
        "pl-PL": "Rodzaj wypożyczalni"
      },
      "2385": {
        "pl-PL": "rowery"
      },
      "2386": {
        "pl-PL": "sprzęt wodny"
      },
      "2387": {
        "pl-PL": "sprzęt narciarski"
      },
      "2388": {
        "pl-PL": "jachty i motorówki"
      }
    }
  },
]
```

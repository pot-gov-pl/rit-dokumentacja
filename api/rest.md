# Dokumentacja interfesjów REST

## Endpointy

### Adresy bazowe interfejsów

Środowisko produkcyjne:
* https://rit.poland.travel/api

### Pobieranie danych o podziale terytorialnym

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

### Pobieranie danych obiektów

`/v1/objects` - zwraca obiekty według filtrów zadanych w query stringu
`/v1/objects/{id}` - zwraca obiekt o zadanym ID

Dostępne filtry atrybutów:
* like
* not_equal
* greater_than
* greater_equal
* lesser_than
* lesser_equal

Inne filtry:
* categories - filtruj według kategorii, np. `categories[]=C004&categories[]=C006`

Przykład: wyszukanie wszystkich obiektów z tekstem "Gwiazda" w nazwie wymaga użycia filtru "like" na atrybucie "A001" odpowiadającym tytułowi (nazwie) obiektu:

```
# curl --silent --show-error --globoff https://rit.poland.travel/api/v1/objects?like[A001]=Gwiazda

[{"id":403281},{"id":412073},{"id":434258},{"id":438780},{"id":439218},{"id":440864},{"id":441529},{"id":441792},{"id":442745},{"id":458507},{"id":466356},{"id":466737}]

# curl --silent --show-error --globoff https://rit.poland.travel/api/v1/objects/403281 | jq | grep A001

"A001": "Apartament Pod Gwiazdami",
"A001_CS_CZ": null,
"A001_DE_DE": null,
"A001_EN_EN": "Pod Gwiazdami (Under the Stars) Apartment",
"A001_ES_ES": null,
"A001_FR_FR": null,
"A001_HE_IL": null,
"A001_HU_HU": null,
"A001_IT_IT": null,
"A001_RU_RU": null,
"A001_UK_UA": null,
"A001": "A001_EN_EN",
```

### Pobieranie metadanych

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

## Struktura danych

### JSON wynikowy /v1/objects

ROOT
* metric
  * requested_at - data wykonania żadania, do celów debugowania
* attributes
  * id - ID obiektu turystycznego w bazie RIT (tzw RIT ID)
  * state - stan publikacji obiektu (1 = opublikowany, inne wartości - nie do publikacji)
  * AXXX - wartość atrybutu AXXX (np. A001)
  * AXXX_YY_ZZ - wartość atrybutu AXXX w tłumaczeniu na język YY_ZZ (np. A001_EN_EN - tytuł po angielsku)
  * created_at - data utworzenia obiektu
  * updated_at - data aktualizacji obiektu
  * duplicate_id - ID obiektu, którego ten jest duplikatem
* categories - tablica codów kategorii, np. C001, C002, do których należy obiekt
* images - tablica obrazów
  * id - ID obrazu
  * src - URL do obrazu
  * width
  * height
  * author
  * title
  * copyright
  * descrfiption
  * latitude
  * longitude
  * mime_type - np. "image/jpeg"
  * created_at - data utworzenia obrazu
  * updated_at - data aktualizacji informacji o obrazie
* languages - wersje językowe obiektu (inne niż polska)
  * YY_ZZ - np. EN_EN, DE_DE
    * AXXX - nazwa atrybutu z tłumaczeniem dla AXXX (np. A001: A001_EN_EN)

### JSON wynikowy /v1/attributes

ROOT - zawiera **tablicę** atrybutów, każdy opisany tak:
* id - kod atrybutu, np. A001
* name - nazwa (polska)
* description - opis / etykieta
* is_multilanguage - gdy pozycje słownika posiadają tłumaczenia na inne języki
* type - typ atrybutu, jeden z: SHORT_TEXT, LONG_TEXT, NUMBER, BOOLEAN, DATE, COMPLEX, MULTIPLY_LIST, SINGLE_LIST (patrz: [stara dokumentacja](https://github.com/pot-gov-pl/rit-dokumentacja/blob/master/webservices.md#ad-2-atrybuty) )
* parent - atrybut nadrzędny 
* subattributes - tablica kodów atrybutów podrzędnych (niepuste tylko dla atrybutu typu COMPLEX)
* validation - regexp do walidowania poprawności wartości atrybutu
* dictionary - kod słownika np. D001 dla atrybutów typu SINGLE_LIST, MULTIPLY_LIST
* translations 
  * AXXX - kod atrybutu
    * XX-YY - tłumaczenie dla języka XX-YY, np. "en-GB"


### JSON wynikowy /v1/categories

ROOT - zawiera **tablicę** kategorii, każda opisana tak:
* id - kod kategorii, np. C001
* name - nazwa (polska)
* parent - atrybut nadrzędny 
* subcategories - tablica kodów atrybutów podrzędnych (niepuste tylko dla atrybutu typu COMPLEX)
* attributes - tablica kodów atrybutów używanych przez obiekty tej kategorii
* translations 
  * AXXX - kod atrybutu
    * XX-YY - tłumaczenie dla języka XX-YY, np. "en-GB"

### JSON wynikowy /v1/dictionaries

ROOT - zawiera **tablicę** obiektów opisujących słowniki, każdy obiek opisany tak:
* id - kod słownika, np. D001
* name - nazwa (polska)
* is_multilanguage - gdy pozycje słownika posiadają tłumaczenia na inne języki
* items - tablica pozycji słownika
  * id - id pozycji
  * order - waga pozwalająca na posortowanie pozycji w słowniku
* translations
  * # - numer ID pozycji
    * XX-YY - tłumaczenie dla języka XX-YY, np. "en-GB"
  * DXXX - tłumaczenia dla nazwy słownika
    * XX-YY - tłumaczenie dla języka XX-YY, np. "en-GB"

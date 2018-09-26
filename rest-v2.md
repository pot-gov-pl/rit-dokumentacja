# Opis API Informator v2

Wersja 1.3, Polska Organizacja Turystyczna, Betacom S.A.

## Historia zmian

Autor | Data ukończenia |	Opis zmian |	Wersja |	Status
--- | --- | --- | --- | ---
Jacek Guz |	18.11.2014 |	Utworzenie dokumentu |	1.0 |	Do przeglądu
Jacek Guz |	21.01.2015 |	Pkt 7. Pobranie listy obiektów turystycznych i pkt 8. Pobranie listy obiektów turystycznych w pobliżu trasy. Dodano nowy parametr ascii. |	1.1	 |
Jacek Guz	|	27.01.2015	|	Pkt 9. Dodano pobranie drzewa kategorii.	|	1.2 |
Jacek Guz	|	21.08.2018	|	 |	1.3	|

## 1.	Wstęp
Dokument opisuje API REST Informatora w wersji v2.

## 2.	Lista metod
Nazwa	| Opis
--- | ---
/api/v2/regions.jsonp<br>/api/v2/regions |	Pobiera listę województw.
/api/v2/cities.jsonp<br>/api/v2/cities |	Pobiera listę miejscowości spełniających podane kryteria.
/api/v2/districts.jsonp<br>/api/v2/districts |	Pobiera listę powiatów spełniających podane kryteria.
/api/v2/communitis.jsonp<br>/api/v2/communitis |	Pobiera listę gmin spełniających podane kryteria.
/api/v2/search/{pageNo}/{pageSize}.jsonp<br>/api/v2/search/{pageNo}/{pageSize} |	Pobiera listę obiektów turystycznych spełniających podane kryteria.
/api/v2/searchclose/{pageNo}/{pageSize}.jsonp<br>/api/v2/searchclose/{pageNo}/{pageSize} |	Pobiera listę obiektów turystycznych w pobliżu trasy spełniających podane kryteria.
/api/v2/categories.jsonp | Pobiera drzewo kategorii

## 3.	Pobranie listy województw

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/regions.jsonp<br>/api/v2/regions | GET | Pobiera listę województw.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
(Brak) | |

Wynik:

```json
{"data":[
{"id":1912,"name":"DOLNOŚLĄSKIE"},
...
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id województwa
name |	Nazwa

## 4.	Pobranie listy miejscowości

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/cities.jsonp<br>/api/v2/cities | GET | Pobiera listę miejscowości.<br><br>Przykład:<br><br>/api/v2/cities.jsonp?start=lub&community=lubartów&district=lubartowski&region=lubelskie

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
start | 	N | 	Początkowe litery miejscowości. Wielkość liter nie ma znaczenia.<br><br>/api/v2/cities.jsonp?start=lub
regionId | 	N | 	Id województwa.<br><br>/api/v2/cities.jsonp?regionId=1914
region | 	N | 	Nazwa województwa. Parametr jest ignorowany gdy jest podane regionId. Wielkość liter nie ma znaczenia. Nazwa musi być pełna.<br><br>/api/v2/cities.jsonp?region=lubelskie
distrcitId | 	N | 	Id powiatu.<br><br>/api/v2/cities.jsonp?districtId=1458
district | 	N | 	Nazwa powiatu. Parametr jest ignorowany gdy jest podane districtId. Wielkość liter nie ma znaczenia. Nazwa musi być pełna.<br><br>/api/v2/cities.jsonp?district=lubartowski
communityId | 	N | 	Id gminy.<br><br>/api/v2/cities.jsonp?communityId=3774
community | 	N | 	Nazwa gminy. Parametr jest ignorowany gdy jest podane communityId. Wielkość liter nie ma znaczenia. Nazwa musi być pełna.<br><br>/api/v2/cities.jsonp?community=lubartów

Wynik:

```json
{"data":[
{"id":93129,"name":"Lubartów",
"community":{"id":3774,"name":"Lubartów",
"district":{"id":1458,"name":"lubartowski",
"region":{"id":1914,"name":"LUBELSKIE"}}}}
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id miejscowości
name |	Nazwa miejscowości
community |	Obiekt opisujący gminę.
community.id |	Id gminy
community.name |	Nazwa gminy
community.district |	Obiekt opisujący powiat.
community.district.id |	Id powiatu
community.district.name |	Nazwa powiatu
community.district.region |	Obiekt opisujący województwo.
community.district.region.id |	Id województwa
community.district.region.name |	Nazwa województwa


## 5.	Pobranie listy powiatów

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/districts.jsonp<br>/api/v2/districts | GET | Pobiera listę powiatów.<br><br>Przykład:<br><br>/api/v2/districts.jsonp?start=lub&region=lubelskie

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
start |	N |	Początkowe litery powiatu. Wielkość liter nie ma znaczenia.<br><br>/api/v2/districts.jsonp?start=lub
regionId |	N |	Id województwa.<br><br>/api/v2/districts.jsonp?regionId=1914
region |	N	 |Nazwa województwa. Parametr jest ignorowany gdy jest podane regionId. Wielkość liter nie ma znaczenia. Nazwa musi być pełna.<br><br>/api/v2/districts.jsonp?region=lubelskie

Wynik:

```json
{"data":[
{"id":1458,"name":"lubartowski",
"region":{"id":1914,"name":"LUBELSKIE"}}
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id powiatu
name	 |Nazwa powiatu
region	 |Obiekt opisujący województwo.
region.id |	Id województwa
region.name |	Nazwa województwa

## 6.	Pobranie listy gmin

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/communitis.jsonp<br>/api/v2/communitis | GET | Pobiera listę gmin.<br><br>Przykład:<br><br>/api/v2/communitis.jsonp?start=lub&region=lubelskie&district=lubartowski

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
start |	N |	Początkowe litery gminy. Wielkość liter nie ma znaczenia.<br><br>/api/v2/communitis.jsonp?start=lub
regionId |	N |	Id województwa.<br><br>/api/v2/communitis.jsonp?regionId=1914
region |	N |	Nazwa województwa. Parametr jest ignorowany gdy jest podane regionId. Wielkość liter nie ma znaczenia. Nazwa musi być pełna.<br><br>/api/v2/communitis.jsonp?region=lubelskie
distrcitId |	N |	Id powiatu.<br><br>/api/v2/communitis.jsonp?districtId=1458
district |	N |	Nazwa powiatu. Parametr jest ignorowany gdy jest podane districtId. Wielkość liter nie ma znaczenia. Nazwa musi być pełna.<br><br>/api/v2/communitis.jsonp?district=lubartowski

Wynik:

```json
{"data":[
{"id":3774,"name":"Lubartów",
"district":{"id":1458,"name":"lubartowski",
"region":{"id":1914,"name":"LUBELSKIE"}}}
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id gminy
name |	Nazwa gminy
district |	Obiekt opisujący powiat.
district.id |	Id powiatu
district.name |	Nazwa powiatu
district.region |	Obiekt opisujący województwo.
district.region.id |	Id województwa
district.region.name |	Nazwa województwa

## 7.	Pobranie listy obiektów turystycznych

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/search/{pageNo}/{pageSize}.jsonp<br>/api/v2/search/{pageNo}/{pageSize}| GET | Pobiera listę obiektów turystycznych spełniających podane kryteria.<br><br>Przykład:<br><br>/api/v2/search/1/10.jsonp?lang=en&text=zamek

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
pageNo |	T |	Numer strony.<br><br>W przykładzie poniżej wartość 1 oznacza stronę 1.<br><br>/api/v2/search/1/10.jsonp
pageSIze |	T |	Rozmiar strony.<br><br>W przykładzie poniżej wartość 10 oznacza 10 obiektów na stronie.<br><br>/api/v2/search/1/10.jsonp
lang |	N |	Język. Dopuszczalne wartości: pl, en, de, fr. W przypadku braku parametry przyjmowana jest wartość: pl. Wielkość liter ma znaczenie. W przypadku użycia innej wartości przyjmowana jest wartość: pl.<br><br>/api/v2/search/1/10.jsonp?lang=en
cat |	N |	Id kategorii. W przypadku kilku kategorii wartości id podajemy po przecinku.<br><br>/api/v2/search/1/10.jsonp?cat=80,81
text |	N |	Szukany tekst. Wielkość liter nie ma znaczenia.<br><br>/api/v2/search/1/10.jsonp?text=zamek
rid |	N |	Id województwa.<br><br>/api/v2/search/1/10.jsonp?rid=1918
cid |	N |	Id miejscowości.<br><br>/api/v2/search/1/10.jsonp?cid=107124
km |	N |	Odległość od miejscowości w kilometrach. Parametr jest ignorowany gdy nie ma ustawionego cid.<br><br>/api/v2/search/1/10.jsonp?cid=107124&km=5
a[id] |	N |	Wartość atrybutu od podanym id.<br><br>Np. dla atrybutu od id=21 i wartości ‘wartosc’:<br><br>/api/v2/search/1/10.jsonp?a21=wartość<br><br>W przypadku gdy atrybut jest typu słownikowego w miejsce wartości należy podać id wartości słownikowej.<br><br>/api/v2/search/1/10.jsonp?a21=11<br><br>W przypadku gdy atrybut jest typu słownikowego z wielokrotnym wyborem należy podać id wartości słownikowych rozdzielone przecinkiem.<br><br>/api/v2/search/1/10.jsonp?a21=11,15
ascii |	N |	Czy wyszukiwanie bez diakrytyków.<br><br>Wartość true oznacza, że wyszukiwanie będzie bez diakrytyków.<br><br>Inna wartość lub brak tego parametru oznacza wyszukiwanie z diakrytykami.<br><br>/api/v2/search/1/10.jsonp?ascii=true

Wynik:

```json
{
  "data":[
    {"id":74397,
    "objectId":74397,
    "name":"Chata pod Zamkiem",
    "znw":false,
    "newObject":false,
    "rightToLeft":false,
    "categories":[],
    "location":{"east":19.651695,"north":50.464316,"name":null,"province":null,"city":null,"street":"Zamkowa","houseNumber":""},
    "score":2.1735,
    "shortText":"",
    "language":{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false}},
    ...],
  "pageInfo":{"page":1,"pageSize":8,"maxPages":83,"totalRecords":412}
}
```

Parametr wynikowy | Opis
--- | ---
id|	Id obiektu
objectId|	Id obiektu. To samo co w id.
name|	Nazwa obiektu
znw|	Czy zawsze na wierzchu? Zawsze ma wartość false. Jest wykorzystywany w Informatorze.
newObject|	Czy obiekt jest nowy?
rightToLeft|	Kierunek czytania. Wartość false oznacza z lewej do prawej.
categories|	Lista kategorii bez atrybutów. <br><br>Format pojedynczej kategorii: `{"cat_attributes":[],"id":77,"name":"Root","parentId":null}`.
location|	Położenie geograficzne obiektu.<br><br>Format: `{"east":19.651695,"north":50.464316,"name":null,"province":null,"city":null, "street":"Zamkowa","houseNumber":""}`, gdzie:<br>east – długość geograficzna wschodnia,<br>north – szerokość geograficzna północna,<br>name - nazwa,<br>province - województwo,<br>city – miejscowość <br>street - ulica,<br>houseNumber – numer domu
score|	Wartość liczbowa oznaczająca stopień dopasowania obiektu do kryteriów wyszukiwania.<br><br>Obiekty z większym score są lepiej dopasowane. Wyniki wyszukiwania są sortowane po tej wartości. Wartość score jest wyznaczana przez SOLR. Sposób wyznaczania można znaleźć pod adresem https://wiki.apache.org/solr/SolrRelevancyFAQ#How_are_documents_scored
shortText|	W przypadku wyszukiwania z parametrem text, zawiera on fragmenty tekstu w którym znaleziono szukaną frazę. Znaleziona fraza jest objęta tagiem \<em\>. Np. dla text=zamek, może mieć wartość <br><br>`"...Chata pod \<em\>Zamkiem\</em\>...\\n(...)\\n...www.gastronauci.pl/pl/5620-chata-pod-\<em\>zamkiem\</em\>-ogrodzieniec..."`
language|	Język danych obiektu.<br><br>Format: `{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false}`, gdzie:<br>id – id języka<br>name - nazwa<br>shortName- nazwa skrócona,<br>selected – nie używane w widget, zawsze false<br>rightToLeft – kierunek czytania. Wartość false oznacza z lewej do prawej.
pageInfo|	Informacja techniczna o stronicowaniu wyników wyszukiwania.<br><br>Format: `{"page":1,"pageSize":8,"maxPages":83,"totalRecords":412}}`, gdzie:<br>page – aktualna strona wyników<br>pageSize – rozmiar strony<br>maxPages – liczba stron<br>totalRecords – liczba wszystkich wyników.

## 8.	Pobiera listę obiektów turystycznych w pobliżu trasy

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/searchclose/{pageNo}/{pageSize}.jsonp<br>/api/v2/searchclose/{pageNo}/{pageSize} | GET | Pobiera listę obiektów turystycznych w pobliżu trasy spełniających podane kryteria.<br><br>Przykład:<br><br>/api/v2/searchclose/1/10.jsonp?lang=en&text=zamek

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
pageNo |	T |	Numer strony.<br><br>W przykładzie poniżej wartość 1 oznacza stronę 1.<br><br>/api/v2/searchclose/1/10.jsonp
pageSIze |	T |	Rozmiar strony.<br><br>W przykładzie poniżej wartość 10 oznacza 10 obiektów na stronie.<br><br>/api/v2/searchclose/1/10.jsonp
lang |	N |	Język. Dopuszczalne wartości: pl, en, de, fr. W przypadku braku parametry przyjmowana jest wartość: pl. Wielkość liter ma znaczenie. W przypadku użycia innej wartości przyjmowana jest wartość: pl.<br><br>/api/v2/searchclose/1/10.jsonp?lang=en
cat |	N |	Id kategorii. W przypadku kilku kategorii wartości id podajemy po przecinku.<br><br>/api/v2/searchclose/1/10.jsonp?cat=80,81
text |	N |	Szukany tekst. Wielkość liter nie ma znaczenia.<br><br>/api/v2/searchclose/1/10.jsonp?text=zamek
rid |	N |	Id województwa.<br><br>/api/v2/searchclose/1/10.jsonp?rid=1918
cid |	N |	Id miejscowości.<br><br>/api/v2/searchclose/1/10.jsonp?cid=107124
a[id] |	N |	Wartość atrybutu od podanym id.<br><br>Np. dla atrybutu od id=21 i wartości ‘wartosc’:<br><br>/api/v2/searchclose/1/10.jsonp?a21=wartość<br><br>W przypadku gdy atrybut jest typu słownikowego w miejsce wartości należy podać id wartości słownikowej.<br><br>/api/v2/searchclose/1/10.jsonp?a21=11<br><br>W przypadku gdy atrybut jest typu słownikowego z wielokrotnym wyborem należy podać id wartości słownikowych rozdzielone przecinkiem.<br><br>/api/v2/searchclose/1/10.jsonp?a21=11,15
p |	T |	Współrzędne geograficzne pojedynczego punktu trasy w formacie: szerokość geograficzna, długość geograficzna:<br><br>p=52.23212,21.06908<br><br>Całą trasę przekazujemy w postaci serii punktów.<br><br>/api/v2/searchclose//1/10.jsonp? p=52.23212,21.06908&p=52.22888,21.06358&p=52.22378,21.04136&p=...
km |	N |	Odległość od trasy w kilometrach. Parametr jest ignorowany gdy nie ma ustawionego p. W przypadku braku parametru przyjmowana jest wartość 5 km.<br><br>/api/v2/searchclose/1/10.jsonp? p=52.23212,21.06908&p=52.22888,21.06358&p=52.22378,21.04136&p=...&km=5
ascii |	N |	Czy wyszukiwanie bez diakrytyków.<br><br>Wartość true oznacza, że wyszukiwanie będzie bez diakrytyków.<br><br>Inna wartość lub brak tego parametru oznacza wyszukiwanie z diakrytykami.<br><br>/api/v2/searchclose/1/10.jsonp?ascii=true

```json
{"data":[
  {"id":74397,
  "objectId":74397,
  "name":"Chata pod Zamkiem",
  "znw":false,
  "newObject":false,
  "rightToLeft":false,
  "categories":[],
  "location":{"east":19.651695,"north":50.464316,"name":null,"province":null,"city":null,"street":"Zamkowa","houseNumber":""},
  "score":2.1735,
  "shortText":"",
  "language":{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false}
},
  ...],
  "pageInfo":{"page":1,"pageSize":8,"maxPages":83,"totalRecords":412}
}
```

Parametr wynikowy | Opis
--- | ---
id|	Id obiektu
objectId|	Id obiektu. Nie używany w widget. Jest wykorzystywany w Informatorze.
name|	Nazwa
znw|	Czy zawsze na wierzchu? Nie używany w widget. Zawsze ma wartość false. Jest wykorzystywany w Informatorze.
newObject|	Czy obiekt jest nowy?
rightToLeft|	Kierunek czytania. Wartość false oznacza z lewej do prawej.
categories|	Lista kategorii bez atrybutów. cat_atrributes ma zawsze wartość [].<br><br>Format pojedynczej kategorii taki sam jak przy opisie pobierania drzewa kategorii: `{"cat_attributes":[],"id":77,"name":"Root","parentId":null}`.
location|	Położenie geograficzne obiektu.<br><br>Format: `{"east":19.651695,"north":50.464316,"name":null,"province":null,"city":null, "street":"Zamkowa","houseNumber":""}`, gdzie:<br>east – długość geograficzna wschodnia,<br>north – szerokość geograficzna północna,<br>name - nazwa,<br>province - województwo,<br>city – miejscowość <br>street - ulica,<br>houseNumber – numer domu
score|	Wartość liczbowa oznaczająca stopień dopasowania obiektu do kryteriów wyszukiwania.<br><br>Obiekty z większym score są lepiej dopasowane. Wyniki wyszukiwania są sortowane po tej wartości. Wartość score jest wyznaczana przez SOLR. Sposób wyznaczania można znaleźć pod adresem https://wiki.apache.org/solr/SolrRelevancyFAQ#How_are_documents_scored
shortText|	W przypadku wyszukiwania z parametrem text, zawiera on fragmenty tekstu w którym znaleziono szukaną frazę. Znaleziona fraza jest objęta tagiem \<em\>. Np. dla text=zamek, może mieć wartość <br><br>`"...Chata pod \<em\>Zamkiem\</em\>...\\n(...)\\n...www.gastronauci.pl/pl/5620-chata-pod-\<em\>zamkiem\</em\>-ogrodzieniec..."`
language|	Język danych obiektu.<br><br>Format: `{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false}`, gdzie:<br>id – id języka<br>name - nazwa<br>shortName- nazwa skrócona,<br>selected – nie używane w widget, zawsze false<br>rightToLeft – kierunek czytania. Wartość false oznacza z lewej do prawej.
pageInfo|	Informacja techniczna o stronicowaniu wyników wyszukiwania.<br><br>Format: `{"page":1,"pageSize":8,"maxPages":83,"totalRecords":412}}`, gdzie:<br>page – aktualna strona wyników<br>pageSize – rozmiar strony<br>maxPages – liczba stron<br>totalRecords – liczba wszystkich wyników.

## 9.	Pobranie drzewa kategorii

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/categories.jsonp | GET | Pobiera drzewo kategorii.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
lang	 | T	 | Język. Dopuszczalne wartości: `pl`, `en`, `de`, `fr`. Wielkość liter ma znaczenie. W przypadku użycia innej wartości przyjmowana jest wartość: `pl`.<br>/api/v2/categories.jsonp?lang=en

Wynik:

```json
{"cat_attributes":[],"id":77,"name":"Root","parentId":null, "childs":[]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id kategorii
name |	Nazwa
parentId |	Id kategorii nadrzędnej. Wartość null oznacza, brak kategorii nadrzędnej.
cat_attributes |	Lista atrybutów danej kategorii. Opis struktury atrybutu w poniższej tabelce.
childs | Lista podkategorii. Każda podkategoria ma taka samą strukturę jak kategoria nadrzędna:<br>`{"cat_attributes":[],"id":78,"name":"Obiekty POI","parentId":77, "childs":[]}`


Struktura pojedynczego atrybutu:

```json
{"id":1,
"name":"Nazwa",
"type":"SHORT_TEXT",
"value":null,
"valueList":[],
"values":null,
"attributes":null,
"dictType":null}
```

Pole atrybutu | Opis
--- | ---
id |	Id atrybutu
name |	Nazwa
type |	Typ atrybutu. Przyjmuje wartości:<br>SHORT_TEXT – krótki tekst<br>LONG_TEXT – długi tekst<br>DATE – data<br>NUMBER – liczba<br>COORDINATES – współrzędne geograficzne np.: 52.247749,21.014217<br>SINGLE_LIST – lista pojedynczego wyboru, zawiera słownik dopuszczalnych wartość (patrz parametr values)<br>MULTIPLY_LIST – lista wielokrotnego wyboru, , zawiera słownik dopuszczalnych wartości (patrz parametr values)<br>COMPLEX – atrybut grupujący, która zawiera listę atrybutów.
value |	Wartość atrybutu. W przypadku pobierania drzewa kategorii zawsze ma wartość null. Jest wykorzystywany w przypadku pobierania obiektu turystycznego.
valueList |	Wartości atrybutu gdy type SINGLE_LIST lub MULTIPLY_LIST. Zawiera listę id wybranych wartości słownikowych. W przypadku pobierania drzewa kategorii zawsze ma wartość `[]`. Jest wykorzystywany w przypadku pobierania obiektu turystycznego.
values |Lista dopuszczalnych wartości słownikowych. Wypełniane tylko gdy  type SINGLE_LIST lub MULTIPLY_LIST.<br><br>Odstępstwem od tej reguły są atrybuty zawierające powiaty, gminy, miejscowości, dla nich nie jest to wypełniane (patrz parametr dictType). <br><br>Zobacz też przykład poniżej tabelki.
attributes |	Lista atrybutów. Wypełniane tylko gdy type COMPLEX.<br><br>Zobacz przykład pod tabelą.
dictType |	Typ słownika. Wypełniane tylko gdy  type SINGLE_LIST lub MULTIPLY_LIST. Może przyjmować wartości:<br>PROVINCE – słownik województw<br>DISTRICT – słownik powiatów<br>COMMUNITY – słownik gmin<br>PLACE – słownik miejscowości<br>OTHER – inny słowniki

Przykład szczególnego przypadku `values` dla atrybutu typu "województwo":

```json
{"id":9,
"name":"Województwo",
"type":"SINGLE_LIST",
"value":null,
"valueList":[],
"values":[
{"id":1912,"value":"DOLNOŚLĄSKIE"},
{"id":1913,"value":"KUJAWSKOPOMORSKIE"},
…],
"attributes":null
,"dictType":"PROVINCE"}
```

Przykład szczególnego przypadku `attributes` dla atrybutu typu COMPLEX:

```json
{"id":2,
"name":"Opis",
"type":"COMPLEX",
"value":null,
"valueList":[],
"values":null,
"attributes":[
{"id":3,"name":"Opis skrócony","type":"LONG_TEXT","value":null,"valueList":[],"values":null, "attributes":null,"dictType":null},
{"id":4,"name":"Pełny opis","type":"LONG_TEXT","value":null,"valueList":[],"values":null, "attributes":null,"dictType":null}],"dictType":null}
]}
```

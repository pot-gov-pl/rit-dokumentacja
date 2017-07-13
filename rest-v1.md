# Opis API widget Informator

Wersja 1.2, Polska Organizacja Turystyczna, Betacom S.A.

## Historia zmian

Autor | Data ukończenia |	Opis zmian |	Wersja |	Status
--- | --- | --- | --- | ---
Jacek Guz |	22.07.2014 |	Utworzenie dokumentu |	1.0 |	Do przeglądu
Jacek Guz |	14.01.2015 |	Pkt 8. Pobieranie obiektu turystycznego. W zwracanym wyniku przy opisie attachments i images dodano 2 nowe atrybuty: type i fileName. |	1.1	 | |
Jacek Guz |	21.01.2015 |	Pkt 9. Wyszukanie obiektów i pkt 10. Wyszukanie obiektów w pobliżu trasy. Dodano nowy parametr ascii. |	1.2	 | |

## 1.	Wstęp
Dokument opisuje API dla widgetu w Informatorze.

Nazwy metod nie zawierają części stałej URL. Prawidłowe wywołanie musi zawierać protokół, domenę i nazwę aplikacji np.: https://rittest.poland.travel/informator-web/api/searchWithLang/pl/1.jsonp?text=zamek&callback=responseFun.

Metody, których nazwa kończy się na ".jsonp" są przeznaczone do wywołania za pomocą cross-domain ajax. Wymagają one oprócz podania opisanych niżej parametrów, podania zawsze parametru callback. W przypadku używania JQuery, parametr ten jest dodawany automatycznie. Wystarczy wywołać w poniższy sposób:

```JavaScript
$.ajax({
url: 'https://rittest.poland.travel/informator-web/api/searchWithLang/pl/1.jsonp?text=zamek',
  dataType: 'jsonp',
  data: {},
  success: function(result) {
  	//obsługa zwróconych danych
  },
  error: function(jqXHR, textStatus, errorThrown) {
  	//obsługa błędów
  }
});
```

## 2.	Lista metod

Nazwa	| Opis
--- | ---
/api/regions.jsonp	|	Pobiera listę województw.
/api/cities.jsonp	|	Pobiera listę miejscowości spełniających podane kryteria.
/api/districts.jsonp	|	Pobiera listę powiatów spełniających podane kryteria.
/api/communitis.jsonp	|	Pobiera listę gmin spełniających podane kryteria.
/api/objects/[lang]/[object_id].jsonp	|	Pobiera obiekt turystyczny w wybranej wersji językowej.
/api/searchWithLang/[lang]/[page_no].jsonp	|	Wyszukanie listy obiektów turystycznych w wybranej wersji językowej spełniających podane kryteria.
/api/searchclose/[lang]/[page_no].jsonp	|	Wyszukanie listy obiektów turystycznych w wybranej wersji językowej spełniających podane kryteria w pobliżu podanej trasy.
/api/report/searchresult	|	Pobiera raport PDF z wyników wyszukiwania.
/api/report/object	|	Pobiera raport PDF danego obiektu.
/api/report/route	|	Pobiera raport PDF z wyznaczania trasy.
/api/files/[file_id]	|	Pobiera plik graficzny.
/api/attachments/[attachment_id]	|	Pobiera plik załącznika.
/api/categories/[lang].jsonp	|	Pobiera drzewo kategorii w wybranej wersji językowej.

## 3.	Pobranie listy województw

Wywołanie | Metoda | Opis
--- | --- | ---
/api/regions.jsonp | GET | Pobiera listę województw.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
(Brak) | |

Wynik:

```json
{"data":[
{"id":1912,"name":"DOLNOŚLĄSKIE"},
…
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id województwa
name |	Nazwa

## 4.	Pobranie listy miejscowości

Wywołanie | Metoda | Opis
--- | --- | ---
/api/cities.jsonp | GET | Pobiera listę miejscowości.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
start | 	N | 	Początkowe litery miejscowości. Wielkość liter nie ma znaczenia. Z powodów wydajnościowych najlepiej przyjąć minimum 3 pierwsze litery.<br>/api/cities.jsonp?start=lub
regionId	 | N	 | Id województwa. Z powodów wydajnościowych najlepiej używać w połączeniu z parametrem start.<br>/api/cities.jsonp?start=war&regionId=1918

Wynik:

```json
{"data":[
{"id":35707,"name":"Warchoły, gm. Miedzna, pow. węgrowski, woj. MAZOWIECKIE","regionId":1918},
…
]}
```

Parametr wynikowy | Opis
--- | ---
id	 | Id miejscowości
name	 | Nazwa (zawiera gminę, powiat i województwo)
regionId | 	Id województwa.

## 5.	Pobranie listy powiatów

Wywołanie | Metoda | Opis
--- | --- | ---
/api/districts.jsonp | GET | Pobiera listę powiatów.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
start	 | N	 | Początkowe litery powiatu. Wielkość liter nie ma znaczenia. Z powodów wydajnościowych najlepiej przyjąć minimum 3 pierwsze litery.<br>/api/districts.jsonp?start=lub

Wynik:

```json
{"data":[
{"id":1596,"name":"lubaczowski"},
…
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id powiatu
name |	Nazwa

## 6.	Pobranie listy gmin

Wywołanie | Metoda | Opis
--- | --- | ---
/api/communitis.jsonp | GET | Pobiera listę gmin.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
start	 | N	 | Początkowe litery gminy. Wielkość liter nie ma znaczenia. Z powodów wydajnościowych najlepiej przyjąć minimum 3 pierwsze litery.<br>/api/communitis.jsonp?start=lub

Wynik:

```json
{"data":[
{"id":4788,"name":"Lubaczów"},
…
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id gminy
name |	Nazwa

## 7.	Pobranie drzewa kategorii

Wywołanie | Metoda | Opis
--- | --- | ---
/api/categories/*[lang]*.jsonp | GET | Pobiera drzewo kategorii w wybranej wersji językowej. W przypadku gdy nie ma danych w wybranej wersji języka, zwracana jest wersja polska.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
lang	 | T	 | Język. Dopuszczalne wartości: `pl`, `en`, `de`, `fr`. Wielkość liter ma znaczenie. W przypadku użycia innej wartości przyjmowana jest wartość: `pl`.<br>/api/categories/pl.jsonp

Wynik:

```json
{"data":[
{"cat_attributes":[],"id":77,"name":"Root","parentId":null},
…
]}
```

Parametr wynikowy | Opis
--- | ---
id |	Id kategorii
name |	Nazwa
parentId |	Id kategorii nadrzędnej. Wartość null oznacza, brak kategorii nadrzędnej.
cat_attributes |	Lista atrybutów danej kategorii. Opis struktury atrybutu w poniższej tabelce.

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

## 8.	Pobranie obiektu turystycznego

Wywołanie | Metoda | Opis
--- | --- | ---
/api/objects/*[lang]*/*[object_id]*.jsonp | GET | Pobiera obiekt turystyczny w wybranej wersji językowej. W przypadku gdy nie ma danych w wybranej wersji języka, zwracana jest wersja polska.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
lang	 | T	 | Język. Dopuszczalne wartości: `pl`, `en`, `de`, `fr`. Wielkość liter ma znaczenie. W przypadku użycia innej wartości przyjmowana jest wartość: `pl`.<br>/api/objects/pl/50128.jsonp
object_id	 | T	 | Id obiektu.<br>/api/objects/pl/50128.jsonp

Wynik:

```json
{"data":{
"id":58903,
"objectId":null,
"name":"Restauracja na Zamku Królewskim",
"znw":false,
"newObject":false,
"rightToLeft":false,
"description":"",
"attachments":[],
"categories":[],
"images":[],
"location":{"east":20.217547,"north":50.034278,"name":"Niepołomice","province":"MAŁOPOLSKIE", "city":"Niepołomice","street":"Zamkowa 2","houseNumber":null},
"price":{"adult":0.0,"child":0.0},
"seeMoreList":[],
"time":0.0,
"language":{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false},
"startDate":null,
"endDate":null}}

```

Parametr wynikowy | Opis
--- | ---
id	| Id obiektu
objectId |	Id obiektu. Nie używany w widget. Jest wykorzystywany w Informatorze.
name |	Nazwa
znw |	Czy zawsze na wierzchu? Nie używany w widget. Zawsze ma wartość false. Jest wykorzystywany w Informatorze.
newObject |	Czy obiekt jest nowy?
rightToLeft |	Kierunek czytania. Wartość false oznacza z lewej do prawej.
description |	Opis
attachments |	Lista załączników.<br><br>Każdy załącznik ma format: `{"id": 1, "name": "nazwa_załącznika", "type"="document", "fileName"="Zeszyt1.csv"}`, gdzie:<br>type – typ załącznika, może przyjmować wartości: image, movie, document;<br>fileName – nazwa pliku.
categories |	Lista kategorii i atrybutów.<br><br>Format pojedynczej kategorii taki sam jak przy opisie pobierania drzewa kategorii: `{"cat_attributes":[],"id":77,"name":"Root","parentId":null}`.
images |	Lista zdjęć obiektu.<br><br>Każde zdjęcie ma format: `{"id": 1, "name": "nazwa_zdjęcia", "type"="image", "fileName"="zamek001.png"}`, gdzie:<br>type – typ załącznika, może przyjmować wartości: image;<br>fileName – nazwa pliku.
location |	Położenie geograficzne obiektu.<br><br>Format: `{"east":20.217547,"north":50.034278,"name":"Niepołomice","province":"MAŁOPOLSKIE", "city":"Niepołomice","street":"Zamkowa 2","houseNumber":null}`, gdzie:<br>east – długość geograficzna wschodnia,<br>north – szerokość geograficzna północna,<br>name - nazwa,<br>province - województwo, <br>city – miejscowość, <br>street - ulica,<br>houseNumber – numer domu.
price |	Cena biletu:<br>adult – dla dorosłego, <br>child – dla dziecka.
seeMoreList |	Lista obiektów powiązanych.<br><br>Każdy obiekt powiązany ma format:`{"id": 1, "name": "nazwa_obiektu"}`.
time |	Czas zwiedzania w godzinach.
language |	Język danych obiektu.<br><br>Format: `{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false}`, gdzie: id – id języka,<br>name - nazwa, <br>shortName- nazwa skrócona,<br>selected – nie używane w widget, zawsze false, <br>rightToLeft – kierunek czytania. Wartość false oznacza z lewej do prawej.
startDate |	Data ważności od
endDate |	Data ważności do


## 9.	Wyszukanie obiektów

Wywołanie | Metoda | Opis
--- | --- | ---
/api/searchWithLang/*[lang]*/*[page_no]*.jsonp | GET | Wyszukuje obiekty turystyczne spełniające zadane parametry. Metoda wspiera stronicowanie.<br><br>Rozmiar strony stały i ma wartość 8.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
lang|	T|	Język. Dopuszczalne wartości: pl, en, de, fr. Wielkość liter ma znaczenie. W przypadku użycia innej wartości przyjmowana jest wartość: pl.<br><br>/api/searchWithLang/pl/1.jsonp
page_no|	T|	Numer strony.<br><br>/api/searchWithLang/pl/1.jsonp
cat|	N|	Id kategorii. W przypadku kilku kategorii wartości id podajemy po przecinku.<br><br>/api/searchWithLang/pl/1.jsonp?cat=80,81
text|	N|	Szukany tekst. Wielkość liter nie ma znaczenia.<br><br>/api/searchWithLang/pl/1.jsonp?text=zamek
rid|	N|	Id województwa.<br><br>/api/searchWithLang/pl/1.jsonp?rid=1918
cid|	N|	Id miejscowości.<br><br>/api/searchWithLang/pl/1.jsonp?cid=107124
km|	N|	Odległość od miejscowości w kilometrach. Parametr jest ignorowany gdy nie ma ustawionego cid.<br><br>/api/searchWithLang/pl/1.jsonp?cid=107124&km=5
a[id]|	N|	Wartość atrybutu od podanym id.<br><br>Np. dla atrybutu od id=21 i wartości ‘wartosc’:<br>/api/searchWithLang/pl/1.jsonp?a21=wartość<br><br>W przypadku gdy atrybut jest typu słownikowego w miejsce wartości należy podać id wartości słownikowej:<br>/api/searchWithLang/pl/1.jsonp?a21=11<br><br>W przypadku gdy atrybut jest typu słownikowego z wielokrotnym wyborem należy podać id wartości słownikowych rozdzielone przecinkiem:<br>/api/searchWithLang/pl/1.jsonp?a21=11,15
ascii|	N|	Czy wyszukiwanie bez diakrytyków.<br><br>Wartość true oznacza, że wyszukiwanie będzie bez diakrytyków.<br><br>Inna wartość lub brak tego parametru oznacza wyszukiwanie z diakrytykami.<br><br>/api/searchWithLang/pl/1.jsonp?ascii=true

Wynik:

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
shortText|	W przypadku wyszukiwania z parametrem text, zawiera on fragmenty tekstu w którym znaleziono szukaną frazę. Znaleziona fraza jest objęta tagiem \<em\>. Np. dla text=zamek, może mieć wartość <br>"...Chata pod \<em\>Zamkiem\</em\>...\\n(...)\\n...www.gastronauci.pl/pl/5620-chata-pod-\<em\>zamkiem\</em\>-ogrodzieniec..."
language|	Język danych obiektu.<br><br>Format: `{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false}`, gdzie:<br>id – id języka<br>name - nazwa<br>shortName- nazwa skrócona,<br>selected – nie używane w widget, zawsze false<br>rightToLeft – kierunek czytania. Wartość false oznacza z lewej do prawej.
pageInfo|	Informacja techniczna o stronicowaniu wyników wyszukiwania.<br><br>Format: `{"page":1,"pageSize":8,"maxPages":83,"totalRecords":412}}`, gdzie:<br>page – aktualna strona wyników<br>pageSize – rozmiar strony<br>maxPages – liczba stron<br>totalRecords – liczba wszystkich wyników.


## 10.	Wyszukanie obiektów w pobliżu trasy

Wywołanie | Metoda | Opis
--- | --- | ---
/api/searchclose/*[lang]*/*[page_no]*.jsonp | GET | Wyszukuje obiekty turystyczne spełniające zadane parametry w pobliżu trasy. Metoda wspiera stronicowanie.<br><br>Rozmiar strony stały i ma wartość 8.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
lang|	T|	Język. Dopuszczalne wartości: pl, en, de, fr. Wielkość liter ma znaczenie. W przypadku użycia innej wartości przyjmowana jest wartość: pl.<br><br>/api/searchclose/pl/1.jsonp
page_no|	T|	Numer strony.<br><br>/api/searchclose/pl/1.jsonp
cat|	N|	Id kategorii. W przypadku kilku kategorii wartości id podajemy po przecinku.<br><br>/api/searchclose/pl/1.jsonp?cat=80,81
text|	N|	Szukany tekst. Wielkość liter nie ma znaczenia.<br><br>/api/searchclose/pl/1.jsonp?text=zamek
rid|	N|	Id województwa.<br><br>/api/searchclose/pl/1.jsonp?rid=1918
cid|	N|	Id miejscowości.<br><br>/api/searchclose/pl/1.jsonp?cid=107124
a[id]|	N|	Wartość atrybutu od podanym id.<br><br>Np. dla atrybutu od id=21 i wartości ‘wartosc’:<br>/api/searchclose/pl/1.jsonp?a21=wartość<br><br>W przypadku gdy atrybut jest typu słownikowego w miejsce wartości należy podać id wartości słownikowej:<br>/api/searchclose/pl/1.jsonp?a21=11<br><br>W przypadku gdy atrybut jest typu słownikowego z wielokrotnym wyborem należy podać id wartości słownikowych rozdzielone przecinkiem:<br>/api/searchclose/pl/1.jsonp?a21=11,15
p | N | Współrzędne geograficzne pojedynczego punktu trasy w formacie: szerokość geograficzna, długość geograficzna:<br><br>p=52.23212,21.06908<br><br>Całą trasę przekazujemy w postaci serii punktów.<br><br>/api/searchclose/pl/1.jsonp? p=52.23212,21.06908&p=52.22888,21.06358&p=52.22378,21.04136&p=...
km|	N|	Odległość od trasy w kilometrach. Parametr jest ignorowany gdy nie ma ustawionego p. W przypadku braku parametru przyjmowana jest wartość 5 km<br><br>/api/searchclose/pl/1.jsonp? p=52.23212,21.06908&p=52.22888,21.06358&p=52.22378,21.04136&p=...&km=5
ascii|	N|	Czy wyszukiwanie bez diakrytyków.<br><br>Wartość true oznacza, że wyszukiwanie będzie bez diakrytyków.<br><br>Inna wartość lub brak tego parametru oznacza wyszukiwanie z diakrytykami.<br><br>/api/searchclose/pl/1.jsonp?ascii=true

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
shortText|	W przypadku wyszukiwania z parametrem text, zawiera on fragmenty tekstu w którym znaleziono szukaną frazę. Znaleziona fraza jest objęta tagiem \<em\>. Np. dla text=zamek, może mieć wartość <br>"...Chata pod \<em\>Zamkiem\</em\>...\\n(...)\\n...www.gastronauci.pl/pl/5620-chata-pod-\<em\>zamkiem\</em\>-ogrodzieniec..."
language|	Język danych obiektu.<br><br>Format: `{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false}`, gdzie:<br>id – id języka<br>name - nazwa<br>shortName- nazwa skrócona,<br>selected – nie używane w widget, zawsze false<br>rightToLeft – kierunek czytania. Wartość false oznacza z lewej do prawej.
pageInfo|	Informacja techniczna o stronicowaniu wyników wyszukiwania.<br><br>Format: `{"page":1,"pageSize":8,"maxPages":83,"totalRecords":412}}`, gdzie:<br>page – aktualna strona wyników<br>pageSize – rozmiar strony<br>maxPages – liczba stron<br>totalRecords – liczba wszystkich wyników.

## 11.	Pobranie pliku graficznego

Wywołanie | Metoda | Opis
--- | --- | ---
/api/files/*[file_id]* | GET |Pobiera plik graficzny. Metoda przeznaczona do użycia z tagiem `<img>`.<br>Nie zaleca się używać w widget z uwagi na możliwe duże rozmiary plików.<br>`<img scr="/api/files/11209">`

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
file_id	 | T	 | Id pliku. Zwykle jest brany z atrybutu attachments z wyniku pobrania obiektu (patrz Pobierz obiekt turystyczny).<br>/api/attachments/11209

Wynik - w nagłówku response ustawiane jest:
```
Content-Disposition: inline; filename*=UTF-8'nazwa_pliku
```

## 12.	Pobranie pliku załącznika

Wywołanie | Metoda | Opis
--- | --- | ---
/api/attachments/*[attachment_id]* | GET |Pobiera plik załącznika. Metoda przeznaczona do użycia z tagiem `<a>`.<br>Nie zaleca się używać w widget z uwagi na możliwe duże rozmiary plików.<br>`<a href="/api/attachments/11209">`

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
attachment_id	 | T	 | Id pliku. Zwykle jest brany z atrybutu attachments z wyniku pobrania obiektu (patrz Pobierz obiekt turystyczny).<br>/api/attachments/11209

Wynik - w nagłówku response ustawiane jest:
```
Content-Disposition: inline; filename*=UTF-8'nazwa_pliku
```

## 13.	Pobranie raportu PDF w wyników wyszukiwania

Wywołanie | Metoda | Opis
--- | --- | ---
/api/report/searchresult | POST |Pobiera raport PDF z wyników wyszukiwania.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
searchresult	 | T	 | Obiekt zwrócony przez metodę pobierającą obiekt turystyczny

Przykład parametru wejściowego:

```json
[{
"name": "Chata pod Zamkiem"
"categories": [],
"location":{"east":19.651695,"north":50.464316,"name":null,"province":null, "city":null,"street":"Zamkowa","houseNumber":""},
"score":1.0,
"shortText": ""
},
…]
```

Wynik - w nagłówku response ustawiane jest:
```
Content-Disposition: inline; filename*=UTF-8'nazwa_pliku
```

## 14.	Pobranie raportu PDF z danego obiektu

Wywołanie | Metoda | Opis
--- | --- | ---
/api/report/object | POST |Pobiera raport PDF z danego obiektu.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
object	 | T	 | Obiekt zwrócony przez metodę pobierającą obiekt turystyczny

Przykład parametru wejściowego:

```json
{
"id":58903,
"objectId":null,
"name":"Restauracja na Zamku Królewskim",
"znw":false,
"newObject":false,
"rightToLeft":false,
"description":"",
"attachments":[],
"categories":[],
"images":[],
"location":{"east":20.217547,"north":50.034278,"name":"Niepołomice","province":"MAŁOPOLSKIE", "city":"Niepołomice","street":"Zamkowa 2","houseNumber":null},
"price":{"adult":0.0,"child":0.0},
"seeMoreList":[],
"time":0.0,
"language":{"id":45,"name":"Polski","shortName":"pl-PL","selected":false,"rightToLeft":false},
"startDate":null,
"endDate":null
}

```

Wynik - w nagłówku response ustawiane jest:
```
Content-Disposition: inline; filename*=UTF-8'nazwa_pliku
```

## 15.	Pobranie raportu PDF z wyznaczania trasy

Wywołanie | Metoda | Opis
--- | --- | ---
/api/report/route | POST |Pobiera raport PDF z wyznaczania trasy.

Parametr wejściowy | Wymagany | Opis
--- | --- | ---
route	 | T	 | Wynik wyszukiwania

Przykład parametru wejściowego:

```json
{
"startPoint":{"name":"Warszawa","east":19.651695,"north":50.464316},
"finishPoint":{"name":"Zakopane","east":19.651695,"north":50.464316},
"instructions": [{"description":"jedź prosto","distance":"12 km"},…],
"distance": 352,
"route":[{"name":"","east":19.651695,"north":50.464316},…],
}
```

Wynik - w nagłówku response ustawiane jest:
```
Content-Disposition: inline; filename*=UTF-8'nazwa_pliku
```

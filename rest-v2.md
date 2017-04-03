# Opis API Informator v2

Wersja 1.1, Polska Organizacja Turystyczna, Betacom S.A.

## Historia zmian

Autor | Data ukończenia |	Opis zmian |	Wersja |	Status
--- | --- | --- | --- | ---
Jacek Guz |	18.11.2014 |	Utworzenie dokumentu |	1.0 |	Do przeglądu
Jacek Guz |	21.01.2015 |	Pkt 7. Pobranie listy obiektów turystycznych i pkt 8. Pobranie listy obiektów turystycznych w pobliżu trasy. Dodano nowy parametr ascii. |	1.1	 | |

## 1.	Wstęp
Dokument opisuje API REST Informatora w wersji v2.

## 2.	Lista metod
Nazwa	| Opis
--- | ---
/api/v2/regions.jsonp |	Pobiera listę województw.
/api/v2/cities.jsonp |	Pobiera listę miejscowości spełniających podane kryteria.
/api/v2/districts.jsonp |	Pobiera listę powiatów spełniających podane kryteria.
/api/v2/communitis.jsonp |	Pobiera listę gmin spełniających podane kryteria.
/api/v2//search/{pageNo}/{pageSize}.jsonp |	Pobiera listę obiektów turystycznych spełniających podane kryteria.
/api/v2//searchclose/{pageNo}/{pageSize}.jsonp |	Pobiera listę obiektów turystycznych w pobliżu trasy spełniających podane kryteria.

## 3.	Pobranie listy województw

Wywołanie | Metoda | Opis
--- | --- | ---
/api/v2/regions.jsonp | GET | Pobiera listę województw.

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

## 5.	Pobranie listy powiatów

## 6.	Pobranie listy gmin

## 7.	Pobranie listy obiektów turystycznych

## 8.	Pobiera listę obiektów turystycznych w pobliżu trasy

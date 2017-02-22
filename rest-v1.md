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

## 4.	Pobranie listy miejscowości

## 5.	Pobranie listy powiatów

## 6.	Pobranie listy gmin

## 7.	Pobranie drzewa kategorii

## 8.	Pobranie obiektu turystycznego

## 9.	Wyszukanie obiektów

## 10.	Wyszukanie obiektów w pobliżu trasy

## 11.	Pobranie pliku graficznego

## 12.	Pobranie pliku załącznika

## 13.	Pobranie raportu PDF w wyników wyszukiwania

## 14.	Pobranie raportu PDF z danego obiektu

## 15.	Pobranie raportu PDF z wyznaczania trasy

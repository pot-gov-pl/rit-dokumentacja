# Użytkowanie interfejsów WSDL

Wersja 7.1, Polska Organizacja Turystyczna, Betacom S.A.

## Historia zmian

Autor | Data ukończenia |	Opis zmian |	Wersja |	Status
--- | --- | --- | --- | ---
Robert Stepuch |	2013-03-21 |	Utworzenie dokumentu |	1.0 |	Do przeglądu
Robert Stepuch |	2013-09-20 |	Nowy rozdział: Przykładowy skrypt PHP |	2.0 |	Do przeglądu
Robert Stepuch |	2013-12-10 |	Nowy rozdział: Nowy webservice: CollectTouristObjectsCache |	3.0 |	Do przeglądu
Grzegorz Kowalski |	2014-03-27 |	Dodatkowy przykład w PHP; Informacje o środowisku testowym; Zmiany kosmetyczne |	3.1a |	Do przeglądu
Jacek Guz |	2014-04-09 |	Uszczegółowienie sposobu przekazywania danych województw, powiatów, gmin, miejscowości. |	4.0 |	Do przeglądu
Grzegorz Kowalski |	2014-04-09 |	Uzupełnienie (Rozdział 8, Ad. 4) |	4.0a |	Do przeglądu
Jacek Guz |	2014-07-21 |	Poprawienie opisu operacji getReport. |	5.0 |	Do przeglądu
Jacek Guz |	2016-07-26 |	Nowy webservice: GetTouristObjectLanguages |	6.0 |	Do przeglądu
Jacek Guz |	2016-09-13 |	Nowy webservice GetTouristObjectEvents |	7.0 |	Do przeglądu
Grzegorz Kowalski |	2017-01-19 |	Usunięcie przykładów łączenia z webserwisami w PHP |	7.1 |	Do przeglądu

## Spis treści

## 1.	Słownik

**System RIT** – Repozytorium Informacji Turystycznej - system informatyczny złożony z:
- bazy danych obiektów turystycznych – zwanej Bazą RIT,
-	mechanizmów wymiany danych z systemami zewnętrznymi – zwanych zespołem interfejsów wymiany danych,
- aplikacji zarządzającej bazą danych oraz zarządzającej mechanizmami wymiany danych – zwanej Aplikacją RIT,

**System zewnętrzny** – dowolna baza danych oraz oprogramowanie, przechowujący i przetwarzający dane o obiektach turystycznych. 

**Przekazanie danych, przekazanie** – System zewnętrzny przekazuje dane do Systemu RIT. Stroną inicjującą połączenie może być System zewnętrzny lub System RIT. Przekazanie danych, to kierunek przepływu danych pomiędzy systemami, gdzie System zewnętrzny dostarcza danych, a System RIT, przetwarza je w celu umieszczenia ich, w swojej bazie danych.

**Pobranie danych, pobranie** – System zewnętrzny pobiera dane z Systemu RIT. Stroną inicjującą połączenie może być System zewnętrzny lub System RIT. Pobranie danych, to kierunek przepływu danych pomiędzy systemami, gdzie System RIT dostarcza dane, a System zewnętrzny, przetwarza je w celu umieszczenia ich, w swojej bazie danych.

**Obiekt turystyczny** –  dokładnie jeden obiekt z dziedziny obiektów turystycznych (hotel, restauracja, atrakcja turystyczna, wydarzenie itp.).

**Kanał dystrybucji** - umowna nazwa, zestawu usług, technologii i konfiguracji, umiejscowionych w systemie RIT, pozwalająca na wymianę danych ( przekazanie i pobranie ), pomiędzy Systemem zewnętrznym a Systemem RIT.

## 2.	Udostępnione interfejsy w postaci webservice

Na potrzeby integracji zostały przygotowane interfejsy WSDL:

-	MetadataOfRIT.wsdl - pobranie zawartości bazy merytorycznej: informacji o strukturze kategorii i atrybutów, opisujących Obiekty turystyczne,
-	GiveTouristObjects.wsdl – przekazanie danych o obiektach turystycznych do Systemu RIT,
-	CollectTouristObjects.wsdl – pobranie danych o obiektach turystycznych z Systemu RIT,
-	CollectTouristObjectsCache.wsdl – pobranie danych o obiektach turystycznych z Systemu RIT,
-	GetTouristObjectLanguages.wsdl – pobranie danych o wersjach językowych obiektu z Systemu RIT,
-	oraz serwis HTTP do udostępniania plików graficznych.

Implementacja interfejsów została wykonana z wykorzystaniem standardu **JAX-WS**. 

Implementacja JAX-WS to **Apache CXF w wersji 2.5.1**. 

Serwer web, udostępniający RIT webservices, to: **Apache HTTP Server 2.2.23**.

Styl kodowania treści komunikatu w RIT webservices to: **document/literal**.

Strona kodowa treści komunikatu w RIT webservices to: **UTF-8**.

Metoda autentykacji do RIT webservices to: **client certificate** (obsługa autentykacji po stronie serwera Apache).

Webservices są dostępne pod adresami ( odpowiednio ):
-	https://intrit.poland.travel/rit/integration/GiveTouristObjects?wsdl
-	https://intrit.poland.travel/rit/integration/MetadataOfRIT?wsdl
-	https://intrit.poland.travel/rit/integration/CollectTouristObjects?wsdl
-	https://intrit.poland.travel/rit/integration/CollectTouristObjectsCache?wsdl
-	https://intrit.poland.travel/rit/integration/GetTouristObjectLanguages?wsdl 
-	https://intrit.poland.travel/rit/integration/GetTouristObjectEvents?wsdl

Serwis do udostępniania plików binarnych:
-	https://intrit.poland.travel/rit/integration/getfile?fileid=????

Środowisko testowe RIT jest dostępne pod adresami:
-	https://intrittest.poland.travel/rit/integration/GiveTouristObjects?wsdl
-	https://intrittest.poland.travel/rit/integration/MetadataOfRIT?wsdl
-	https://intrittest.poland.travel/rit/integration/CollectTouristObjects?wsdl
-	https://intrittest.poland.travel/rit/integration/CollectTouristObjectsCache?wsdl
-	https://intrittest.poland.travel/rit/integration/GetTouristObjectLanguages?wsdl 
-	https://intrittest.poland.travel/rit/integration/GetTouristObjectEvents?wsdl
-	https://intrittest.poland.travel/rit/integration/getfile?fileid=????

## 3.	Wspólne elementy

### Metryka

### Obiekt turystyczny

### Raport z wykonania

## 4.	Operacje w CollectTouristObjects

### Operacja searchTouristObjects

## 5.	Operacje w CollectTouristObjectsCache

### Operacja searchTouristObjectsInCache

## 6.	Operacje w GiveTouristObjects

### Operacja addModifyObject

### Operacja addModifyObjects

### Operacja delObject

### Operacja delObjects

### Operacja getReport

## 7.	Operacje w MetadataOfRIT

### Operacja getMetadataOfRIT

## 8.	Operacje w GetTouristObjectLanguages

### Operacja getLanguages

## 9.	Operacje w GetTouristObjectEvents

### Operacja getEvents

## 10.	Atrybuty, kategorie i języki narodowe w RIT

### Ad 1) Kategorie

### Ad 2) Atrybuty

### Ad 3) Wersja językowa

### Ad 4) Dane adresowe obiektu

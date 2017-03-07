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

1.	[Słownik](#1---słownik)
2.	[Udostępnione interfejsy w postaci webservice](#2---udostępnione-interfejsy-w-postaci-webservice)
3.	[Wspólne elementy](#3---wspólne-elementy)
  - [Metryka](#metryka)
  - [Obiekt turystyczny](#obiekt-turystyczny)
  - [Raport z wykonania](#raport-z-wykonania)
4.	[Operacje w CollectTouristObjects](#4---operacje-w-collecttouristobjects)
  - [Operacja searchTouristObjects](#operacja-searchtouristobjects)
5.	[Operacje w CollectTouristObjectsCache](#5---operacje-w-collecttouristobjectscache)
  - [Operacja searchTouristObjectsInCache](#operacja-searchTouristObjectsInCache)
6.	[Operacje w GiveTouristObjects](#6---operacje-w-givetouristobjects)
  - [Operacja addModifyObject](#operacja-addmodifyobject)
  - [Operacja addModifyObjects](#operacja-addmodifyobjects)
  - [Operacja delObject](#operacja-delobject)
  - [Operacja delObjects](#operacja-delobjects)
  - [Operacja getReport](#operacja-getreport)
7.	[Operacje w MetadataOfRIT](#7---operacje-w-metadataofrit)
  - [Operacja getMetadataOfRIT](#operacja-getmetadataofrit)
8.	[Operacje w GetTouristObjectLanguages](#8---operacje-w-gettouristobjectlanguages)
  - [Operacja getLanguages](#operacja-getLanguages)
9.	[Operacje w GetTouristObjectEvents](#9---operacje-w-gettouristobjectevents)
  - [Operacja getEvents](#operacja-getevents)
10.	[Atrybuty, kategorie i języki narodowe w RIT](#10---atrybuty-kategorie-i-języki-narodowe-w-RIT)
  - [Ad 1) Kategorie](#ad-1-kategorie)
  - [Ad 2) Atrybuty](#ad-2-atrybuty)
  - [Ad 3) Wersja językowa](#ad-3-wersja-językowa)
  - [Ad 4) Dane adresowe obiektu](#ad-4-dane-adresowe-obiektu)

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

Przy opisie użyto, miejsc użycia elementów, które są skrótami dla:
-	GIVE – wszystkich operacji zawartych w interfejsie GiveTouristObjects.wsdl
-	COLLECT – wszystkich operacji zawartych w interfejsie CollectTouristObjects.wsdl

Jeśli któryś z elementów zostanie użyty w niewłaściwym miejscu, nie spowoduje to błędu, zaś taki element zostanie/powinien zostać  zignorowany.

Przeznaczanie elementów jest takie same w obydwu miejscach użycia GIVE/COLLECT. Jeśli jest inaczej ujęto to w opisie tego elementu.

### Metryka

Element 'metric' ma za zadanie przekazać do Systemu RIT informacje o żądającym. Jest wykorzystywana w każdym z WebServices.
Przykład:

```xml
  <col:metric>
    <met:distributionChannel></met:distributionChannel>
    <met:username></met:username>
    <met:requestUniqueIdentifier></met:requestUniqueIdentifier>
    <met:requestDate></met:requestDate>
  </col:metric>
```

Opis elementów:

Element	| Przeznaczenie	| Używany	| Typ XML	| Krotność
---	| ---	| ---	| ---	| ---
distributionChannel	|	Nazwa kanału dystrybucji, przypisanego indywidualnie dla każdego Systemu zewnętrznego. Nazwę należy pobrać od administratora Systemu RIT w POT.	|	GIVE/COLLECT	|	string	|	1..1
username	|	Nazwa użytkownika z Systemu zewnętrznego uruchamiającego webservice. Nazwa użytkownika jest tylko zapisywana do logu tekstowego w celach informacyjnych. 	|	GIVE/COLLECT	|	string	|	1..1
requestUniqueIdentifier	|	Dowolny ciąg znaków jednoznacznie identyfikujący wywołanie po stronie Systemu zewnętrznego. Ciąg znaków jest tylko zapisywana do logu tekstowego, w celach informacyjnych.  W przypadku problemów, z pracą webservice, można posłużyć się  tym identyfikatorem do ustalenia, wywołania które spowodowało problem.	|	GIVE/COLLECT |	string	|	1..1
requestDate	|	Data i czas systemowy na maszynie z której jest uruchamiany webservice. Data i czas są zapisywane do logu tekstowego w celach informacyjnych.	|	GIVE/COLLECT	|	date	|	1..1

### Obiekt turystyczny

Element 'touristObject' opisuje obiekt turystyczny. Jest wykorzystywany do przekazywania/pobierania danych do/z Systemu RIT. W zależności od kierunku przepływu danych, niektóre elementy są pomijane lub dodatkowo wykorzystywane do opisu obiektu turystycznego.

Opis jednego obiektu turystycznego składa się z głównych elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
touristObjectIdentifierRIT	|	Element użyty w jednej z operacji GIVE przekazuje informację o identyfikatorze obiektu, który jednoznacznie identyfikuje obiekt w Systemie RIT. Aby element mógł być użyty System zewnętrzny musi uprzednio pobrać identyfikator obiektu z Systemu RIT i przechować go w własnej bazie danych. W operacji przekazania do Systemu RIT, element ten nie może współistnieć z elementem: touristObjectIdentifierSZ.<br><br>Element użyty w jednej z operacji COLLECT przekazuje informację o identyfikatorze obiektu, który jednoznacznie identyfikuje obiekt w Systemie RIT. W operacji pobrania z Systemu RIT element ten może współistnieć z elementem: touristObjectIdentifierSZ.<br><br>Opis podelementów w tabelce: touristObjectIdentifierRIT.	|	GIVE/COLLECT	|	Element	|	0..1
touristObjectIdentifierSZ	|	Element użyty w jednej z operacji GIVE przekazuje informację o identyfikatorze obiektu, który jednoznacznie identyfikuje obiekt w Systemie zewnętrznym.  W operacji przekazania do Systemu RIT, element ten nie może współistnieć z elementem: touristObjectIdentifierRIT. W operacji GIVE element jest obowiązkowy.<br><br>Element użyty w jednej z operacji COLLECT przekazuje informację o identyfikatorach obiektu, w innych systemach zewnętrznych, z których została pobrane dane opisujące obiekt. W operacji pobrania z Systemu RIT element ten może współistnieć z elementem: touristObjectIdentifierSZ. W operacji COLLECT element jest opcjonalny.<br><br>Opis podelementów w tabelce: touristObjectIdentifierSZ.	|	GIVE/COLLECT	|	Element	|	0..unbounded
lastReleasedInRIT	|	Data i czas ostatniej modyfikacji w Systemie RIT. 	|	COLLECT	|	date	|	0..1
categories	|	Lista kategorii do której przypisany jest obiekt. Kategorie są zgodne z słownikiem kategorii w Systemie RIT, do pobrania za pomocą webservice: MetadataOfRIT.wsdl.<br><br>Opis podelementów w tabelce: categories.	|	GIVE/COLLECT	|	Element	|	1..1
attributes	|	Lista atrybutów które posiada obiekt. Atrybuty są zgodne z słownikiem atrybutów w Systemie RIT, do pobrania za pomocą webservice: MetadataOfRIT.wsdl.<br><br>Opis podelementów w tabelce: attributes.	|	GIVE/COLLECT	|	Element	|	1..1
binaryDocuments	|	Lista dokumentów jakie są skojarzone z obiektem turystycznym.<br><br>Opis podelementów w tabelce: binaryDocuments.	|	GIVE/COLLECT	|	Element	|	0..1

Element: **touristObjectIdentifierRIT**

Przykład:
```xml
<touristObjectIdentifierRIT>
    <identifierRIT><identifierRIT>
    <lastModified><lastModified>
    <version><version>
<touristObjectIdentifierRIT>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
identifierRIT	|	Identyfikator w Systemie RIT.	|	GIVE/COLLECT	|	int	|	1..1
lastModified	|	Data ostatniej modyfikacji w Systemie  RIT.	|	COLLECT	|	date	|	0..1
version	|	Wersja obiektu w Systemie RIT.	|	COLLECT	|	string	|	0..1

Element: **touristObjectIdentifierSZ**

Przykład:
```xml
<touristObjectIdentifierSZ>
   <identifierType><identifierType>
   <distributionChannel>
       <name><name>
       <code><code>
   <distributionChannel>
   <artificialIdentifier><artificialIdentifier>
   <databaseTable><databaseTable>
   <concatenationOfField><concatenationOfField>
   <lastModified><lastModified>
<touristObjectIdentifierSZ>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
identifierType	|	Typ używanego identyfikatora.<br>-	typ I1 - sztuczny identyfikator bazodanowy ( artificialIdentifier )<br>-	typ I2 - nazwa tabeli + sztuczny identyfikator bazodanowy  ( databaseTable + artificialIdentifier )<br>-	typ I3 – konkatenacja wybranych ( niezmiennych ) pól tekstowych obiektu ( concatenationOfField )	|	GIVE/COLLECT	|	enumeration: [I1,I2,I3]	|	1..1
distributionChannel	|	Kanał dystrybucji z którego pochodzi danych obiekt:<br>- name – ciąg znaków będący nazwą kanału<br>- code – kod kanału, jednoznacznie identyfikujący kanał, pod jaką jest on zapisany w Systemie RIT.	|	COLLECT	|	Element	|	0..1
artificialIdentifier	|	Sztuczny identyfikator obiektu w Systemie zewnętrznym. Element obowiązkowy dla identifierType=I1 i identifierType=I2.	|	GIVE/COLLECT	|	string	|	0..1
databaseTable	|	Nazwa tabeli bazodanowej w której przechowywany jest sztuczny identyfikator. Element obowiązkowy dla identifierType=I2.	|	GIVE/COLLECT	|	string	|	0..1
concatenationOfField	|	Dowolny ciąg znaków jednoznacznie identyfikujący obiekt w Systemie zewnętrznym. Element obowiązkowy dla identifierType=I3.	|	GIVE/COLLECT	|	string	|	0..1
lastModified	|	Data i czas ostatniej modyfikacji w Systemie zewnętrznym.	|	GIVE	|	date	|	0..1


Element: **categories**

Przykład:
```xml
 <categories>
  <category>
     <met1:code></met1:code>
     <met1:name></met1:name>
     <met1:description></met1:description>
     <met1:attributeCodes>
        <met1:attributeCode></met1:attributeCode>
     </met1:attributeCodes>
  <category>
<categories>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
category	|	Element opisujący dokładnie jedną kategorię.	| 	GIVE/COLLECT	|	Element	|	1..unbouded
code	|	Kod kategorii do której przypisany jest obiekt.	|	GIVE/COLLECT	|	string	|	1..1
name	|	Nazwa kategorii w Systemie RIT	|	COLLECT	|	string	|	0..1
description	|	Opis kategorii w systemie RIT. Używane tylko w interfejsie MetadataOfRIT.wsdl.	|	--	|	string	|	0..1
attributeCodes	|	Lista kodów atrybutów używana do opisania kategorii. Używane tylko w interfejsie MetadataOfRIT.wsdl.	|	--	|	Element	|	0..unbounded
attributeCode	|	Kod atrybutu używany do opisania kategorii. Używane tylko w interfejsie MetadataOfRIT.wsdl.	|	--	|	string	|	1..unbounded

Element: **attributes**

Przykład:
```xml
<attributes>
   <attribute code="">
      <attrVals language="">
         <value><value>
      <attrVals>
      <complex>
         <compAttrVals code="">
           <attrVals language="">
              <value><value>
            <attrVals>
         <compAttrVals>
      <complex>
      <parentCategoryCode><parentCategoryCode>
   <attribute>
<attributes>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
attribute	|	Element opisujący dokładnie jeden atrybut.	| 	GIVE/COLLECT	|	Element	|	1..unbounded
attribute ->code	|	Kod atrybutu który opisuje obiekt. Kody atrybutów są zgodne z słownikiem atrybutów w Systemie RIT, do pobrania za pomocą webservice: MetadataOfRIT.wsdl.<br>Atrybut 'code' elementu 'attribute' jest wymagany.	|	GIVE/COLLECT	|	string	|	1..1
attribute ->attrVals	|	Element pozwalający na opisanie listy wartości atrybutów.  Każda lista wartości atrybutów ma podany język w którym są zapisane dane. Dla jednego elementu 'attribute', może istnieć wiele 'attrVals' ale każdy musi mieć inny język. <br><br>Opis podelementów w tabelce: attrVals.	|	GIVE/COLLECT	|	element	|	0..unbounded
attribute ->complex	|	Element pozwalający na przekazywanie złożonych atrybutów. Zawiera listę wartości atrybutów.<br><br>Każda lista wartości atrybutów ma podany język w którym są zapisane dane. Dla jednego elementu 'attribute', może istnieć wiele 'attrVals' ale każdy musi mieć inny język. <br><br>Opis podelementów w tabelce: compAttrVals.	|	GIVE	|	element	|	0..unbounded
parentCategoryCode	|	Element opisuje listę kodów kategorii do których należy dany atrybut. W aktualnej implementacji lista będzie liczyć zawsze jedną pozycję.	|	COLLECT	|	string	|	0..unbounded

Element: **attrVals**

Przykład:
```xml
 <attrVals language="pl-PL">
     <value>Tekst<value>
 <attrVals>
 ```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
attrVals	|	Element przechowujący wartości atrybutów w określonym języku.	|	GIVE/COLLECT	|	element	|	0..unbounded
attrVals->language	|	Język w którym przechowywane są wartości atrybutów.<br><br>Atrybut elementu może zawierać:<br>- ciąg znaków „all” który mówi, że wartość atrybutu jest taka sama we wszystkich językach, w jakich został zdefiniowany atrybut obiektu turystycznego. Używany tylko w GIVE.<br>- locale dla języka w formacie xx-XX. Lista dozwolonych atrybutów jest do pobrania za pomocą webservice: MetadataOfRIT.wsdl ( ritDictionary → languages ).	|	GIVE/COLLECT	|	string	|	1..1
attrVals->value	|	Wartość atrybutu lub lista wartości atrybutu.<br>Lista wartości może składać się z:<br>- jednej  wartości (np. dla atrybutu ulica będzie to 'ul. Warszawska')<br>- wielu wartości (np. dla atrybutu udogodnienia będzie to: TV/SAT, WiFi, łazienka)<br><br>W przypadku atrybutów słownikowych takich jak województwo, powiat, gmina, miejscowość podajemy czystym tekstem nazwę np.: „Mazowieckie”, „Warszawa” itp.<br>W przypadku przekazywania danych do POT (GIVE), można również podawać zamiast nazwy ID województwa, ID powiatu, ID gminy, ID miejscowości. ID musi być zgodne z ID bazy POT (zawartość tabel rit_province, rit_district, rit_community, rit_place).	|	GIVE/COLLECT	|	string	|	1..unbounded

Element: **compAttrVals**

Przykład:
```xml
<compAttrVals code="">
    <attrVals language="">
      <value><value>
    <attrVals>
<compAttrVals>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
compAttrVals->code	|	Kod atrybutu który opisuje obiekt. Kody atrybutów są zgodne z słownikiem atrybutów w Systemie RIT, do pobrania za pomocą webservice: MetadataOfRIT.wsdl.<br><br>Atrybut 'code' elementu 'compAttrVals' jest wymagany.	|	GIVE	|	string	|	1..1
compAttrVals->attrVals	|	Element pozwalający na opisanie listy wartości atrybutów.  Każda lista wartości atrybutów ma podany język w którym są zapisane dane. Dla jednego elementu 'attribute', może istnieć wiele 'attrVals' ale każdy musi mieć inny język.<br><br>Opis podelementów w tabelce: attrVals.	|	GIVE	|	element	|	0..unbounded

Element: **binaryDocuments**

Przykład:
```xml
<binaryDocuments>
   <documentURL>
	  <fileName><fileName>
	  <fileType><fileType>
	  <URL><URL>
	  <certificate>
		 <validTo><validTo>
		 <distributionChannelOwner><distributionChannelOwner>
	  <certificate>
   <documentURL>
   <documentFile>
	  <fileName><fileName>
	  <fileType><fileType>
	  <relativePathToDirectory>out_files<relativePathToDirectory>
	  <certificate>
		 <validTo><validTo>
		 <distributionChannelOwner><distributionChannelOwner>
	  <certificate>
   <documentFile>
   <documentBase64>
	  <fileName><fileName>
	  <fileType><fileType>
	  <encoded><encoded>
	  <certificate>
		 <validTo><validTo>
		 <distributionChannelOwner><distributionChannelOwner>
	  <certificate>
   <documentBase64>
<binaryDocuments>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
documentURL	|	Element do przekazania dokumentu binarnego dostępnego jako adres URL.<br><br>Opis podelementów w tabelce: documentURL.	|	GIVE/COLLECT	|	element	|	0..unbounded
documentFile	|	Element do przekazania dokumentu binarnego dostępnego jako plik umieszczony na serwerze FTP w POT.<br><br>Opis podelementów w tabelce: documentFile.	|	GIVE	|	element	|	0..unbounded
documentBase64	|	Element do przekazania dokumentu binarnego dostępnego jako strumień zakodowany w Base64.<br><br>Opis podelementów w tabelce: documentBase64.	|	GIVE	|	element	|	0..unbounded

Element: **documentURL**

Przykład:
```xml
   <documentURL>
	  <fileName><fileName>
	  <fileType><fileType>
	  <URL><URL>
	  <certificate>
		 <validTo><validTo>
		 <distributionChannelOwner><distributionChannelOwner>
	  <certificate>
   <documentURL>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
fileName	|	Przyjazna nazwa tego pliku lub nazwa pliku  z rozszerzeniem. W tym elemencie należy podać tekst który będzie widziany przez użytkownika i na jego podstawie, będzie mógł rozróżnić plik spośród innych przypisanych do obiektu turystycznego.<br>W przypadku braku takiej nazwy należy podać nazwę pliku z rozszerzeniem.	|	GIVE/COLLECT	|	string	|	1..1
fileType	|	Typ pliku (rozszerzenie pliku lub ContentType dla pliku)	|	GIVE/COLLECT	|	string	|	1..1
URL	|	Adres URL wskazujący na plik binarny.<br><br>Dla GIVE, jest to dowolny adres internetowy dostępny dla systemu RIT. Adres powinien korzystać z metody 'HTTP', nie być zabezpieczony żadną z metod autentykacji.<br><br>Dla COLLECT, jest to adres internetowy, dla części integracyjnej aplikacji RIT. Pliki udostępnianie pod tym adresem będą dostępne przez 7 dni od daty pobrania obiektu z systemu RIT.	|	GIVE/COLLECT	|	string	|	1..1
certificate	|	Element opisuje pozwolenie na użytkowanie pliku udzielone przez POT / udostępniającego plik.	|	COLLECT	|	element	|	0..1
certificate ->validTo	|	Data do  pozwolenia na użytkowanie pliku.	|	COLLECT	|	date	|	1..1
certificate ->distributionChannelOwner	|	Źródłowy kanał dystrybucji z którego pozyskano plik.	| 	COLLECT	|	string	|	1..1

Element: **documentFile**

Przykład:
```xml
  <documentFile>
	  <fileName><fileName>
	  <fileType><fileType>
	  <relativePathToDirectory><relativePathToDirectory>
	  <certificate>
		 <validTo><validTo>
		 <distributionChannelOwner><distributionChannelOwner>
	  <certificate>
   <documentFile>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
fileName | Przyjazna nazwa tego pliku lub nazwa pliku  z rozszerzeniem. W tym elemencie należy podać tekst który będzie widziany przez użytkownika i na jego podstawie, będzie mógł rozróżnić plik spośród innych przypisanych do obiektu turystycznego.<br><br>W przypadku braku takiej nazwy należy podać nazwę pliku z rozszerzeniem.	|	GIVE/COLLECT	|	string	|	1..1
fileType	|	Typ pliku (rozszerzenie pliku lub ContentType dla pliku)	|	GIVE/COLLECT	|	string	|	1..1
relativePathToDirectory	|	Relatywna ścieżka do katalogu na serwerze FTP w POT, w którym został umieszczony plik.	| 	GIVE/COLLECT	|	string	|	1..1
certificate	|	Element opisuje pozwolenie na użytkowanie pliku udzielone przez POT / udostępniającego plik.	|	COLLECT	|	element	|	0..1
certificate->validTo	|	Data do  pozwolenia na użytkowanie pliku.	|	COLLECT	|	date	|	1..1
certificate ->distributionChannelOwner	|	Źródłowy kanał dystrybucji z którego pozyskano plik.	| 	COLLECT	|	string	|	1..1

Element: **documentBase64**

Przykład:
```xml
 <documentBase64>
	  <fileName><fileName>
	  <fileType><fileType>
	  <encoded><encoded>
	  <certificate>
		 <validTo><validTo>
		 <distributionChannelOwner><distributionChannelOwner>
	  <certificate>
   <documentBase64>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
fileName	|	Przyjazna nazwa tego pliku lub nazwa pliku  z rozszerzeniem. W tym elemencie należy podać tekst który będzie widziany przez użytkownika aplikacji RIT i na jego podstawie, będzie mógł rozróżnić plik spośród innych przypisanych do obiektu turystycznego.<br><br>W przypadku braku takiej nazwy należy podać nazwę pliku z rozszerzeniem.	|	GIVE/COLLECT	|	string	|	1..1
fileType	|	Typ pliku (rozszerzenie pliku lub ContentType dla pliku)	|	GIVE/COLLECT	|	string	|	1..1
encoded	|	Plik binarny zakodowany w Base64.	|	GIVE/COLLECT	|	string	|	1..1
certificate	|	Element opisuje pozwolenie na użytkowanie pliku udzielone przez POT / udostępniającego plik.	|	COLLECT	|	element	|	0..1
certificate ->validTo	Data	| do  pozwolenia na użytkowanie pliku.	|	COLLECT	|	date	|	1..1
certificate ->distributionChannelOwner	|	Źródłowy kanał dystrybucji z którego pozyskano plik.	| 	COLLECT	|	string	|	1..1

### Raport z wykonania

Element 'report' zawiera w sobie tylko elementy ' reportForObject' który zawierają raport/informację o statusie i błędach jakie stwierdzono w czasie przetwarzania obiektu.

Przykład:
```xml
<report>
	<reportForObject>
	   <identifierRIT>
		  <identifierRIT><identifierRIT>
		  <lastModified>2014-09-19<lastModified>
	   <identifierRIT>
	   <identifierSZ>
		  <identifierType><identifierType>
		  <artificialIdentifier><artificialIdentifier>
		  <databaseTable><databaseTable>
		  <concatenationOfField><concatenationOfField>
	   <identifierSZ>
          <positionInXML></positionInXML>
	   <objectState><objectState>
	   <reportLine>
		  <lineType><lineType>
		  <textLine><textLine>
	   <reportLine>
	<reportForObject>
<report>
```

Opis elementów:

Element |	Przeznaczenie |	Używany |	Typ XML |	Krotność
---	| ---	| ---	| ---	| ---
report	|	Element grupujący elementy 'reportForObject'.	|	GIVE	|	element	| 1..1
reportForObject	|	Element zawierający raport tylko dla jednego obiektu turystycznego.	|	GIVE	|	element	|	1..unbounded
identifierRIT	|	Element opisuje obiekt którego dotyczy dany raport. Dla operacji przekazania danych do RIT, są to skopiowane dane identyfikacyjne, podane przy wywołaniu operacji z GiveTouristObjects.wsdl.<br><br>W operacji przekazania do Systemu RIT element ten nie może współistnieć z elementem: identifierSZ i positionInXML.<br><br>Opis podelementów w tabelce: touristObjectIdentifierRIT.	|	GIVE	|	element	|	0..1
identifierSZ	|	Element opisuje obiekt którego dotyczy dany raport. Dla operacji przekazania danych do RIT, są to skopiowane dane identyfikacyjne, podane przy wywołaniu operacji z GiveTouristObjects.wsdl.<br><br>W operacji przekazania do Systemu RIT element ten nie może współistnieć z elementem: identifierRIT i positionInXML.<br><br>Opis podelementów w tabelce: touristObjectIdentifierSZ.	|	GIVE	|	element	|	0..1
positionInXML	|	Element opisuje pozycję danego obiektu w przekazanym XML, jeśli nie udało się zidentyfikować obiektu za pomocą elementów:  identifierSZ i  identifierRIT.	|	GIVE	|	string	|	0..1
objectState	|	Status obiektu po przetworzeniu:<br>- OK – obiekt turystyczny bez błędów zapisany w bazie Systemu RIT<br>- WARNING – obiekt turystyczny z ostrzeżeniami, zapisany w bazie Systemu RIT<br>- ERROR – obiekt turystyczny z błędami, nie zapisany w bazie Systemu RIT	|	GIVE	|	string	|	1..1
reportLine	|	Jedna linijka raportu ( zazwyczaj jeden wyodrębniony błąd / komunikat ).	| GIVE	|	element	|	0..unbounded
reportLine->lineType	|	Typ linijki raportu:<br>- WARNING – ostrzeżenie o nieprawidłowości, nieblokujące zapisu do bazy danych Systemu RIT<br>- ERROR – błąd blokujący zapis do bazy danych Systemu RIT	|	GIVE	|	string	|	1..1
reportLine->textLine	|	Treść linijki raportu.	|	GIVE	|	string	|	1..1


## 4.	Operacje w CollectTouristObjects

### Operacja searchTouristObjects

Element 'searchCondition' zawiera kryteria po jakich mają być przeszukiwane obiekty w bazie danych Systemu RIT.  **Wyszukiwanie odbywa się tylko i wyłącznie wśród obiektów przypisanych w RIT do danego kanały dystrybucji.**

Element searchCondition w żądaniu (request) ma następującą postać:

```xml
 <searchCondition>
	<language></language>
	<allForDistributionChannel></allForDistributionChannel>
	<searchAttributeAnd>
	   <attributeValue>
		  <attributeCode></attributeCode>
		  <valueToSearch></valueToSearch>
	   </attributeValue>
	</searchAttributeAnd>
	<searchCategoryAnd>
	   <categoryCode></categoryCode>
	</searchCategoryAnd>
	<objectIdentifier>
	   <identifierRIT></identifierRIT>
	   <identifierSZ>
		  <identifierType></identifierType>
		  <artificialIdentifier>1</artificialIdentifier>
		  <databaseTable>obiekt</databaseTable>
		  <concatenationOfField></concatenationOfField>
	   </identifierSZ>
	</objectIdentifier>
	<lastModifiedRange>
	   <dateFrom></dateFrom>
	   <dateTo></dateTo>
	</lastModifiedRange>
</searchCondition>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
language	|	Język w którym zostaną udostępnione wartości wszystkich atrybutów, opisujących obiekt turystyczny.	|	string	|	1..1
allForDistributionChannel	|	Jeśli element zostanie podany z wartością 'true' to:<br>- udostępnione zostaną wszystkie obiekty dostępne dla kanału dystrybucji,<br>- elementy  searchAttributeAnd,  objectIdentifier,  searchCategoryAnd,  lastModifiedRange nie będą brane pod uwagę w kryteriach wyszukiwania.	|	boolean	|	0..1
searchAttributeAnd	|	Element pozwala na podanie wartości dowolnego atrybutu po którym odbywać się ma wyszukiwanie obiektów. Wyszukane zostaną wszystkie obiekty. Które posiadają danych atrybut o określonej wartości. Elementy  allForDistributionChannel,  objectIdentifier,  searchCategoryAnd,  lastModifiedRange nie będą brane pod uwagę w kryteriach wyszukiwania.<br><br>Opis podelementów w tabelce: searchAttributeAnd.	|	element	|	0..1
searchCategoryAnd	|	Element pozwala na podanie kodu dowolnej kategorii Systemu RIT. Wyszukane zostaną wszystkie obiekty należące do tych kategorii. Elementy  allForDistributionChannel,  searchAttributeAnd, objectIdentifier,  lastModifiedRange nie będą brane pod uwagę w kryteriach wyszukiwania.	|	element	|	0..1
objectIdentifier	|	Element pozwala na podanie identyfikatorów RIT lub identyfikatorów z systemu zewnętrznego po których ma być przeszukiwana baza Systemu RIT.<br><br>Elementy  allForDistributionChannel,  searchAttributeAnd, searchCategoryAnd,  lastModifiedRange nie będą brane pod uwagę w kryteriach wyszukiwania.<br><br>Opis podelementów w tabelkach: touristObjectIdentifierRIT, touristObjectIdentifierSZ.	|	element	|	0..1
lastModifiedRange	|	Element pozwala na podanie zakresu dat  lub jednego z krańców zakresu dat, odnoszących się do daty ostatniej modyfikacji obiektu turystycznego w Systemie RIT. Zwracane są wszystkie obiekty dostępne dla kanału dystrybucji w podanym zakresie dat.<br><br>Jedna z dat: dateFrom,  dateTo musi być zawsze podana.<br><br>Elementy  allForDistributionChannel,  searchAttributeAnd, searchCategoryAnd, objectIdentifier nie będą brane pod uwagę w kryteriach wyszukiwania.	|	element	|	0..1
lastModifiedRange->dateFrom	|	Dolny kraniec ograniczenia w zakresie dat.	|	date	|	0..1
lastModifiedRange->dateTo	|	Górny kraniec ograniczenia w zakresie dat.	|	date	|	0..1


Element 'CollectTouristObjectResponse' w odpowiedzi (response) ma następującą postać:

```xml
   <CollectTouristObjectResponse>
         <status></status>
         <info></info>
         <touristObject>
         …....
         <touristObject>
   <CollectTouristObjectResponse>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
status	|	Status odpowiedzi:<br>- OK – wyszukiwanie przebiegło bez błędów<br>- ERROR – podczas wyszukiwania pojawił się błąd wynikający z nieprawidłowo podanych parametrów wyszukania. Więcej informacji w elemencie 'info'.	| string	|	0..1
info	|	Element zawiera informację o ilości znalezionych rekordów lub treść błędu wynikającego z nieprawidłowo podanych parametrów wyszukania.		| string	|	0..1
touristObject	|	Elementy opisujące znalezione obiekty turystyczne. Opis elementu w rozdziale: Wspólne elementy → Obiekt turystyczny niniejszego dokumentu.	|	|

## 5.	Operacje w CollectTouristObjectsCache

### Operacja searchTouristObjectsInCache

Element 'searchCondition' zawiera kryteria po jakich mają być przeszukiwane obiekty w bazie danych Systemu RIT.  **Wyszukiwanie odbywa się tylko i wyłącznie wśród obiektów przypisanych w RIT do danego kanały dystrybucji.**

Dane są pobierane z cache, utworzonego w systemie RIT. Cache jest uaktualniany codziennie, w godzinach nocnych. Zawiera on wstępnie przygotowane dane o wszystkich obiektach dostępnych w systemie RIT. Zadaniem operacji jest  udostępnienie danych o obiektach, w znacznie krótszym czasie niż operacja 'searchTouristObject' w webservice 'CollectTouristObjects'. Operacja posiada 2 zoptymalizowane funkcjonalności:
-	wybór wszystkich obiektów dostępnych dla danego kanału dystrybucji
-	wybór obiektów zmodyfikowanych w określonym przedziale czasu

Element searchConditionInCache w żądaniu ( request ) ma następującą postać:

```xml
 <searchCondition>
	<language></language>
	<allForDistributionChannel></allForDistributionChannel>
	<lastModifiedRange>
	   <dateFrom></dateFrom>
	   <dateTo></dateTo>
	</lastModifiedRange>
</searchCondition>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
language	| 	Język w którym zostaną udostępnione wartości wszystkich atrybutów, opisujących obiekt turystyczny.		| string	| 	1..1
allForDistributionChannel		| Jeśli element zostanie podany z wartością 'true' to:<br>- udostępnione zostaną wszystkie obiekty dostępne dla kanału dystrybucji,<br>- elementy  searchAttributeAnd,  objectIdentifier,  searchCategoryAnd,  lastModifiedRange nie będą brane pod uwagę w kryteriach wyszukiwania. 	| 	boolean		| 0..1
lastModifiedRange		| Element pozwala na podanie zakresu dat  lub jednego z krańców zakresu dat, odnoszących się do daty ostatniej modyfikacji obiektu turystycznego w Systemie RIT. Zwracane są wszystkie obiekty dostępne dla kanału dystrybucji w podanym zakresie dat.<br><br>Jedna z dat: dateFrom,  dateTo musi być zawsze podana.<br><br>Elementy  allForDistributionChannel,  searchAttributeAnd, searchCategoryAnd, objectIdentifier nie będą brane pod uwagę w kryteriach wyszukiwania.	| 	element	| 	0..1
lastModifiedRange->dateFrom	| 	Dolny kraniec ograniczenia w zakresie dat.	| 	date	| 	0..1
lastModifiedRange->dateTo	| 	Górny kraniec ograniczenia w zakresie dat.	| 	date	| 	0..1

Element 'CollectTouristObjectCacheResponse' w odpowiedzi ( response ) ma następującą postać:

```xml
   <CollectTouristObjectResponse>
         <status></status>
         <info></info>
         <touristObject>
         …....
         <touristObject>
   <CollectTouristObjectResponse>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
status	|	Status odpowiedzi:<br>- OK – wyszukiwanie przebiegło bez błędów<br>- ERROR – podczas wyszukiwania pojawił się błąd wynikający z nieprawidłowo podanych parametrów wyszukania. Więcej informacji w elemencie 'info'.	| string	|	0..1
info	|	Element zawiera informację o ilości znalezionych rekordów lub treść błędu wynikającego z nieprawidłowo podanych parametrów wyszukania.		| string	|	0..1
touristObject	|	Elementy opisujące znalezione obiekty turystyczne. Opis elementu w rozdziale: Wspólne elementy → Obiekt turystyczny niniejszego dokumentu.	|	|

## 6.	Operacje w GiveTouristObjects

### Operacja addModifyObject

Operacja **addModifyObject** jest przeznaczona do przekazywania danych jednego obiektu turystycznego, do systemu RIT. W czasie wywołania operacji, przekazywany obiekt jest przetwarzany w Systemie RIT i dopiero po przetworzeniu zwracany jest wynik operacji.   Operacja w wyniku działania zwraca raport opisujący zawierający status wykonania i ewentualne błędy stwierdzone podczas wykonania.

Element AddModifyObjectRequest w żądaniu ( request ) ma następującą postać:

```xml
<AddModifyObjectRequest>
 <metric>
	.....
 </metric>
 <touristObject>
  .....
 </touristObject>
</AddModifyObjectRequest>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
metric		| Opis elementu w rozdziale: **Wspólne elementy → Metryka** niniejszego dokumentu.	| 	element	| 	1..1
touristObject	| 	Elementy opisujące znalezione obiekty turystyczne. Opis elementu w rozdziale: Wspólne elementy → Obiekt turystyczny niniejszego dokumentu.	| element	| 	1..1


Element AddModifyObjectResponse w odpowiedzi ( response ) ma następującą postać:

```xml
<AddModifyObjectResponse>
	 <report>
	  .....
	 </report>
 </AddModifyObjectResponse>
 ```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
report		| Opis elementu w rozdziale: **Wspólne elementy → Raport z wykonania** niniejszego dokumentu.	| element	| 	1..1

### Operacja addModifyObjects

Operacja **addModifyObjects** jest przeznaczona do przekazywania danych więcej niż jednego obiektu turystycznego, do systemu RIT. W czasie wywołania operacji, przekazywany obiekt nie jest przetwarzany w Systemie RIT, ale jest rejestrowany do późniejszego przetworzenia. Obiekty przekazane za pomocą tej operacji są  przetwarzane po odesłaniu odpowiedzi do klienta wywołującego tą operację.  Operacja w wyniku działania zwraca identyfikator transakcji, pod którym zostało zarejestrowane to żądanie.

Element AddModifyObjectsRequest w żądaniu ( request ) ma następującą postać:

```xml
<AddModifyObjectRequest>
 <metric>
	.....
 </metric>
 <touristObject>
  .....
 </touristObject>
</AddModifyObjectRequest>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
metric		| Opis elementu w rozdziale: **Wspólne elementy → Metryka** niniejszego dokumentu.		| element	| 	1..1
touristObject	| 	Elementy opisujące znalezione obiekty turystyczne. Opis elementu w rozdziale: **Wspólne elementy → Obiekt turystyczny** niniejszego dokumentu.	| 	element	| 	1..unbounded


Element AddModifyObjectsResponse w odpowiedzi ( response ) ma następującą postać:

```xml
<AddModifyObjectsResponse>
   <transactionIdentifier></transactionIdentifier>
</AddModifyObjectsResponse>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
transactionIdentifier		| Unikalny identyfikator żądania. Wartość wymagana do pobrania raportu za pomocą operacji getReport.		| element		| 1..1

### Operacja delObject

Operacja **delObject** jest przeznaczona do usuwania danych jednego obiektu turystycznego, z systemu RIT. Obiekt taki musi być uprzednio choć raz przekazany lub pobrany do/z Systemu RIT, za pomocą kanału dystrybucji. Operacja w wyniku działania zwraca raport opisujący zawierający status wykonania i ewentualne błędy stwierdzone podczas wykonania.

Element DelObjectRequest w żądaniu ( request ) ma następującą postać:

```xml
<DelObjectRequest>
	 <metric>
	  ….....
	 </metric>
	 <identifierRIT>
		…...........
	 </identifierRIT>
	 <identifierSZ>
	…...........
	 </identifierSZ>
</DelObjectRequest>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
metric		| Opis elementu w rozdziale: Wspólne elementy → Metryka niniejszego dokumentu.	| 	element	| 	1..1
identifierRIT		| Element opisuje dokładnie jeden obiekt w Systemie RIT. Element nie może współistnieć wraz z elementem: identifierSZ.<br><br>Opis podelementów w tabelce: touristObjectIdentifierRIT. 	| 	element		| 0..1
identifierSZ		| Element opisuje dokładnie jeden obiekt w Systemie RIT. Element nie może współistnieć wraz z elementem: identifierRIT.<br><br>Opis podelementów w tabelce: touristObjectIdentifierSZ. 	| element	| 	0..1


Element DelObjectResponse w odpowiedzi ( response ) ma następującą postać:

```xml
<DelObjectResponse>
	 <report>
	  .....
	 </report>
 </DelObjectResponse>
 ```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
report	| 	Opis elementu w rozdziale: **Wspólne elementy → Raport** z wykonania niniejszego dokumentu.	| 	element		| 1..1


### Operacja delObjects

Operacja delObjects jest przeznaczona do usuwania danych wielu obiektów turystycznych, z systemu RIT. Obiekty takie muszą być uprzednio choć raz przekazane lub pobrane do/z Systemu RIT, za pomocą kanału dystrybucji. W czasie wywołania operacji, usuwany obiekt nie jest przetwarzany w Systemie RIT, ale jest rejestrowany do późniejszego przetworzenia. Obiekty przekazane za pomocą tej operacji są  przetwarzane, po odesłaniu odpowiedzi do klienta wywołującego tą operację.  Operacja w wyniku działania zwraca identyfikator transakcji, pod którym zostało zarejestrowane to żądanie.

Element DelObjectsRequest w żądaniu ( request ) ma następującą postać:

```xml
<DelObjectsRequest>
	 <metric>
	  ….....
	 </metric>
	 <identifierRIT>
		…...........
	 </identifierRIT>
	 <identifierSZ>
	…...........
	 </identifierSZ>
</DelObjectsRequest>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
metric		| Opis elementu w rozdziale: **Wspólne elementy → Metryka** niniejszego dokumentu.		| element		| 1..1
identifierRIT	| 	Element opisuje dokładnie jeden obiekt w Systemie RIT. Element nie może współistnieć wraz z elementem: identifierSZ.<br><br>Opis podelementów w tabelce: touristObjectIdentifierRIT.	| element	| 	0..unbounded
identifierSZ		| Element opisuje dokładnie jeden obiekt w Systemie RIT. Element nie może współistnieć wraz z elementem: identifierRIT.<br><br>Opis podelementów w tabelce: touristObjectIdentifierSZ.	| element	| 	0..unbounded


Element DelObjectsResponse w odpowiedzi ( response ) ma następującą postać:

```xml
<DelObjectsResponse>
	 <transactionIdentifier></transactionIdentifier>
 </DelObjecstResponse>
 ```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
transactionIdentifier		| Unikalny identyfikator żądania. Wartość wymagana do pobrania raportu za pomocą operacji getReport.		| element		| 1..1


### Operacja getReport

Operacja **delObjects** jest przeznaczona do usuwania danych wielu obiektów turystycznych, z systemu RIT. Obiekty takie muszą być uprzednio choć raz przekazane lub pobrane do/z Systemu RIT, za pomocą kanału dystrybucji. W czasie wywołania operacji, usuwany obiekt nie jest przetwarzany w Systemie RIT, ale jest rejestrowany do późniejszego przetworzenia. Obiekty przekazane za pomocą tej operacji są  przetwarzane, po odesłaniu odpowiedzi do klienta wywołującego tą operację.  Operacja w wyniku działania zwraca identyfikator transakcji, pod którym zostało zarejestrowane to żądanie.
Element GetReportRequest w żądaniu ( request ) ma następującą postać:

```xml
<GetReportRequest>
 <metric>
  ....
 </metric>
 <transactionIdentifier></transactionIdentifier>
</GetReportRequest>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
metric	| Opis elementu w rozdziale: Wspólne elementy → Metryka niniejszego dokumentu.	| element	| 1..1
transactionIdentifier	| Unikalny identyfikator żądania. Wartość uzyskana z wyniku działania operacji addModifyObjects, delObjects.	| element	| 1..1

Element GetReportResponse w odpowiedzi ( response ) ma następującą postać:

```xml
<GetReportResponse>
  <status></status>
  <info></info>
  <report>

  </report>
</GetReportResponse>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
status		| Status odpowiedzi:<br>- OK – wyszukiwanie przebiegło bez błędów<br>- ERROR – podczas wyszukiwania pojawił się błąd wynikający z nieprawidłowo podanych parametrów wyszukania. Więcej informacji w elemencie 'info'.	| string	| 1..1
info	| Element zawiera informację o ilości znalezionych rekordów lub treść błędu wynikającego z nieprawidłowo podanych parametrów wyszukania.	| string	| 1..1
report	| Opis elementu w rozdziale: **Wspólne elementy → Raport z wykonania** niniejszego dokumentu.	| element	| 0..unbounded


## 7.	Operacje w MetadataOfRIT

### Operacja getMetadataOfRIT

Operacja pozwala na pobranie bazy merytorycznej Systemu RIT.

Element  MetadataRequest w żądaniu ( request ):

```xml
 <MetadataRequest>
         <language></language>
         <metric>
          …......
         <metric>
 </MetadataRequest>
 ```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
language	| Język w którym zostanie udostępniona zawartość bazy merytorycznej Systemu RIT.	| string	| 1..1
metric	| Opis elementu w rozdziale: Wspólne elementy → Metryka niniejszego dokumentu.	| element	| 1..1

Element  MetadataResponse w odpowiedzi ( response ):

```xml
  <MetadataResponse xmlns:ns3="http://www.pot.gov.pl/rit/integration/commonElements/RITException" xmlns:ns2="http://www.pot.gov.pl/rit/integration/commonElements/Metric" xmlns="http://www.pot.gov.pl/rit/integration/MetadataOfRIT">
    <lastModificationDate></lastModificationDate>
    <ritCategory>
       <code></code>
       <name></name>
       <description></description>
       <attributeCodes>
         <attributeCode></attributeCode>
       </attributeCodes>
    </ritCategory>
…..
   <ritAttribute>
       <code></code>
       <name></name>
       <description></description>
       <typeValidator></typeValidator>
       <typeUI></typeUI>
       <dictionaryCode></dictionaryCode>
       <complexAttribute>
          <attributeCode></attributeCode>
       </complexAttribute>
    </ritAttribute>
…..
    <ritDictionary>
       <code></code>
       <name></name>
       <value></value>
    </ritDictionary>
</MetadataResponse>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
lastModificationDate		| Data ostatniej modyfikacji bazy merytorycznej. Jeśli którykolwiek z składowych bazy merytorycznej ulegnie zmianie, to w tym elemencie będzie znajdować się data ostatniej zmiany. Porównując tą wartość do daty ostatniego pobrania, można ustalić czy baza merytoryczna uległa zmianie.	| 	date	| 	1..1
ritCategory	| 	Element opisujący zawartość słownika kategorii w Systemie RIT. <br><br>Opis podelementów w tabelce: *ritCategory*.	| 	element	| 	0..unbounded
ritAttribute	| 	Element opisujący zawartość słownika atrybutów w Systemie RIT. <br><br>Opis podelementów w tabelce: *ritAttribute*.	| 	element	| 	0..unbounded
ritDictionary	| 	Element opisujący zawartość słowników zdefiniowanych w  Systemie RIT,  na potrzeby atrybutów typu słownikowego.<br><br>Opis podelementów w tabelce: *ritDictionary*.<br><br>**Uwaga. Dane słowników województw, powiatów, gmin, miejscowości nie są zwracane.**	| 	element	| 	0..unbounded

Element  *ritCategory*

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
code	| 	Unikalny kod kategorii do której przypisany jest obiekt.	| string	| 	1..1
name	| 	Nazwa kategorii w Systemie RIT.	| string	| 1..1
description	| 	Opis kategorii w systemie RIT. Używane tylko w interfejsie MetadataOfRIT.wsdl.	| string	| 0..1
attributeCodes	| 	Lista kodów atrybutów które opisują kategorię.	| element	| 0..1
attributeCode	| 	Kod atrybutu używany do opisania kategorii. Używane tylko w interfejsie MetadataOfRIT.wsdl.	| string	| 0..unbounded

Element  *ritAttribute*

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
code	| Unikalny kod atrybutu.	| 	string	| 	1..1
name	| 	Nazwa atrybutu w Systemie RIT	| 	string	| 	1..1
description	| 	Opis atrybutu w systemie RIT.	|  	string	| 	0..1
typeValidator	| 	Dopuszczalny typ danych, zgodnie z którym wartości atrybutów będą sprawdzane przy przekazywaniu do Systemu RIT:<br>- SHORT_TEXT, LONG_TEXT – dowolny ciąg znaków<br>- NUMBER – liczba całkowita<br>- MULTIPLY_LIST lub SINGLE_LIST  – atrybut typu słownikowego wymagany element 'dictionaryCode'<br>- COMPLEX – atrybut typu słownikowego wymagany element 'complexAttribute'<br>- BOOLEAN – typ boolean 'true' lub 'false'<br>- DATE – typ data	| 	string	| 	1..1
typeUI	| 	Typ danych który zostanie użyty do zaprezentowania wartości atrybutu w aplikacji wewnętrznej  i informatorze, w RIT. Element nie jest obowiązkowy i jest tylko sugestią sposobu prezentacji wartości atrybutu. <br><br>**Aktualnie lista typów UI nie jest ustalona.**	| 	string	| 	0..1
maxLength	| 	Maksymalna wielkość wartości atrybutu dla atrybutu którego typeValidator = STRING.	| 	string	| 	0..1
dictionaryCode	| Kod słownika w  Systemie RIT, na potrzeby atrybutów typu słownikowego. Element wypełniony gdy element typeValidator = 'MULTIPLY_LIST' lub 'SINGLE_LIST'<br><br>Patrz element: ritDictionary. 	| string	| 0..1
complexAttribute	| 	Lista kodów atrybutów składających się na atrybut. Element wypełniony gdy element typeValidator = 'COMPLEX'.	|  	element	| 	0..1
complexAttribute->attributeCode	| 	Kody atrybutów które składają się na typ złożony atrybutu.	| 	string	| 	0..unbounded

Element  *ritDictionary*

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
code	| Unikalny kod słownika.	| 	string	| 	1..1
name	| 	Nazwa słownika w Systemie RIT	| 	string	| 	1..1
value	| 	Pozycje słownika.		| string	| 	0..unbounded


## 8.	Operacje w GetTouristObjectLanguages

### Operacja getLanguages

Operacja pozwala na pobranie wersji językowych dla podanego obiektu.

Element GetTouristObjectLanguagesRequest  w żądaniu ( request ) ma następującą postać:

```xml
 <GetTouristObjectLanguagesRequest>
<metric>
	…
	</metric>
	<objectIdentifier>
	   <identifierRIT></identifierRIT>
	   <identifierSZ>
		  <identifierType></identifierType>
		  <artificialIdentifier></artificialIdentifier>
		  <databaseTable>obiekt</databaseTable>
		  <concatenationOfField></concatenationOfField>
	   </identifierSZ>
	</objectIdentifier>
</GetTouristObjectLanguagesRequest>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
metric	| 	Opis elementu w rozdziale: Wspólne elementy → Metryka niniejszego dokumentu.	| 	string	| 	1..1
identifierRIT	| 	Element opisuje dokładnie jeden obiekt w Systemie RIT. Element nie może współistnieć wraz z elementem: identifierSZ.<br><br>Opis podelementów w tabelce: *touristObjectIdentifierRIT*.	| boolean	| 0..1
identifierSZ	| 	Element opisuje dokładnie jeden obiekt w Systemie RIT. Element nie może współistnieć wraz z elementem: identifierRIT.<br><br>Opis podelementów w tabelce: *touristObjectIdentifierSZ*.	| element	| 0..1


Element GetTouristObjectLanguagesResponse w odpowiedzi ( response ) ma następującą postać:

```xml
<GetTouristObjectLanguagesResponse>
	<language></language>
</GetTouristObjectLanguagesResponse>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
language		| Kod wersji językowej np.: pl-PL, de-DE itp.	| 	string	| 1..n


## 9.	Operacje w GetTouristObjectEvents

### Operacja getEvents

Operacja pozwala na pobranie zdarzeń dotyczących obiektów w danym okresie. Obsługiwane zdarzenia:
-	TO_ARCHIVE – przeniesienie do archiwum
-	MERGE – scalenie obiektów

Element GetTouristObjectEventsRequest w żądaniu ( request ) ma następującą postać:

```xml
 <GetTouristObjectEventsRequest>
	<metric>
…
	</metric>
	<criteria>
		<dateFrom></dateFrom>
		<dateTo></dateTo>
	</criteria>
</GetTouristObjectEventsRequest>
```

Opis elementów:

Element |	Przeznaczenie |		Typ XML |	Krotność
---	| ---	| ---	| ---
metric		|Opis elementu w rozdziale: Wspólne elementy → Metryka niniejszego dokumentu.		|string		|1..1
dataFrom		|Data początkowa  zdarzeń.		|date		|1..1
dataTo		|Data końcowa zdarzeń.		|date		|1..1


Element GetTouristObjectEventsResponse w odpowiedzi ( response ) ma następującą postać:

```xml
<GetTouristObjectEventsResponse>
	<events>
		<eventType>TO_ARCHIVE</eventType>
		<eventDate>2016-08-02T09:00:26.000+02:00</eventDate>
		<objectId>74866</objectId>
	</events>
…
	<events>
		<eventType>MERGE</eventType>
		<eventDate>2016-09-09T09:11:45.000+02:00</eventDate>
		<objectId>75736</objectId>
		<newObjectId>85343</newObjectId>
	</events>
</GetTouristObjectEventsResponse>
```

Opis elementów:

Element |	Przeznaczenie |	Typ XML |	Krotność
---	| ---	| ---	| ---
eventType	|	Typ zdarzenia. Wartości: TO_ARCHIVE, MERGE.	|	string		|1..1
eventDate	|	Data i czas zdarzenia.	|	dateTime		|1..1
objectId	|	Id obiektu, którego dotyczy zdarzenie.	|	string	|	1..1
newObejctId		|Id obiektu po scaleniu obiektu. Występuje tylko dla zdarzenia MERGE.	|	string		|0..1


## 10.	Atrybuty, kategorie i języki narodowe w RIT

Niniejszy rozdział zawiera dodatkowe informacje o sposobie przetwarzania żądań w Systemie RIT.

W niniejszym rozdziale, słowo  'dokument' oznacza dokument 'Struktura danych RIT v.X.xlsx' zawierającego opis bazy merytorycznej systemu RIT (gdzie X oznacza numer wersji dokumentu).

W Systemie RIT, opis obiektu turystycznego jest złożony z:
1.	listy kategorii do której przypisany jest obiekt turystycznych
2.	listy atrybutów które posiada obiekt turystycznych
3.	wersji językowej w której przygotowany został opis obiektu
4.	innych pomocniczych atrybutów, utworzonych na potrzeby systemu RIT, do organizacji przetwarzania obiektu turystycznego.

### Ad 1) Kategorie

Każdy obiekt w systemie RIT, jest przypisany do co najmniej jednej kategorii. Każda kategoria składa się z listy atrybutów które stanowią opis tej kategorii. Lista atrybutów opisujących obiekt, jest uwarunkowana kategorią/-ami do której należy dany obiekt turystyczny.

Kategorie w systemie RIT, mają strukturę hierarchiczną. Na górze struktury znajduje się kategoria ROOT, z której niższe kategorie dziedziczą atrybuty. Hierarchia kategorii może ulegać zmianie,  gdyż jest dostępna do edycji dla użytkownika systemu RIT. Bieżąca struktura kategorii nie jest udostępniana przez webservice MetaDataOfRIT, gdyż do opisu obiektu turystycznego w RIT, hierarchia kategorii nie ma istotnego znaczenia.

Aktualny opis hierarchii jest zawarty w dokumencie, zakładka: Kategorie.

W czasie przekazywania danych do systemu RIT, każdy obiekt turystyczny musi mieć przydzieloną kategorię. Dobierać należy kategorie z poziomu 3 lub poziomu 4 ( patrz dokument ). Kategorie z poziomów 2,1,0 zostaną automatycznie przypisane do obiektu i nie ma potrzeby ich podawania w elemencie 'touristObjects/categories'.

W czasie przekazywania danych do systemu RIT, mogą się zdarzyć następujące przypadki:
-	kategoria obiektu turystycznego w systemie źródłowym nie istnieje lub nie ma odpowiednika w systemie RIT – w takiej sytuacji należy wybrać najbardziej zbliżoną kategorię spośród istniejących w systemie RIT.
-	kategoria obiektu turystycznego, w systemie źródłowym istnieje lecz ilość atrybutów opisujących obiekt w systemie źródłowym i docelowym jest nierówna – należy przekazać wszystkie atrybuty obiektu turystycznego zgromadzone w systemie źródłowym

W czasie pobierania danych do systemu RIT, dla każdego obiektu turystycznego zostaną zwrócone wszystkie kategorie do których przypisany jest obiekt ( niezależnie od poziomu z którego pochodzi kategoria).  

Przykład:

```xml
         <ritCategory>
            <code>C001</code>
            <name>Root</name>
            <description>Root</description>
            <attributeCodes>
               <attributeCode>A001</attributeCode>
               <attributeCode>A002</attributeCode>
            </attributeCodes>
         </ritCategory>
         <ritCategory>
            <code>C002</code>
            <name>Obiekty POI</name>
            <description>Obiekty POI</description>
            <attributeCodes>
               <attributeCode>A008</attributeCode>
               <attributeCode>A134</attributeCode>
            </attributeCodes>
         </ritCategory>
```

### Ad 2) Atrybuty

Każdy obiekt w systemie RIT,  posiada jeden lub więcej atrybutów. Lista atrybutów opisujących obiekt, wynika z kategorii do których przypisany jest dany obiekt.

W czasie przekazywania lub pobierania danych do/z systemu RIT, wartości wszystkich atrybutów powinny być przekazane/będą udostępniane jako ciąg znaków. Weryfikacja zgodności danej wartości z typem danych, opisujących danych atrybut, będzie wykonana na etapie weryfikacji poprawności obiektu ( patrz podrozdział: Etapy przetwarzania obiektów ).

Aktualny opis atrybutów jest zawarty w dokumencie, zakładka: Atrybuty.

Przypisanie atrybutów do kategorii, jest zawarte w dokumencie, zakładka: Atrybuty kategorii.

Ze względu na typ wartości, atrybuty w systemie RIT, dzielą się na:
-	typy prymitywne ( SHORT_TEXT, LONG_TEXT, NUMBER, BOOLEAN, DATE )
-	typ COMPLEX – jest to atrybut złożony, który sam nie przechowuje wartości atrybutu, a tylko grupuje inne atrybuty
-	typy MULTIPLY_LIST, SINGLE_LIST – jest to atrybut, którego wartościami mogą być tylko pozycje z słownika przypisanego do tego atrybutu.

Podczas przekazywania danych do systemu RIT, należy zapewnić:
-	dla typów prymitywnych - zgodność wartości z typem
-	dla typu COMPLEX – użycie odpowiedniego zestawu atrybutów zgrupowanych w ramach tego typu złożonego. Uwaga: używanie atrybutów typu COMPLEX, nie jest obowiązkowe.
-	dla typu MULTIPLY_LIST, SINGLE_LIST  – użycie pozycji istniejącej w słowniku przypisanym do danego atrybutu. W przypadku gdy istniejąca pozycja nie istnieje w słowniku systemu RIT, to również należy ją przekazać. W przypadku atrybutów: województwo, powiat, gmina, miejscowość należy przekazać nazwę np. Mazowieckie, Warszawa itp. Dopuszcza się żeby zamiast nazwy podawać ID województwa, ID powiatu, ID gminy, ID miejscowości (patrz opis elementu AttrVals).

W czasie przekazywania danych do systemu RIT, mogą się zdarzyć następujące przypadki:
-	atrybut obiektu turystycznego, w systemie źródłowym, nie istnieje w systemie RIT – w takiej sytuacji należy pominąć przekazywanie takiego atrybutu,
-	atrybut obiektu turystycznego, w systemie źródłowym istnieje lecz typ atrybutu jest niezgodny z tym w systemie RIT  – jeśli jest możliwość wykonania konwersji typów to należy ją wykonać. Gdy nie ma takiej możliwości atrybut należy pominąć,
-	kategoria w systemie RIT, do której przypisano obiekt nie zawiera w sobie, atrybutów posiadanych przez obiekt w systemie źródłowym – należy przekazać taki atrybut do systemu RIT.

W czasie pobierania danych z systemu RIT, udostępniane są wszystkie atrybuty przypisane do obiektu turystycznego:
-	dla typów prymitywnych - zgodne z typem
-	dla typu COMPLEX – aktualnie nie używane.
-	dla typu MULTIPLY_LIST, SINGLE_LIST  – wartość pozycji z słownika w systemie RIT.

Przykład definicji atrybutu:

```xml
<ritAttribute>
	<code>A017</code>
	<name>Kod pocztowy</name>
	<description>Kod pocztowy</description>
	<typeValidator>SHORT_TEXT</typeValidator>
	<typeUI>NULL</typeUI>
	<valRegexExpr>^[0-9]{2}\-[0-9]{3}$</valRegexExpr>
	<maxLegth>250</maxLegth>
 </ritAttribute>
 <ritAttribute>
	<code>A002</code>
	<name>Opis</name>
	<description>Opis obiektu</description>
	<typeValidator>COMPLEX</typeValidator>
	<typeUI>NULL</typeUI>
	<maxLegth>null</maxLegth>
	<complexAttribute>
	   <attributeCode>A003</attributeCode>
	   <attributeCode>A004</attributeCode>
	</complexAttribute>
 </ritAttribute>
```


### Ad 3) Wersja językowa

Każdy obiekt w systemie RIT,  posiada jedną lub więcej wersji językowych.  Wersja językowa to zestaw wartości atrybutów, które są zapisane w jednym z języków narodowych. Każda z wersji językowych jest przetwarzana oddzielnie w systemie RIT.  Wpływa to na sposób wymiany danych z systemem RIT.

Podczas przekazywania danych do systemu RIT, dla każdego z atrybutów należy podać, w jakiej wersji językowej jest wartość danego atrybutu:
-	locale dla języka narodowego ( 'pl-PL', 'en-GB' itp. ) - wartość atrybutu jest zapisana dla danego języka narodowego. Wartość tego atrybutu, zostanie umieszczona tylko w wskazanej wersji językowej. Należy używać locale dla wszystkich atrybutów które mogą podlegać tłumaczeniom
-	'all'- wartość atrybutu zostanie umieszczona we wszystkich wersjach językowych, dla danego obiektu turystycznego.

Podczas przekazywania danych do systemu RIT, wersje językowe są tworzone na bazie przekazywanych atrybutów. Z każdego z atrybutów jest pobierana informacja o wersji językowej i na tej podstawie tworzona jest lista wersji językowych.  Atrybuty nie mają w swojej konfiguracji wytycznych co do wersji językowej w jakiej mogą być używane. Jednak można zastosować pewne reguły:
-	atrybuty posiadające wartości będące nazwami własnymi  ( miasto, miejscowość itp. ) –  powinny być przekazywane dla każdej wersji językowej ( 'all' )
-	atrybuty posiadające wartości będące liczbami, datami lub nie zawierające słów – powinny być przekazywane dla każdej wersji językowej ( 'all' )
-	atrybuty posiadające wartości z słownika – powinny być przekazywane dla konkretnego języka  narodowego  ( 'pl-PL', 'en-GB' itp. ). Uwaga: jeśli słownik posiada nazwy własne, w języku polskim, które nie podlegają tłumaczeniu, to można użyć dla niego każdej wersji językowej ( 'all' ).
-	pozostałe atrybuty powinny być przekazywane dla konkretnego języka  narodowego  ( 'pl-PL', 'en-GB' itp. )

Podczas pobierania danych z systemu RIT, należy podać konkretną wersję językową, w której dane mają być dostarczone. Nie jest dopuszczalne użycie każdej wersji językowej ( 'all' ).


### Ad 4) Dane adresowe obiektu

Podawanie danych adresowych obiektu możliwe jest na dwa sposoby:

1.	Bezpośrednie podanie czystym tekstem nazwy województwa, powiatu, gminy i miejscowości;
2.	Tak jak dla wszystkich danych słownikowych: Podawanie numeru ID pozycji słownika województw, powiatów, gmin lub miejscowości (wymaga to znajomości numerów ID).

W pierwszym przypadku mogą wystąpić dwie sytuacje powodujące błędy lub uniemożliwiające poprawne przetwarzanie danych:

-	Nazwa województwa, powiatu, gminy lub miejscowości będzie błędna (lub jeszcze nieznana, nieobecna w bazie) – wówczas obiekt zostanie przyjęty do bazy RIT i użytkownik o odpowiedniej wiedzy merytorycznej poprawi błąd ręcznie lub odrzuci obiekt
-	Nazwa województwa, powiatu, gminy lub miejscowości będzie niejednoznaczna – system RIT postara się na podstawie wszystkich danych (województwa, powiatu, gminy, miejscowości) określić prawidłową, niejednoznaczną lokalizację obiektu, ewentualnie obiekt zostanie przyjęty lecz skierowany do poprawki przez żywą osobę

System jest niewrażliwy na wielkość liter w przypadku tych atrybutów.

Adresowanie czystym tekstem
```xml
<attribute code="A009">
	<attrVals language="all">
		<value>śląskie</value>
	</attrVals>
</attribute>
<attribute code="A010">
	<attrVals language="all">
		<value>tychy</value>
	</attrVals>
</attribute>
<attribute code="A011">
	<attrVals language="all">
		<value>Tychy</value>
	</attrVals>
</attribute>
<attribute code="A012">
	<attrVals language="all">
		<value>Tychy</value>
	</attrVals>
</attribute>
```

Adresowanie numerami ID
```xml
<attribute code="A009">
	<attrVals language="all">
		<value>1919</value>
	</attrVals>
</attribute>
<attribute code="A010">
	<attrVals language="all">
		<value>1587</value>
	</attrVals>
</attribute>
<attribute code="A011">
	<attrVals language="all">
		<value>4729</value>
	</attrVals>
</attribute>
<attribute code="A012">
	<attrVals language="all">
		<value>96392</value>
	</attrVals>
</attribute>
```

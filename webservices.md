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
   <attribute code=”">
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

# vegbilder

Lager metadata for vegbilder tatt med Viatech utstyr: Les EXIF-header, stedfest, filflytting m.m

# UFERDIG! Til uttesting

# Tre rutiner for håndtering av vegbilder og metadata om vegbilder

### 1. Lag metadata

*(kommer snart!)*

Leser metadata fra EXIF-header og (littegrann) metadata fra filnavn og mappenavn til bildet. 
Hvert bilde får en unik id (UUID), samt referanse til UUID for forrige og neste bilde. 
(Ja, dette betyr at det er trivielt å hente neste og forrige bilde på strekningen). 

Metadata skrives til en json-fil med samme filnavn som bildet. 

### 2. Stedfest metadata 

*(kommer snart!)*

Vi leser metadata fra steg 1. Ut fra vegreferanse-verdier og bildedato gjør vi oppslag 
på historisk vegrefeanse (per bildedato, med Visveginfo-tjenesten). Metadata utvides
med veglenkeID, veglenkeposisjon og koordinater for vegens senterlinje. Resultatet skrivest
tilbake til json-filen.

### 3. Oppdater vegreferanse-verdier

*(kommer snart!)*

Statens vegvesen har (eldre) programvare som er avhengig av at mappe- og filnavn for 
vegbilder følger oppbyggingen av vegreferansen 
(år - fylke - vegkategori, status og nummer - hp - meter). 
Denne koden bruker bildets stedfesting på vegnett 
(veglenkeID og veglenkeposisjon) til å hente de 
vegrefeanse-verdiene som er gyldig 
i dag, oppretter ny mappestruktur i hht ny vegreferanse og kopierer bildefil og 
oppdaterte metadata dit. Orginale metadata er selvsagt med videre. 

# Database og tjenester med metadata

Metadata (alle json-filene) legges i en romlig (spatial) database 
(postgis, oracle spatial), og vi bygger kule tjenester 
oppå der igjen! I første omgang konsentrerer vi oss om å bygge WFS 
(web feature services, dvs spørre- og søketjenester for metadata) og 
WMS (Web map services, dvs ferdige kartlag). Metadata fra disse 
tjenestene inkluderer selvsagt lenke (url) til en webserver som serverer
vegbildene direkte fra disk. 

Vi bruker FME for å synkronisere metadata-databasen med det som finnes på disk. 
FME støvsuger disk etter nye og gamle JSON-filer, og oppdaterer databasen
direkte. I tillegg skriver FME et tidsstempel i alle json-filene for når dette
metadata-elementet ble oppdatert. Nøkkelen ligger selvsagt i UUID som ble opprettet 
i steg 1, og som blir med videre gjennom steg 2 (stedfesting) og steg 3 (oppdatering
av vegreferanse, med tilhørende oppdatering av fil- og mappenavn)  


# Filstruktur

Filstruktur er bygget opp rundt ønsket om at python-kode skal kompileres til kjørbare filer på windows. 
Ergo føyer vi oss etter filstrukturen til pyinstaller. 


```
vegbilder/                <- Dette repositoryet
|__README.md              <- Den fila du leser nå
|
|__STEG1_mappe/ (kommer snart)
   |__python-kode for steg 1 (kommer snart)  
   |__dist/
      |__ *.exe 

|__Steg_2_mappe/ (kommer snart
    |__ div python-kode
    |__dist/ 
       |__ *.exe 
       
|__Steg_3_mappe (kommer snart) 
    |__ div python-kode
    |__dist/ 
       |__ *.exe 
      
|__test_pyinstaller/
   |__test.py             <-  Program som skal kompileres
   |__dist/
      |__test.exe         <- Demo, kjørbar fil for windows
```


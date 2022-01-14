---
layout: post
title: "Skrive data i Firebase Realtime Database (UTGÅTT)"
date: 2022-01-10 23:48:00 +0100
categories: database firebase
---

I [forrige leksjon](2022-01-09-oppsett-av-firebase-realtime-database.md) opprettet du en Firebase Realtime Database og la den til en nettside. I denne leksjonen skal du lære hvordan du legger inn data i databasen, og hvordan du henter data ut fra databasen.

## Forskjellen på relasjonsdatabaser og dokumentbaserte databaser
Tidligere har du lært å modellere relasjonsdatabaser, og å kode dem i SQL. En viktig forskjell på _relasjonsdatabaser_, eller _SQL-baserte databaser_, og _dokumentbaserte databaser_, eller _NoSQL-databaser_, er at i dokumentbaserte databaser er det ingen forhåndsbestemte regler for hvordan databasen skal bygges opp. Hele databasen tar utgangspunkt i en tekstfil (ofte en .JSON-fil), der man legger all dataen inn, uten å ha bestemt felter, rader og kolonner på forhånd. Man skriver rett og slett bare all dataen inn i tekstfilen. Dette gjør dokumentbaserte databaser veldig fleksible og enkle å sette opp, men det krever også mer orden fra de som skal bruke databasen, og for at en slik database skal være brukervennlig krever det at programmet eller nettsiden man bruker til å aksessere databasen er satt opp på en måte som gjør det enklere å legge inn riktig data.

Vi kan likevel trekke ut noen likheter mellom relasjonsdatabaser (SQL) og dokumentbaserte databaser (NoSQL). En _tabell_ i SQL tilsvarer en _samling (collection)_ i NoSQL, og en _rad_ (all dataen tilknyttet et objekt i databasen) i SQL tilsvarer et _dokument (document)_ i NoSQL. Du kan lese mer om oppbyggingen av dokumentbaserte databaser [hos MongoDB (mongodb.com)](https://www.mongodb.com/document-databases).

### Trenger vi datamodellering med dokumentbaserte databaser?

<img src="https://api.ndla.no/image-api/raw/x1jYWIh5.svg" alt="ER-modell" width="350px">

Det kan kanskje være enkelt å tro at vi ikke trenger datamodellering i NoSQL-databaser, siden vi ikke trenger å opprette tabeller og felter på samme måte som i SQL, men det kan uansett være en god ide å ha oversikt over hvordan dataene skal organiseres i databasen, og derfor kan det være lurt å sette opp en datamodell også når du skal bruke NoSQL-databaser.

På bildet ser du oppbyggingen av en Firebase-database med elevdata:<br>
![Skjermbilde av Firebase-database med data om elever](/img/fb-elevliste-eksempel.png)

Som du kan se på bildet har noen av elevene ulik info. Noen har telefon, andre har epost, og noen har begge deler. Dette ville ikke vært mulig i en SQL-database, der alle feltene må være opprettet på forhånd. Dette viser noe av fleksibiliteten i en NoSQL-database.

## Legge inn data i databasen
Du skal nå legge inn dataen fra bildet over i Firebase-databasen din. Åpne html-fila _index.html_ som du opprettet i forrige leksjon. Scriptet i _index.html_ skal se omtrent slik ut:

```javascript
// Importerer funksjonen 'initializeApp' fra SDK-pakken 'firebase-app.js'. Pass på at du bruker fullstendig URL med 'https://'
import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.2/firebase-app.js";
// Legger til funksjonene vi skal bruke fra SDK-pakken 'firebase-database.js'. Pass på at du bruker fullstendig URL med 'https://'
// Du kan legge inn flere funksjoner i { } etterhvert som du bruker flere databasefunksjoner.
// Om du trenger flere SDK-pakker kan du finne dem på https://firebase.google.com/docs/web/learn-more#libraries-cdn
import { getDatabase, ref, set, onValue } from "https://www.gstatic.com/firebasejs/9.6.2/firebase-database.js";

// Konfigurerer Firebase-appen din
//Ord på formatet _ALL_CAPS tilsvarer unike verdier for databasen din
const firebaseConfig = {
  apiKey: "_API_KEY",
  authDomain: "_PROJECT_ID.firebaseapp.com",
  projectId: "_PROJECT_ID",
  storageBucket: "_PROJECT_ID.appspot.com",
  messagingSenderId: "_SENDER_ID",
  appId: "_APP_ID",
  databaseURL: "https://_PROJECT_ID-default-rtdb._LOCATION.firebasedatabase.app/"
};
// Starter Firebase-appen med de angitte innstillingene
const app = initializeApp(firebaseConfig);
// Henter ut databasen fra Firebase-appen og lagrer den som et objekt vi kan bruke i JavaScript
const db = getDatabase(app);
```

### Funksjonene `set` og `ref`
Du skal nå skrive kode for å opprette nye elementer i databasen. Til dette bruker vi Firebase-funksjonene `set` og `ref`. 

[`set`](https://firebase.google.com/docs/reference/js/database.md#set) er en funksjon for å skrive data til databasen. Denne funksjonen tar inn to parametere; hvor dataen skal skrives til (databasereferanse) og hvilken data som skal skrives, slik:
```javascript
set(_DATABASEREFERANSE, _DATA)
```
Dataen skrives inn på formen
```javascript
{
_FELTNAVN_1: _DATA_1,
_FELTNAVN_2: _DATA_2
}
```

[`ref`](https://firebase.google.com/docs/reference/js/database.md#ref) brukes inne i `set`, for å angi databasereferansen, altså hvilken database som skal brukes, og hvilken sti inne i databasen dataen skal skrives til. Stien angir hvilken samling og dokument som skal skrives til. En samling i dokumentbaserte-databaser tilsvarer som sagt omtrent en tabell i relasjonsdatabaser, og et dokument tilsvarer en rad. Koden for å lage en referanse til en elev blir derfor slik:
```javascript
ref(_DATABASE, _STI_TIL_DOKUMENT)`
```
Stien til dokumentet skrives inn på formen
```javascript
"_SAMLING/_DOKUMENT_ID"
```

## Opprette et dokument i databasen
Nå er du klar til å legge inn den første eleven i databasen:
```javascript
//Legger inn et nytt dokument i samlingen "elever" med dokumentid=1.
set(ref(db, "elever/0"), {
  etternavn: "Thomasen",
  fornavn: "Rebecca",
  epost: "",
  telefon: "12345678"
});
```
Hvis id-en (1) finnes fra før vil dokumentet bli overskrevet. Hvis det ikke finnes fra før opprettes et nytt dokument med denne id-en.


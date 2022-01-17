---
layout: post
title: "Skrive og lese data i Cloud Firestore"
date: 2022-01-16 22:49:00 +0100
categories: database firebase
---
(Dette dokumentet er ikke helt ferdig, men er godt underveis)


I [forrige leksjon](2022-01-14-oppsett-av-cloud-firestore.md) opprettet du en Firestore-database og la den til en nettside. I denne leksjonen skal du lære hvordan du legger inn data i databasen, og hvordan du henter data ut fra databasen.

## Forskjellen på relasjonsdatabaser og dokumentbaserte databaser
Tidligere har du lært å modellere relasjonsdatabaser, og å kode dem i SQL. En viktig forskjell på _relasjonsdatabaser_, eller _SQL-baserte databaser_, og _dokumentbaserte databaser_, eller _NoSQL-databaser_, er at i dokumentbaserte databaser er det ingen forhåndsbestemte regler for hvordan databasen skal bygges opp. Hele databasen tar utgangspunkt i en tekstfil (ofte en .JSON-fil), der man legger all dataen inn, uten å ha bestemt felter, rader og kolonner på forhånd. Man skriver rett og slett bare all dataen inn i tekstfilen. Dette gjør dokumentbaserte databaser veldig fleksible og enkle å sette opp, men det krever også mer orden fra de som skal bruke databasen, og for at en slik database skal være brukervennlig krever det at programmet eller nettsiden man bruker til å aksessere databasen er satt opp på en måte som gjør det enklere å legge inn riktig data.

Vi kan likevel trekke ut noen likheter mellom relasjonsdatabaser (SQL) og dokumentbaserte databaser (NoSQL). En _tabell_ i SQL tilsvarer en _samling (collection)_ i NoSQL, og en _rad_ (all dataen tilknyttet et objekt i databasen) i SQL tilsvarer et _dokument (document)_ i NoSQL. Du kan lese mer om oppbyggingen av dokumentbaserte databaser [hos MongoDB (mongodb.com)](https://www.mongodb.com/document-databases).

### Trenger vi datamodellering med dokumentbaserte databaser?

<img src="https://api.ndla.no/image-api/raw/x1jYWIh5.svg" alt="ER-modell" width="350px">

Det kan kanskje være enkelt å tro at vi ikke trenger datamodellering i NoSQL-databaser, siden vi ikke trenger å opprette tabeller og felter på samme måte som i SQL, men det kan uansett være en god ide å ha oversikt over hvordan dataene skal organiseres i databasen, og derfor kan det være lurt å sette opp en datamodell også når du skal bruke NoSQL-databaser.

På bildene under ser du oppbyggingen av en Firestore-database med elevdata:<br>
![Skjermbilde av Firestore-database med data om elever](/img/fs-elevliste-eksempel-1.png)

![Skjermbilde av Firestore-database med data om elever](/img/fs-elevliste-eksempel-2.png)

![Skjermbilde av Firestore-database med data om elever](/img/fs-elevliste-eksempel-3.png)

Som du kan se på bildene har noen av elevene ulik info. Noen har telefon, andre har epost, og noen har begge deler. Dette ville ikke vært mulig i en SQL-database, der alle feltene må være opprettet på forhånd. Dette viser noe av fleksibiliteten i en NoSQL-database.

## Legge inn data i databasen
Du skal nå legge inn dataen fra bildet over i Firebase-databasen din. Åpne html-fila _index.html_ som du opprettet i forrige leksjon. Scriptet i _index.html_ skal se omtrent slik ut:

```html
<script type="module">
  // Importerer intitalizApp fra Firebase-app SDK
  import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.3/firebase-app.js";
  // Importerer alle Firestore-funksjonene vi skal bruke. 
  // Om du får feilmeldingen "ReferenceError: [...] is not defined", kan det være det fordi du har brukt en Firestore-funksjon uten å ha importert den her.
  import { getFirestore, collection, doc, addDoc, setDoc, getDoc, getDocs, query, where, orderBy, startAt, startAfter, endAt, limit, limitToLast } from "https://www.gstatic.com/firebasejs/9.6.3/firebase-firestore.js"

  // Konfigurerer Firebase
  const firebaseConfig = {
    apiKey: "_API_KEY_",
    authDomain: "_PROJECT_ID_.firebaseapp.com",
    projectId: "_PROJECT_ID_",
    storageBucket: "_PROJECT_ID_.appspot.com",
    messagingSenderId: "_SENDER_ID_",
    appId: "_APP_ID_",
  };

  // Starter opp firebase med den angite konfigurasjonen
  const app = initializeApp(firebaseConfig);
  //Henter ut referanse til Firestore-databasen
  const db = getFirestore();
</script>
```

### Opprette et nytt dokument i databasen med addDoc()
Du skal nå skrive kode for å opprette et nytt dokument i databasen. Den enkleste måten å gjøre dette på er å bruke Firestore-funksjonene `addDoc()` og `collection()`. Dokumenter som opprettes med `addDoc()` får automatisk en unik ID, så du trenger ikke tenke på at dokumentet må ha en primærnøkkel.

En viktig ting med funksjonene knyttet til Firestore er at de er asynkrone. Det vil si at når en databaseoperasjon skal kjøre, vil programmet fortsette å kjøre neste kodelinje uten å vente på at kommunikasjonen med databasen er fullført. Derfor skriver vi ordet `await` foran disse funksjonene, slik at programmet venter til kommunikasjonen med databasen er fullført, før neste kodelinje kjøres.

[`addDoc()`](https://firebase.google.com/docs/reference/js/firestore_.md#adddoc) er en funksjon for å skrive data til databasen. Denne funksjonen tar inn to parametere; hvilken _samling_ (collection) dataen skal skrives til og hvilken data som skal skrives, slik:
```javascript
addDoc(_COLLECTION, _DATA);
```
Dataen skrives inn på formen
```javascript
{
_FELTNAVN_1: _DATA_1,
_FELTNAVN_2: _DATA_2
}
```

[`collection()`](https://firebase.google.com/docs/reference/js/firestore_.md#collection_2) brukes inne i `addDoc`, for å angi hvilken database som skal brukes, og hvilken samling (collection) inne i databasen dataen skal skrives til. En samling i dokumentbaserte-databaser tilsvarer som sagt omtrent en tabell i relasjonsdatabaser, og et dokument tilsvarer en rad. Koden for å lage en referanse til en elev blir derfor slik:
```javascript
collection(_DATABASE, _NAVN_PÅ_COLLECTION)`
```

#### Legge inn en elev i databasen
Nå er du klar til å legge inn den første eleven i databasen. Skriv følgende kode nederst i scriptet ditt (husk å bruke koden `await` foran funksjonen som starter kommunikasjon med databasen):
```javascript
//Legger inn et nytt dokument i samlingen "elever".
await addDoc(collection(db, "elever"), {
    fornavn: "Rebecca",
    etternavn: "Thomasen",
    telefon: "12345678"
});
```
Lagre html-dokumentet og åpne det i en nettleser. Åpne _inspiser_-verktøyet for å sjekke at du ikke har noen feilmeldinger i konsollen, og se på Firestore-databasen din. Nå skal du finne et nytt dokument med dataen du la inn.
![Skjermbilde av Firestore-databasen](/img/fs-elevliste-eksempel-4.png)


### Opprette et nytt dokument i databasen med setDoc()
`setDoc()` er en annen måte å opprette nye dokumenter i databasen. `setDoc()` gir deg muligheten til å definere en egen primærnøkkel for hvert dokument i databasen, slik at du kan bruke for eksempel brukernavn, epost, eller en egendefinert ID (elevID, kundenummer) som primærnøkkel. `setDoc()` brukes i kombinasjon med `doc()` for å angi hvor dokumentet skal lagres i databasen, og hva som skal være primærnøkkelen.

[`setDoc()`](https://firebase.google.com/docs/reference/js/firestore_.md#setdoc) er en funksjon for å skrive data til en spesifikk ID i databasen. Denne funksjonen tar inn to parametere; hvilket _dokument_ dataen skal skrives til (angitt med ID) og hvilken data som skal skrives, slik:
```javascript
setDoc(_DOCUMENT, _DATA);
```
Dataen skrives inn på formen
```javascript
{
_FELTNAVN_1: _DATA_1,
_FELTNAVN_2: _DATA_2
}
```

[`doc()`](https://firebase.google.com/docs/reference/js/firestore_.md#doc) brukes inne i `setDoc()`, for å angi hvilken database som skal brukes, hvilken samling (collection) inne i databasen dataen skal skrives til, og hvilken ID dokumentet skal ha.  Koden for å lage en referanse til en elev blir derfor slik:
```javascript
doc(_DATABASE, _NAVN_PÅ_COLLECTION, _ID)`
```

#### Legge inn en elev i databasen
Nå er du klar til å legge inn en ny elev med egendefinert ID i databasen. Som ID velger vi å lage et brukernavn (3 første bokstaver i etternavn + 2 første bokstaver i fornavn). Skriv følgende kode nederst i scriptet ditt (husk å bruke koden `await` foran funksjonen som starter kommunikasjon med databasen):
```javascript
//Legger inn et nytt dokument i samlingen "elever".
await setDoc(doc(db, "elever", "nilja"), {
  fornavn: "Jakob",
  etternavn: "Nilsen",
  epost: "jakob@nilsen.net"
}):
```
Lagre html-dokumentet og åpne det i en nettleser. Åpne _inspiser_-verktøyet for å sjekke at du ikke har noen feilmeldinger i konsollen, og se på Firestore-databasen din. Nå skal du finne et nytt dokument med dataen du la inn. Legg merke til at ID-en til dokumentet er det samme som vi anga i koden ("nilja").
![Skjermbilde av Firestore-databasen](/img/fs-elevliste-eksempel-5.png)

## Lese data fra databasen
I likhet med at det fins flere måter å skrive data til databasen, fins det også flere måter å lese fra databasen. De enkleste er `getDoc()` og `getDocs()`. 

### Hente ut ett dokument fra databasen med getDoc()
`getDoc()` brukes til å hente ut enkeltdokumenter fra databasen, og krever at du vet ID-en til dokumentet du skal hente. I likhet med `setDoc()` bruker `getDoc()` funksjonen `doc()` for å angi hvilket dokument som skal hentes ut. `getDoc()` returnerer et databaseobjekt, og derfor må du opprette en variabel for å lagre dataen som hentes ut.

Koden for å hente ut det siste dokumentet vi lagde blir:
```javascript
const docSnap = await getDoc(doc(db, "elever", "nilja"));
console.log("Document data:", docSnap.data());
```
Når vi henter ut data fra databasen kaller vi det vi henter ut et "snapshot", fordi det representerer et øyeblikksbilde av dataen i det øyeblikket vi henter den ut fra databasen. `const docSnap` i koden over repesenterer et øyeblikksbilde av ett dokument i databasen (docSnap er her kun et variabelnavn, og kan i utgangspunktet være hva som helst, men dette er et eksempel på et beskrivende variabelnavn). `docSnap.data()` henter ut all dataen som er lagret i dokumentet og presenterer det som et objekt. Om du vil hente ut enkeltdata, kan du angi det slik:
```javascript
console.log("ID:", docSnap.id); //ID: nilja
console.log("Navn:", docSnap.data().fornavn, docSnap.data().etternavn); //Navn: Jakob Nilsen
```
Prøv å lime inn denne linja i koden din og se hva som blir skrevet ut i konsollen.

### Hente ut alle dokumentene i en samling med getDocs()
`getDocs()` brukes for å hente ut flere dokumenter på en gang fra databasen. Det kan være enten alle dokumentene i en samling, eller et utvalg basert på gitte kriterier (query). `getDocs()` returnerer en liste (array) med databaseobjekter.

For å hente ut alle dokumentene i en samling bruker vi `collection()` inne i `getDocs()` for å angi hvilken samling vi skal hente dokumentene fra. For å hente ut alle dokumentene i elevlisten blir koden slik:
```javascript
const snapshot = await getDocs(collection(db, "elever"));
snapshot.forEach((doc) => {
  console.log(doc.data().fornavn, doc.data().etternavn);
});
```
`snapshot.forEach()` er en løkke som kjører en gang for hvert dokument (`doc`) i samlingen. I eksempelet skriver denne løkken ut fornavn og etternavn til alle elevene til konsollen.

### Hente ut utvalgte dokumenter i en samling med query()
Med `query()` kan vi lage spørringer not databasen. En spørring er en betingelse for å filtrere, begrense eller sortere dataen som skrives ut. For eksempel kan vi hente ut bare elever som er registrert med e-post, eller vi kunne hentet ut bare elever som har matematikk som et av sine fag. For å lage en spørring kan vi buke funksjonene `where()`, `orderBy`, `startAt()`, `startAfter()`, `endAt()`, `limit()` eller `limitToLast()`.

Slik lager du en spørring for å bare hente ut elever som er registrert med e-post:
```javascript
query(collection(db, "elever"), where("epost", "!=", "undefined"));
```
Betingelsen `where("epost", "!=", "undefined")` betyr at epost skal være ikke lik (!=) "undefined". Det vil si at spørringen utelukker elever som ikke har registrert epost (epost == defined).

Vi skal nå bruke denne spørringen opp mot databasen og skrive ut resultatet:
```javascript
const querySnapshot = await getDocs(query(collection(db, "elever"), where("epost", "!=", "undefined")));
querySnapshot.forEach((doc) => {
  console.log(doc.data().fornavn, doc.data().etternavn, doc.data().epost);
});
```
`querySnapshot.forEach()` skriver ut alle resultatene av spørringen.

---
layout: post
title: "Lese data fra Cloud Firestore"
date: 2022-01-20 14:30:00 +0100
categories: database firebase
---

I [forrige leksjon](/database/firebase/2022/01/16/skrive-data-til-cloud-firestore.html) lærte du hvordan du legger inn data i databasen. I denne leksjonen skal du lære hvordan du henter data ut fra databasen og lager spørringer for å velge hvilke data du vil hente ut.

## Lese data fra databasen
I likhet med at det fins flere måter å skrive data til databasen, fins det også flere måter å lese fra databasen. De enkleste er `getDoc()` og `getDocs()`. 

### Hente ut ett dokument fra databasen med getDoc()
`getDoc()` brukes til å hente ut enkeltdokumenter fra databasen, og krever at du vet ID-en til dokumentet du skal hente. I likhet med `setDoc()` bruker `getDoc()` funksjonen `doc()` for å angi hvilket dokument som skal hentes ut. `getDoc()` returnerer et databaseobjekt, og derfor må du opprette en variabel for å lagre dataen som hentes ut.

Koden for å hente ut dokumentet vi lagde for eleven Jakob Nilsen blir:
```javascript
const docSnap = await getDoc(doc(db, "elever", "nilja"));
console.log("Document data:", docSnap.data());
```
![Skjermbilde av konsoll med utskriften "Document data" og data tilhørende eleven Jakob Nilsen](/img/2022-01-20-lese-data-fra-cloud-firestore/fs-lesdata-console-nilja.png)

Når vi henter ut data fra databasen kaller vi det vi henter ut et "snapshot", fordi det representerer et øyeblikksbilde av dataen i det øyeblikket vi henter den ut fra databasen. `const docSnap` i koden over repesenterer et øyeblikksbilde av ett dokument i databasen (docSnap er her kun et variabelnavn, og kan i utgangspunktet være hva som helst, men dette er et eksempel på et beskrivende variabelnavn). `docSnap.data()` henter ut all dataen som er lagret i dokumentet og presenterer det som et objekt. Om du vil hente ut enkeltdata, kan du angi det slik:
```javascript
console.log("ID:", docSnap.id);
console.log("Navn:", docSnap.data().fornavn, docSnap.data().etternavn); 
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
![Skjermbilde av konsoll med utskrift av fornavn og etternavn til alle elever i databasen](/img/2022-01-20-lese-data-fra-cloud-firestore/fs-lesdata-console-alle-elever.png)

#### Pil-funksjoner (arrow function expressions)
I eksempelet over med `forEach`-løkken er det brukt en såkalt pil-funksjon. En pil-funksjon fungerer akkurat som en vanlig funksjon, men det er en måte å forkorte hvordan vi skriver en funksjon, slik at koden blir mer kompakt. Pil-funksjonen i eksempelet ser slik ut:
```javascript
(doc) => {
  console.log(doc.data().fornavn, doc.data().etternavn);
}
```
Hvis vi skal skrive denne funksjonen på den vanlige måten, blir det slik:
```javascript
function f(doc){
  console.log(doc.data().fornavn, doc.data().etternavn);
}
```
Når vi bruker pil-funksjoner, fjerner vi altså ordet `function` og funksjonnavnet (`f`), og lar parentesen med eventuelle argumenter som skal inn i funksjonen stå alene, med en pil `=>` mellom argumentet og funksjonens _body_ (det som står mellom krøllparentesene `{}`). Her er et eksempel som viser samme funksjon på standard form og som pil-funksjon.
```javascript
//Standard funksjon uten argumenter
function enFunksjon() {
  //kode som skal kjøres
}

//Pil-funksjon uten argumenter
() => {
  //kode som skal kjøres
}
```

### Hente ut utvalgte dokumenter i en samling med query()
Med `query()` kan vi lage spørringer mot databasen. En spørring er en betingelse for å filtrere, begrense eller sortere dataen som skrives ut. Dette fungerer stort sett på samme måte som i SQL. For eksempel kan vi hente ut bare elever som er registrert med e-post, eller vi kunne hentet ut bare elever som har matematikk som et av sine fag. For å lage en spørring kan vi buke funksjonene `where()`, `orderBy()`, `startAt()`, `startAfter()`, `endAt()`, `limit()` eller `limitToLast()`.

For eksempel kan vi lage en spøring for å hente ut alle elever, sortert på etternavn:
```javascript
query(collection(db, "elever"), orderBy("etternavn"));
```

Eller en spørring for å bare hente ut elever som er registrert med e-post:
```javascript
query(collection(db, "elever"), where("epost", "!=", "undefined"));
```
Betingelsen `where("epost", "!=", "undefined")` betyr at epost skal være ikke lik (!=) "undefined". Det vil si at spørringen utelukker elever som ikke har registrert epost (epost == undefined).

Vi skal nå bruke denne spørringen opp mot databasen og skrive ut resultatet:
```javascript
const querySnapshot = await getDocs(query(collection(db, "elever"), where("epost", "!=", "undefined")));
querySnapshot.forEach((doc) => {
  console.log(doc.data().fornavn, doc.data().etternavn, doc.data().epost); //[Fornavn] [Etternavn] [Epost]
});
```
`querySnapshot.forEach()` skriver ut alle resultatene av spørringen.
![Skjermbilde av konsoll med utskrift av fornavn, etternavn og epost til alle elever som er registrert med epost i databasen](/img/2022-01-20-lese-data-fra-cloud-firestore/fs-lesdata-console-elever-med-epost.png)

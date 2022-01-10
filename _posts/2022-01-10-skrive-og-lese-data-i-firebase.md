---
layout: post
title: "Skrive og lese data i Firebase Realtime Database"
date: 2022-01-10 23:48:00 +0100
categories: database firebase
---
(Under arbeid: Denne posten er ikke skrevet ferdig enda)

I [forrige leksjon](2022-01-09-oppsett-av-firebase-realtime-database.md) opprettet du en Firebase Realtime Database og la den til en nettside. I denne leksjonen skal du lære hvordan du legger inn data i databasen, og hvordan du henter data ut fra databasen.

## Forskjellen på relasjonsdatabaser og dokumentbaserte databaser
Tidligere har du lært å modellere relasjonsdatabaser, og å kode dem i SQL. En viktig forskjell på _relasjonsdatabaser_, eller _SQL-baserte_ databaser, og _dokumentbaserte databaser_, eller _NoSQL-databaser_, er at i NoSQL-databaser er det ingen forhåndsbestemte regler for hvordan databasen skal bygges opp. Hele databasen tar utgangspunkt i et dokument, der man legger all dataen inn, uten å ha bestemt felter, rader og kolonner på forhånd. Man skriver rett og slett bare all dataen inn i dokumentet. Dette gjør NoSQL-databaser veldig fleksible og enkle å sette opp, men det krever også mer orden fra de som skal bruke databasen, og for at en slik database skal være bukervennlig krever det at programmet eller nettsiden man bruker til å aksessere databasen er satt opp på en måte som gjør det enklere å legge inn riktig data.

### Trenger vi datamodellering med dokumentbaserte databaser?

<img src="https://api.ndla.no/image-api/raw/x1jYWIh5.svg" alt="ER-modell" width="350px">

Det kan kanskje være enkelt å tro at vi ikke trenger datamodellering i NoSQL-databaser, siden vi ikke trenger å opprette tabeller og felter på samme måte som i SQL, men det kan uansett være en god ide å ha oversikt over hvordan dataene skal organiseres i databasen, og derfor kan det være lurt å sette opp en datamodell også når du skal bruke NoSQL-databaser.

På bildet ser du oppbyggingen av en Firebase-database med elevdata:<br>
![Skjermbilde av Firebase-database med data om elever](/img/fb-elevliste-eksempel.png)

Som du kan se på bildet har noen av elevene ulik info. Noen har telefon, andre har epost, og noen har begge deler. Dette ville ikke vært mulig i en SQL-database, der alle feltene må være opprettet på forhånd. Dette viser noe av fleksibiliteten i en NoSQL-database.

## Legge inn data i databasen
Du skal nå legge inn dataen fra bildet over i Firebase-databasen din. Åpne html-fila _index.html_ som du opprettet i forrige leksjon. `<script>`

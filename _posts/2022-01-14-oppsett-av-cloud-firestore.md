---
layout: post
title: "Oppsett av Firebase og Cloud Firestore"
date: 2022-01-09 23:48:00 +0100
categories: database firebase
---
I denne guiden skal du lære å sette opp _Firebase_ og _Cloud Firestore_. Firebase er et produkt fra Google som gjør det enkelt å koble en database til en nettside eller app, slik at man kan lagre info og innstillinger over lengre tid. Firebase er en enkel måte å sette opp databaser på, da det ikke krever noe serverside-script eller egen databaseserver.

<h1 id="kom-i-gang-med-firebase">Kom i gang med Firebase</h1>
<p><img src="/img/fb-get-started.png" alt="Skjermbilde av forsiden på firebase.google.com"></p>

<p>For å bruke Firebase, må du ha en Google-konto. Gå til <strong><a href="https://firebase.google.com/">firebase.google.com</a></strong> og logg på med ditt brukernavn og passord ved å trykke <strong>Logg på</strong> øverst i høyre hjørne, hvis du ikke allerede er innlogget. Når du har logget inn, trykker du på <strong>Get started</strong> eller <strong>Go to console</strong> for å komme i gang med Firebase.</p>

<h1 id="opprett-et-nytt-prosjekt">Opprett et nytt prosjekt</h1>
<p><img src="/img/fb-create-a-project.png" alt="Skjermbilde av Firebase-konsollen"></p>

<p>Du skal nå opprette et nytt prosjekt. Trykk på <strong>Create a project</strong>, og følg instruksjonene. Det er tre steg:</p>
<ol>
  <li>Gi prosjektet ditt et navn. I dette første prosjektet skal vi lage en app for å registrere elever, så du kan gjerne kalle prosjektet ditt <strong>Elevliste</strong>.
    <ul>
      <li>I dette steget må du huske å huke av på <strong>I accept the Firebase Terms</strong></li>
    </ul>
  </li>
  <li>Angi om prosjektet ditt skal bruke Google Analytics. Det er ikke nødvendig i dette prosjektet, så du kan slå dette av ved å trykke på bryteren ved <strong>Enable Google Analytics for this project</strong>, slik at den blir grå. Trykk deretter på <strong>Create project</strong>
    <ul>
      <li><em>Google Analytics</em> brukes til å analysere bruk og besøk til nettsiden eller appen din.</li>
    </ul>
  </li>
  <li><em>Your new project is ready</em>. Du har nå opprettet prosjektet ditt. Trykk på <strong>Continue</strong> for å gå til prosjektsiden.</li>
</ol>

<h1 id="legg-til-en-web-app">Legg til en web app</h1>
<p><img src="/img/fb-project-overview-1.png" alt="Skjermbilde av prosjektoversikten i Firebase"></p>

<p>Du skal nå legge til en app i prosjektet ditt. Siden vi skal lage en webside, eller web app, trykker du på symbolet <strong>&lt;/&gt;</strong> over teksten <strong>Add an app to get started</strong>. Først må du gi appen din et navn. Gi appen din navnet <strong>Elevliste-web</strong>. Dette navnet brukes bare internt i Firebase-konsollen, for å skille forskjellige apper fra hverandre, så det er ikke viktig at navnet er unikt og spennende. Trykk på <strong>Register app</strong>.</p>

<p>Du får nå opp koden som du skal bruke for å koble Firebase til nettsiden din. Denne koden kalles <strong>SDK</strong>. Trykk på <strong>Use a <code class="language-plaintext highlighter-rouge">&lt;script&gt;</code> tag</strong> for å få opp riktig kode (vi skal ikke bruke npm i denne guiden). Kopier koden ved å trykke på de to overlappende firkantene nederst til høyre i kodefeltet. NB! Ikke trykk deg videre enda!<br>
<img src="/img/fb-sdk.png" alt="Skjermbilde av SDK-koden til prosjektet"></p>

<p>Nå skal du sette denne koden inn i html-filen der du skal bruke Firebase. Åpne <strong>Visual Studio Code</strong> og opprett en ny mappe (<em>Åpne mappe</em> -&gt; <em>Opprett ny mappe</em>). I denne mappen oppretter du en ny fil som du kaller <strong>index.html</strong>. Tast inn <strong>!</strong> (utropstegn) og trykk enter for å få de nødvendige taggene. Inne i <code class="language-plaintext highlighter-rouge">&lt;body&gt;</code>-taggen limer du inn koden du fikk fra Firebase. Husk at denne koden er unik for deg, så deler av koden din vil være ulik skjermbildet.</p>

<p><a href="/img/fb-vscode-sdk.png"><img src="/img/fb-vscode-sdk.png" alt="Skjermbilde av HTML-filen med SDK-koden limt inn"></a><br>(Klikk på skjermbildet for å gjøre teksten større)</p>

<p>Når du har kopiert inn hele koden, inkludert <code class="language-plaintext highlighter-rouge">&lt;script&gt;</code>-taggene, lagrer du html-filen din og går tilbake til Firebase. Nå kan du trykke på <strong>Continue to console</strong>.</p>

<p>Hvis du trenger å finne igjen denne koden (SDK) senere, finner du den igjen i <strong>Project settings</strong>.</p>

<h1 id="legg-til-en-database">Legg til en Firestore-database</h1>
<p><img src="/img/fb-choose-a-product.png" alt="Skjermbilde av Firebase-konsollen"></p>

<p>Du skal nå legge til Cloud Firestore i prosjektet ditt. Trykk på <strong>Cloud Firestore</strong>.<br>


<p>Trykk på ikonet, og trykk deretter på <strong>Create database</strong>. Du må nå gjøre to valg:</p>
<ol>
  <li>Hvem skal ha lese- og skriverettigheter til databasen din? For enkelthets skyld velger vi <strong>Start in test mode</strong>, som gir både lese- og skrivetilgang til alle som har tilgang på koden til databasen din. Dette kan du endre senere, og du må endre det innen 30 dager.</li>
  <li>Hvor skal databasen din lagres? Her velger du <strong>eur3 (europe-west)</strong>. Da vil databasen din bli lagret i Vest-Europa, som er en fordel, både fordi det er fysisk nært Norge, og fordi landene i Vest-Europa følger EUs personverndirektiv GDPR, som er det samme direktivet som vi følger i Norge.</li>
</ol>

<p>Nå har du opprettet databasen din.<br>
<img src="/img/fs-database-view-empty.png" alt="Skjermbilde av databasen"><br>
Du kan legge inn data i databasen direkte om du ønsker det. Firestore er en dokumentbasert database, eller NoSQL-database. Denne typen database er mye mindre streng på hva slags data som kan lagres i databasen. Man lager ikke tabeller, felter og datatyper på forhånd som i SQL, men man legger data inn direkte som objekter (dokumenter). Metadata legges inn på hvert dokument individualt, slik at hvert dokument kan ha forskjellig data <em>og</em> metadata. Man er derfor mer avhengig av hvordan appen som kommuniserer med databasen er bygd opp for å få en oversiktlig database.</p>

<h1 id="legg-inn-databasen-i-websiden-din">Legg inn databasen i websiden din</h1>
<p>Nå som du har opprettet databasen, må du legge inn databasen i websiden du lagde tidligere. Åpne Visual Studio Code igjen. I koden du kopierte inn tidligere, skal du nå legge til noen linjer.

<p>Du skal legge til tre kodelinjer:</p>
<ol>
  <li>Under <code class="language-plaintext highlighter-rouge">// TODO: Add SDKs for Firebase products that you want to use</code> (linje 14 i skjermbildet over), legger du inn denne koden (kopier linja under og lim den inn i html-dokumentet ditt):
 
```javascript
import { getFirestore, addDoc, collection } from "https://www.gstatic.com/firebasejs/9.6.3/firebase-firestore.js"
```
  </li>
  
  NB! Pass på at versjonsnummeret i URL-en (9.6.3) er det samme som versjonsnummeret fra koden du kopierte når du satte opp web-appen din.
  
  
  <li>Nederst i scriptet, legger du inn denne koden (kopier linja under og lim den inn i html-dokumentet ditt):
    
    
```javascript
const db = getFirestore();
```
    
  </li>
</ol>

# Kontroller koden din
Sjekk at koden din er lik som koden under, men med dine unike verdier i `firebaseconfig`.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script type="module">
        // Import the functions you need from the SDKs you need
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.3/firebase-app.js";
        // TODO: Add SDKs for Firebase products that you want to use
        // https://firebase.google.com/docs/web/setup#available-libraries
        import { getFirestore, addDoc, collection } from "https://www.gstatic.com/firebasejs/9.6.3/firebase-firestore.js"

      
        // Your web app's Firebase configuration
        const firebaseConfig = {
          apiKey: "_API_KEY_",
          authDomain: "_PROJECT_ID_.firebaseapp.com",
          projectId: "_PROJECT_ID_",
          storageBucket: "_PROJECT_ID_.appspot.com",
          messagingSenderId: "_SENDER_ID_",
          appId: "_APP_ID_",
        };
      
        // Initialize Firebase
        const app = initializeApp(firebaseConfig);

        const db = getFirestore();
      </script>
</body>
</html>
```

<p> Cloud Firestore er nå klar til bruk på nettsiden din, og nettsiden kan nå kommunisere direkte med databasen. </p>

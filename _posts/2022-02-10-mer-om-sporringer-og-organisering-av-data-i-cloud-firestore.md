---
layout: post
title: "Mer om spørringer og organisering av data i Cloud Firestore"
date: 2022-02-10 14:30:00 +0100
categories: database firebase
---
_Denne artikkelen er under arbeid. Den vil oppdateres fortløpende med mer info._

Fra datamodellering med relasjonsdatabaser (SQL) er du kanskje vant til at data organiseres over flere tabeller som har relasjoner til hverandre. Om man for eksempel har en database over hoteller, og vil registrere hotellrommene på hvert enkelt hotell, ville vi i SQL ha lagd en tabell for hoteller, og en tabell for hotellrom, og koblet disse sammen med en en-til-mange-relasjon.

![Databasemodell av database for hotell og hotellrom](/img/2022-02-04-organisering-av-data-i-cloud-firestore/Hotell_db_mod.svg)

# Samlinger og undersamlinger
I dokumentbaserte databaser (NoSQL) organiseres data på en annen måte. Siden hvert enkelt databaseobjekt er lagret som et frittstående dokument, med i utgangspunktet ingen regler for hvilke data som skal inn i hvert enkelt dokument, kan vi ikke basere oss på koblinger med nøkler og fremmednøkler, slik som i SQL. I dokumentbaserte databaser er dokumenter i stedet organisert i _samlinger_ (_collections_) og _undersamlinger_ (_subcollections_). Hotelldatabasen i eksemplet vårt vil da være organisert med alle hotellene i en samling, og rommene som undersamlinger til hvert hotell.

![Modell av dokumentdatabase med samlinger, dokumenter og undersamlinger](/img/2022-02-04-organisering-av-data-i-cloud-firestore/dokumentdatabase-modell-hotell.jpg)

Dette kan virke ulogisk om du er vant til relasjonsdatabaser, der det legges mye vekt på struktur, og det er et viktig prinsipp å unngå dobbeltlagring, men begge modellene følger samme prinsipp; ett hotell har flere rom, og ett rom hører til ett hotell. 

NoSQL gir deg derimot mer fleksibilitet. Det er ingenting i veien for at flere hoteller har like romnummer (som de ofte har), fordi rommene lagres under hvert hotell, og ID-ene trenger da bare å være unike for hvert hotell. Det er heller ikke noe problem om et hotell ikke har romnummer, men unike navn på rommene, eller kombinasjoner av bokstaver og tall. Det kan også variere hva slags informasjon som er relevant å ha med i databasen ut fra profilen til hotellet. Noen hotell har kanskje fokus på velvære, og vil registrere om rommet har boblebad, steamdusj eller massasjestoler, mens andre vil bare registrere om rommet har bad eller ikke. Det er ikke noe problem i en NoSQL-database.

# Mange-til-mange-relasjoner
Hva med mange-til-mange-relasjoner? Se for deg en database for å registrere elever, hvilke fag de har tatt, og hvilken karakter de har fått i de ulike fagene. En datamodell for denne relasjonsdatabasen kan se slik ut:

![Databasemodell av database for elever og fag](/img/2022-02-04-organisering-av-data-i-cloud-firestore/elev_fag_dbmod.svg)

I Firebase kan dette gjøres på nesten samme måte, ved å lage to parallelle samlinger for elever og fag, og en egen samling for å holde orden på hvilke elever som har tatt hvilke fag, og hvilke karakterer de har fått. 

![Modell av dokumentdatabase med parallelle samlinger og dokumenter](/img/2022-02-04-organisering-av-data-i-cloud-firestore/dokumentdatabase-modell-elev_fag.jpg)

## Spørringer mot flere samlinger
Forskjellen på dette og en mange-til-mange relasjon i SQL er hvordan spørringer fungerer. I SQL, kunne vi brukt en spørring med JOIN for å hente data fra flere tabeller samtidig. NoSQL har ikke støtte for fremmednøkler og JOIN-operasjoner, så for å hente ut data fra flere tabeller, må vi kjøre flere separate spørringer. I NoSQL kan vi heller ikke definere på forhånd hvilke felter vi vil hente ut, slik som vi kan gjøre i SQL med SELECT.

For å hente ut navn og karakter på alle elever i faget Utvikling, kunne SQL-spørringen sett slik ut:
```sql
SELECT Elev.fornavn, Elev.etternavn, Elev_har_Fag.Karakter
FROM Fag JOIN Elev_har_Fag JOIN Elev
ON Elev.ElevID = Elev_har_Fag.Elev_ElevID AND Fag.FagID = Elev_har_Fag.Fag_FagID
WHERE Fag.Fagnavn = "Utvikling";
```

I Cloud Firestore, må vi lage flere atskilte spørringer, slik:
```javascript
//Henter alle dokumenter fra samlingen Fag der fagnavn er Utvikling, og lagrer de i arrayen fagSporring
const fagSporring = await getDocs(query(collection(db, "Fag"), where("Fagnavn", "==", "Utvikling")));
//(I praksis vil spørringen returnere kun ett dokument, men det blir likevel lagret som en array med ett element)

//Henter alle dokumenter fra samlingen Elev_har_Fag (dvs id, ElevID, FagID, år og karakter) der FagID er lik  
//resultatet fra fagSporring, og lagrer de i arrayen elevHarFagSporring. 
const elevHarFagSporring = await getDocs(query(collection(db, "Elev_har_Fag"), where("FagID", "==", fagSporring[0].id)));
//Siden getDocs returnerer en array må vi angi at vi skal ha data fra element 0 i arrayen 
//(arrayen har i dette tilfellet bare ett element).

//Oppretter en array for å lagre all data om elever 
const elevArr = [];
//Oppretter en (parallell) array for å lagre karakterer
const karakterArr = [];

//Kjører en løkke som går gjennom alle dokumenter fra spørringen elevHarFagSporring.
elevHarFagSporring.forEach((dokument) => {
  //Henter inn hver enkelt elevdokument og lagrer i arrayen elevArr. getDoc henter et dokument med en bestemt id. 
  elevArr.push(await getDoc(doc(db, "Elev", dokument.data().ElevID)));
  //Alternativt kunne dette vært formulert som en spørring, slik: 
  //elevArr.push(await getDocs(query(collection(db, "Elev"), where("__name__", "==", dokument.data.ElevID)));
  //Forskjellen er at getDocs returnerer en array, og elevArr vil da bli en nøstet array.
  
  //Henter karakteren til hver elev og lagrer i arrayen karakterArr. 
  karakterArr.push(dokument.data().karakter);
  //Denne arrayen får da parallelle verdier med ElevArr, altså vil karakterene komme i rekkefølge med riktig elev.
});

//Skriver ut fornavn og etternavn (fra elevArr) og karakter (fra karakterArr) til alle elever
for(i = 0; i < elevArr.length; i++){
  console.log(elevArr[i].data().fornavn, elevArr[i].data().etternavn, karakterArr[i];
}
```


Her kan du lære mer om organisering av data i Cloud Firestore og i NoSQL-databaser generelt:
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/v_hR4K4auoQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

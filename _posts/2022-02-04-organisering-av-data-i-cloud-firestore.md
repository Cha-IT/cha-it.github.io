---
layout: post
title: "Organisering av data i Cloud Firestore"
date: 2022-01-28 14:30:00 +0100
categories: database firebase
---
_Denne artikkelen er under arbeid. Den vil oppdateres fortløpende med mer info._

Fra datamodellering med relasjonsdatabaser (SQL) er du kanskje vant til at data organiseres over flere tabeller som har relasjoner til hverandre. Om man for eksempel har en database over hoteller, og vil registrere hotellrommene på hvert enkelt hotell, ville vi i SQL ha lagd en tabell for hoteller, og en tabell for hotellrom, og koblet disse sammen med en en-til-mange-relasjon.

![Databasemodell av database for hotell og hotellrom](/img/2022-02-04-organisering-av-data-i-cloud-firestore/Hotell_db_mod.svg)

I dokumentbaserte databaser (NoSQL) organiseres data på en annen måte. Siden hvert enkelt databaseobjekt er lagret som et frittstående dokument, med i utgangspunktet ingen regler for hvilke data som skal inn i hvert enkelt dokument, kan vi ikke basere oss på koblinger med nøkler og fremmednøkler, slik som i SQL. I dokumentbaserte databaser er dokumenter i stedet organisert i _samlinger_ (_collections_) og _undersamlinger_ (_subcollections_). Hotelldatabasen i eksemplet vårt vil da være organisert med alle hotellene i en samling, og rommene som undersamlinger til hvert hotell.

![Modell av dokumentdatabase med samlinger og dokumenter](/img/2022-02-04-organisering-av-data-i-cloud-firestore/dokumentdatabase-modell-hotell.jpg)

Dette kan virke ulogisk om du er vant til relasjonsdatabaser, der det legges mye vekt på struktur, og det er et viktig prinsipp å unngå dobbeltlagring.

Her kan du lære mer om organisering av data i Cloud Firestore og i NoSQL-databaser generelt:
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/v_hR4K4auoQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

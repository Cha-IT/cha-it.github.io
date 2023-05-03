---
layout: post
title: "Intro til programmering i Python"
date: 2023-05-03 12:00:00 +0100
categories: programmering python
---

![Python-slange](/img/2023-05-03-intro-til-python/Python_Regius.jpeg)

_Bilde: Sandro De Sousa, [CC BY 3.0](https://creativecommons.org/licenses/by/3.0), via Wikimedia Commons_

# Hva er Python?

Python er et høynivåspråk som ble utviklet på slutten av 80-tallet og er fortsatt en av de mest populære programmeringsspråkene i dag. Det er kjent for å være lett å lære, ha en ren og lesbar syntaks, og ha et bredt spekter av bruksområder.

Her er noen av funksjonene og bruksområdene til Python:

- Python er et tolket språk, som betyr at koden din ikke trenger å kompileres før den kjøres. Dette gjør utviklingen raskere og mer fleksibel, og gir også mulighet for interaktiv koding.

- Python har en stor standardbibliotek som gir tilgang til mange nyttige funksjoner og verktøy. Biblioteket inkluderer alt fra å arbeide med nettverk og filer til dataanalyse og maskinlæring.

- Python er et objektorientert språk, noe som betyr at det støtter objektorientert programmering (OOP). Dette gjør det enkelt å organisere og strukturere kode og gjør det også lettere å gjenbruke kode.

- Python støtter også funksjonell programmering, som kan hjelpe til med å skrive mer modulær og lesbar kode.

- Python er mye brukt innen dataanalyse, maskinlæring og kunstig intelligens. Biblioteker som NumPy, Pandas, Scikit-Learn og TensorFlow gjør det enkelt å utføre avanserte analyser og maskinlæringsoppgaver.

- Python brukes også mye til webutvikling, spesielt med rammeverk som Django og Flask. Disse rammeverkene gjør det enkelt å bygge webapplikasjoner og API-er.

- Python er også populært for vitenskapelige beregninger og simuleringer, og det brukes mye innenfor fysikk, matematikk og ingeniørfag.

- Python er et godt valg for automatisering og skripting, da det kan brukes til å automatisere oppgaver på operativsystemnivå eller for å skrive enkle skript som automatiserer oppgaver i en rekke forskjellige programmer.

Dette er bare noen av bruksområdene til Python, og det er mange flere. Python er et allsidig språk som kan brukes til en rekke forskjellige oppgaver, og det er enkelt å lære og bruke for både nybegynnere og erfarne utviklere.



# Kom i gang med Python 

![Python-logo](/img/2023-05-03-intro-til-python/Python_logo_01.svg)

_Bilde: Dnu72, [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0), via Wikimedia Commons_

Det er to ting du må gjøre før du kan begynne å programmere i Python på PC-en din.

## 1: Installer Python på datamaskinen din

### 1.1 Installere Python på Windows

1. Gå til https://www.python.org/downloads/ i nettleseren din.
2. Scroll ned til "Download for Windows" og klikk på den store gule knappen som sier "Download Python 3.x.x".
3. I neste skjermbilde vil du se en oversikt over tilgjengelige nedlastingsalternativer. Velg den nyeste versjonen av Python 3.x.x (hvor x er det nyeste undernummeret) for ditt operativsystem (32-bit eller 64-bit).
4. Når du har valgt riktig versjon for datamaskinen din, klikker du på linken for å laste ned installasjonsprogrammet.
5. Når nedlastingen er fullført, åpner du filen du nettopp har lastet ned og kjører installasjonsprogrammet.
6. Følg instruksjonene i installasjonsprogrammet for å fullføre installasjonen. Pass på å velge alternativet for å legge til Python til PATH-variabelen din, slik at du kan kjøre Python fra kommandolinjen.
7. Når installasjonen er fullført, kan du åpne et kommandolinjevindu og skrive "python" for å starte Python-interpreteren og begynne å skrive kode.

### 1.2 Installere Python på MacOS

1. Åpne en nettleser og gå til https://www.python.org/downloads/.
2. Klikk på knappen "Download Python" som tilsvarer den nyeste versjonen av Python for Mac.
3. Velg "macOS installer" for å laste ned installasjonsfilen.
4. Åpne installasjonsfilen og dobbeltklikk på "Python.mpkg"-filen for å starte installasjonen.
5. Følg installasjonsveiledningen ved å godta lisensavtalen og velge ønsket installasjonsmappe.
6. Vent til installasjonen er fullført.

Når Python er installert, kan du åpne en terminal og skrive "python" for å starte Python-konsollen og begynne å skrive kode.

## 2: Installer Python i Visual Studio Code

Her er en kort guide for å komme i gang med Visual Studio Code og Python:

1. Last ned og installer Visual Studio Code fra https://code.visualstudio.com/.
2. Åpne Visual Studio Code og installer Python-utvidelsen ved å gå til "Extensions" i sidepanelet (Ctrl + Shift + X) og søke etter "Python". Velg "Python" fra listen over tilgjengelige utvidelser og klikk på "Install".
3. Lag en ny mappe for prosjektet ditt og åpne den i Visual Studio Code ved å gå til "File" > "Open Folder" og velge mappen.
4. Opprett en ny Python-fil ved å gå til "File" > "New File" og lagre filen som "main.py" i mappen.
5. Skriv din første Python-kode i main.py-filen. For eksempel:

```python
print("Hello, World!")
```

6. Lagre filen ved å trykke på Ctrl + S eller gå til "File" > "Save".
7. Kjør koden ved å åpne en terminal i Visual Studio Code ved å gå til "View" > "Terminal" eller ved å trykke på Ctrl + Shift + `. Skriv inn følgende kommando i terminalen:

```python
python main.py
```

Dette vil kjøre programmet ditt og skrive ut "Hello, World!" i konsollen.

Du kan også kjøre koden ved å klikke på "Run" i toppmenyen og deretter velge "Run Without Debugging" eller ved å trykke på F5.

Visual Studio Code gir deg også tilgang til mange nyttige funksjoner for å utvikle Python-programmer, som automatisk påfylling av kode, feilsøking, enhetstesting og mye mer. Utforsk de forskjellige funksjonene for å øke produktiviteten og effektiviteten din som Python-utvikler.



# Grunnleggende begreper og konsepter i Python

Her er noen grunnleggende konsepter i Python

Variabler
----------
![En eske med et spørsmålstegn på](/img/2023-05-03-intro-til-python/box.webp)

_Bilde: Image by BedexpStock_

En variabel er en plass i minnet som kan inneholde en verdi. I Python kan du definere en variabel ved å gi den et navn og sette en verdi til det navnet. For eksempel kan du skrive:
```python
navn = "Ola Nordmann"
alder = 30
```
Her definerer vi to variabler: `navn` og `alder`. `navn` har en verdi av en streng (`"Ola Nordmann"`) og `alder` har en verdi av et heltall (`30`).

Valgsetninger
-------------
![Illustrasjon av et valg mellom to alternativer](/img/2023-05-03-intro-til-python/Ifelselogic.png)

_Bilde: Vihangvk at English Wikibooks, [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0), via Wikimedia Commons_

Valgsetninger brukes til å utføre forskjellige handlinger avhengig av en betingelse. I Python kan du bruke `if`, `elif` og `else` for å lage en valgsetning. For eksempel kan du skrive:
```python
alder = 30
if alder < 18:
    print("Du er for ung til å stemme")
elif alder >= 18 and alder < 67:
    print("Du kan stemme ved valget")
else:
    print("Du er for gammel til å stemme")
```
Her sjekker vi alderen til en person og skriver ut en passende melding basert på alderen. Hvis alderen er mindre enn 18, skriver vi ut "Du er for ung til å stemme". Hvis alderen er mellom 18 og 67, skriver vi ut "Du kan stemme ved valget". Hvis alderen er større enn 67, skriver vi ut "Du er for gammel til å stemme".

Løkker
-------
![To piler i sirkel](/img/2023-05-03-intro-til-python/loop.png)

Løkker brukes til å gjenta en handling flere ganger. I Python kan du bruke `for`-løkker og `while`-løkker. For eksempel kan du skrive:
```python
for i in range(1, 11):
    print(i)
```
Her bruker vi en `for`-løkke til å skrive ut tallene fra 1 til 10. `range(1, 11)` genererer tallene fra 1 til 10, og `for i in range(1, 11)` betyr at vi gjentar handlingen for hvert tall i dette området.

En annen måte å gjenta en handling på er å bruke en `while`-løkke. For eksempel kan du skrive:
```python
tall = 1
while tall <= 10:
    print(tall)
    tall += 1
```
Her bruker vi en `while`-løkke til å skrive ut tallene fra 1 til 10. `tall` starter som 1, og løkken gjentar handlingen så lenge `tall` er mindre eller lik 10. Vi øker `tall` med 1 for hver gjentakelse av løkken ved hjelp av `tall += 1`.

Dette er bare en kort introduksjon til variabler, valgsetninger og løkker i Python. Det er mye mer å lære når det gjelder Python-programmering, inkludert funksjoner, lister, tupler, ordbøker og mer. Men dette er en god start!

Funksjoner
-------
![Puslespill med en brikke løftet ut](/img/2023-05-03-intro-til-python/Puzzle.jpeg)

_Bilde: Alexandra, Alexas_Fotos, CC0, via Wikimedia Commons_

Funksjoner i Python brukes til å dele opp koden din i mindre biter som kan gjenbrukes flere steder i programmet. Du kan tenke på en funksjon som en ferdigskrevet bit med kode som utfører en bestemt oppgave, og som kan kalles fra andre steder i programmet når du trenger å utføre den oppgaven.

For å lage en funksjon i Python bruker du nøkkelordet `def`, etterfulgt av funksjonsnavnet og parenteser, og så koden du vil kjøre i funksjonen. Hvis funksjonen skal returnere en verdi, bruker du nøkkelordet `return`.

Her er et enkelt eksempel på hvordan du kan bruke en funksjon i Python:

```python
def add_numbers(a, b):
    result = a + b
    return result

sum = add_numbers(2, 3)
print(sum) # Output: 5
```

I dette eksempelet definerer vi en funksjon som heter `add_numbers` som tar to argumenter, `a` og `b`. Inne i funksjonen legger vi sammen disse to tallene og lagrer resultatet i en variabel `result`, før vi returnerer resultatet. 

Så, når vi kaller funksjonen med argumentene 2 og 3 (`sum = add_numbers(2, 3)`), blir resultatet 5, som vi lagrer i variabelen `sum`. Vi skriver så ut verdien av `sum` med `print(sum)`.

Dette er bare et veldig enkelt eksempel, men funksjoner kan være mye mer komplekse og ha flere argumenter og returverdier. De er nyttige når du vil skrive kode som skal kunne brukes flere ganger, og når du vil dele opp koden din i mindre, mer oversiktlige biter.



### Bruk av funksjoner i programmer


Det er en god praksis å organisere koden som funksjoner. Her er et eksempel på et program der hele koden er organisert i funksjoner.

```python
def sjekk_alder(alder):
    if alder >= 18:
        print("Du er gammel nok til å kjøpe alkohol.")
    else:
        print("Beklager, du er ikke gammel nok til å kjøpe alkohol.")

def main():
    alder = int(input("Hvor gammel er du? "))
    sjekk_alder(alder)

if __name__ == '__main__':
    main()
```

Her defineres funksjonen `sjekk_alder` som tar inn alderen som et argument og bruker en `if`-valgsetning til å avgjøre om personen er gammel nok til å kjøpe alkohol. Funksjonen skriver ut en passende melding basert på resultatet.

I `main`-funksjonen ber vi brukeren om å oppgi alderen sin ved hjelp av `input`-funksjonen og konverterer inputen til et heltall ved hjelp av `int`-funksjonen. Deretter kaller vi `sjekk_alder`-funksjonen og gir den alderen som argument.

Til slutt sjekker vi om modulen kjøres direkte ved hjelp av `if __name__ == '__main__':`, og hvis den gjør det, kaller vi `main`-funksjonen. Dette gjør det mulig å importere modulen i andre Python-skript uten at `main`-funksjonen kjøres automatisk.


### main-funksjonen


`main`-funksjonen er vanligvis en funksjon som brukes til å starte programmet eller modulen. Det er vanlig å ha en `main`-funksjon i et Python-program for å organisere koden på en oversiktlig måte og gjøre det enklere å gjenbruke koden senere.

`main`-funksjonen kan ha forskjellige navn, men det er vanlig å kalle den `main` eller `run`. Navnet spiller ingen rolle så lenge funksjonen har en klar og beskrivende funksjon.

Typisk vil `main`-funksjonen inneholde hovedlogikken i programmet, som å samle inn data fra brukeren, behandle dataene, og skrive ut resultater. Det kan også inneholde funksjonskall og eventuelle andre nødvendige operasjoner for å starte programmet.

Det er også vanlig å plassere koden som ikke er i en funksjon, som import-setninger og konstanter, øverst i filen.

Her er et eksempel på en typisk struktur for et Python-program som bruker `main`-funksjonen:

```python
# Importer moduler og definere konstanter her

def main():
    # Hovedlogikken i programmet går her
    pass

if __name__ == '__main__':
    main()
```

I dette eksempelet er koden som ikke er inne i funksjonen plassert øverst i filen. Deretter defineres `main`-funksjonen, som inneholder hovedlogikken i programmet. Til slutt sjekker vi om modulen kjøres direkte ved hjelp av `if __name__ == '__main__':`, og hvis den gjør det, kaller vi `main`-funksjonen. Dette gjør det mulig å importere modulen i andre Python-skript uten at `main`-funksjonen kjøres automatisk.

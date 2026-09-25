---
name: resekoll
description: >
  This skill should be used when the user runs "/resekoll", says "kolla
  mina resebokningar", "stäm av schemat mot mailen", "saknar jag några
  bokningar", "vilka resor är obokade", or when the scheduled travel
  check fires. It reads the lecture schedule spreadsheet, searches the
  mailbox for booking confirmations, reports gaps with concrete travel
  suggestions, and writes the status back into the file.
metadata:
  version: "0.1.0"
---

# Resekoll — avstämning

Läs schemat, leta bokningar i mailen, rapportera luckorna med ett förslag på
vad som behöver bokas.

Saknas bladet **Inställningar** i filen: kör `/resekoll-setup` i stället.

## Grundhållning

Tre regler som avgör om verktyget blir betrott eller avstängt.

**Osäkerhet flaggas.** En lucka som rapporteras i onödan kostar tio sekunder.
En bokning som antas finnas kostar ett inställt uppdrag. Väg alltid åt att
flagga.

**Påstå aldrig en avgångstid som inte setts i ett mail.** Rapporten säger när
personen måste vara framme och vilka nätter som behövs. Valet av avgång görs i
söklänken.

**Var tyst när allt är bokat**, om inte Inställningar säger annat. Ett mail som
kommer varje vecka utan att säga något slutar bli läst, och då försvinner även
de veckor det faktiskt betyder något.

## Steg 1 — Läs in

Läs Inställningar, Leverantörer, Länkmallar och schemabladet enligt
kolumnmappningen.

Välj ut uppdrag där datum ligger mellan idag och `Kolla framåt`. Hoppa
därefter över:

- rader där **arrangören bokar resan**
- rader på **hemorten**

De ska aldrig generera vare sig sökning, lucka eller förslag.

## Steg 2 — Avgör behovet per uppdrag

För varje kvarvarande rad, bestäm om **boende** och **resa** behövs. Säger
kolumnen `Boende krävs` något annat än `auto` gäller den, annars reglerna:

- **Restid avgör, inte kilometer.** Går det att vara framme före
  ankomstkravet och lämna efter `Hemresa tidigast` samma dag, räcker en
  dagsresa — även när avståndet passerar `Dagsresa upp till`. Kilometergränsen
  är en genväg för att slippa slå upp restider, aldrig ett skäl att boka en
  natt som inte behövs
- **Pendelavstånd larmar aldrig om biljett.** Ligger orten inom hemortens
  lokaltrafik köps resan på plats. Kontrollera boende som vanligt, men
  efterlys aldrig en biljett ingen bokar i förväg
- Start före `Tidig start kräver natt innan` och längre bort → natt innan
- Slut efter `Sen sluttid kräver natt efter` och längre bort → natt efter
- Utland och `Utlandsresa kräver alltid boende` = Ja → boende

Avstånd och restid behöver ingen exakthet — storleksordningen räcker för att
välja regel. Cacha det som slagits upp i Inställningar så nästa körning slipper
göra om jobbet.

**Slå ihop uppdrag som hänger ihop** innan sökningen: samma ort på efter
varandra följande dagar är en resa med flera nätter, inte två separata luckor.

**Uppdrag strax utanför fönstret** som kräver utlandsresa eller flyg: ta med en
rad som förvarning, utan att räkna dem som luckor. Ett utlandsuppdrag som dyker
upp i larmlistan först om en vecka har redan tappat en veckas framförhållning.

## Steg 3 — Hämta kandidater ur mailen

Hämta mail i `~~mail` från de senaste `Sök bakåt i mailen` dagarna, med
avsändare i de aktiva domänerna på bladet Leverantörer. Domänfiltret först
håller körningen billig; tolkningen därefter gör den träffsäker.

Tolka varje kandidat till: **typ** (hotell, tåg, flyg, taxi, hyrbil),
**status** (bekräftad, ändrad, avbokad, marknadsföring, påminnelse, övrigt),
**datum**, **ort eller sträcka**, **referensnummer**.

Kasta marknadsföring, påminnelser om redan kända bokningar och bokningar som
ligger i det förflutna.

## Steg 4 — Matcha

Matcha kandidat mot uppdrag på ort och datum. `references/matchningsregler.md`
har fallen i detalj. De som avgör utfallet:

- **Senaste beskedet vinner.** En avbokning eller ombokning upphäver en
  tidigare bekräftelse med samma referensnummer. Att hitta ett träffande mail
  räcker alltså inte — hela tråden måste läsas.
- **Datum som nästan stämmer är ett fel, ingen träff.** Ett hotell bokat en
  månad eller en dag fel rapporteras som felbokat, med båda datumen utskrivna.
  Det är den nyttigaste larmtypen som finns.
- **Ort som nästan stämmer är ingen träff.** Samma stad men fel datum, eller
  rätt datum men fel stad, är två olika fel — håll isär dem.
- **Överflödiga bokningar** noteras utan att larma. Ett hotell på en dagsresa
  kostar pengar, aldrig ett missat uppdrag.
- **En träff ska också hålla måttet.** Kontrollera varje bokning som matchat
  mot fönstret i steg 5: ankommer biljetten efter `Framme senast`, eller går
  hemresan före `Hemresa tidigast`, rapportera den som **för tight** med båda
  tiderna utskrivna. En bokning som finns är inte samma sak som en bokning som
  fungerar, och det är ett fel personen själv aldrig upptäcker förrän på
  perrongen.

## Steg 5 — Bygg förslag för varje lucka

Räkna ur fönstret:

- **Måste vara framme** = starttid minus `Framme senast`
- **Tidigast hemresa** = sluttid plus `Hemresa tidigast`
- **Nätter** som följer av behovet i steg 2

Välj färdsätt: under `Tåg framför flyg upp till` föreslås tåg även när flyg går
fortare. Räcker restiden hemifrån ändå inte till för att vara framme i tid,
faller förslaget tillbaka på att resa kvällen innan.

Ankra mot det som redan är bokat. Finns hotellet från den 2:a ska resan gå den
2:a, oavsett vad gig-datumet säger.

Bygg söklänken ur Länkmallar, med lokaladressen när den finns så att träffarna
sorteras efter gångavstånd. Ligger lokalen på ett hotell, föreslå det hotellet
först — noll meter slår varje kedjepreferens.

Är något felbokat: säg att det ska åtgärdas **innan** något nytt bokas. En
andra bokning ovanpå ett misstag gör saken värre.

## Steg 6 — Leverera

**I Excel:** skriv status, förslag och dagens datum i statuskolumnerna. Rör
inga andra kolumner. Är filen låst av att någon har den öppen, rapportera i
mailet ändå och säg att filen inte gick att uppdatera.

**I mailet:** skicka bara när något behöver göras. Luckorna sorteras efter hur
nära i tiden de ligger, det som ligger inom `Kritisk gräns` först. Håll varje
lucka på tre rader: vad som saknas, vilket fönster som gäller, och länken.

Jämför mot statusen från förra körningen och lyft det som **förändrats** överst
— en ny lucka, eller en lucka som stängts. Samma lista oförändrad vecka efter
vecka är det snabbaste sättet att göra mailet osynligt.

`references/rapportmall.md` har formatet.

## Torrläge

Kallas skillen med `torrläge`, `dry run` eller `utan att skicka`: rapportera
allt i chatten och skriv varken i filen eller till mailen. Uppsättningen
använder det för sin provkörning.

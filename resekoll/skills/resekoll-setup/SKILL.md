---
name: resekoll-setup
description: >
  This skill should be used when the user runs "/resekoll-setup", says
  "sätt upp resekollen", "konfigurera resekoll", "koppla min
  föreläsningsfil", "ändra inställningarna för resekollen", or when
  /resekoll runs and finds no Inställningar sheet in the schedule file.
  It locates the user's lecture schedule spreadsheet, maps its columns,
  interviews the user for the settings that cannot be guessed, writes
  everything into the file and creates the scheduled run.
metadata:
  version: "0.1.0"
---

# Resekoll — uppsättning

Kör en gång per person. Resultatet är en schemafil som är redo att stämmas av,
och en schemalagd körning som gör det varje vecka.

Ställ frågor med AskUserQuestion, en omgång i taget. Gissa aldrig ett värde som
bara personen kan känna till — särskilt inte hemorten.

## Steg 1 — Hitta schemafilen

Fråga aldrig efter en sökväg först. Leta upp kandidater och lägg fram dem.

Sök i `~~fillagring` och i anslutna mappar efter `.xlsx` vars namn innehåller
något av: föreläsning, schema, gig, uppdrag, bokning, kalender, innevarande
eller kommande årtal. Öppna de fem mest lovande och läs rubrikraden.

En fil är en trolig kandidat när rubrikerna rymmer **ett datum plus en ort
eller en kund**. Rangordna efter hur många schemafält som känns igen och hur
många rader som ligger i framtiden.

Lägg fram högst fyra kandidater med filnamn, antal kommande rader och de
kolumner som känns igen, så att valet går att göra utan att öppna filerna.
Hittas inget: be om filen eller sökvägen rakt ut.

Har filen flera blad, fråga vilket som gäller. Ligger rubrikraden någon
annanstans än rad 1, säg vilken rad som tolkats som rubrik och be om
bekräftelse.

## Steg 2 — Mappa kolumnerna

Läs rubrikraden och föreslå en mappning. Visa gissningen och be om
bekräftelse i klump — fråga alltså inte kolumn för kolumn.

Obligatoriska fält: **datum**, **ort**, **kund**.
Värdefulla när de finns: starttid, sluttid, lokal, lokaladress, typ.

Saknas starttid går reglerna om tidig start inte att använda. Säg det, och
erbjud att lägga till kolumnen.

Saknas **vem som bokar resan** — vilket är det vanliga — lägg till kolumnen,
förklara att den avgör vilka uppdrag som ska lämnas i fred, och sätt alla rader
till `Jag`. Be personen ändra de rader där arrangören bokar. Det är den enda
efterarbetet uppsättningen ger.

Läs `references/kolumnmappning.md` för rubriknamn som brukar förekomma,
datumformat och hur udda fall hanteras.

## Steg 3 — Bygg ut filen

Ta en säkerhetskopia före första skrivningen: samma mapp, samma namn med
`_backup_ÅÅÅÅ-MM-DD`. Säg var den ligger.

Lägg sedan till det som saknas, utan att röra en enda befintlig kolumn:

- Bladet **Inställningar** — svaren från steg 4
- Bladet **Leverantörer** — avsändardomäner, förifyllt från `references/leverantorer.md`
- Bladet **Länkmallar** — söklänksmönster, förifyllt från `references/lankmallar.md`
- Statuskolumner sist i schemabladet: status boende, status resa dit, status
  resa hem, status lokalt, förslag boende, förslag resa, senast kontrollerad,
  noteringar

Finns bladen redan: uppdatera i stället för att skriva över, och behåll värden
personen själv ändrat.

## Steg 4 — Intervjun

Två omgångar. Första omgången innehåller det som måste besvaras.

**Omgång 1 — går ej att gissa**

- **Hemort.** Fråga rakt ut. Härled aldrig hemorten ur gigen i filen: att en
  ort återkommer ofta betyder att den bokar mycket, inte att personen bor där.
  Finns mailkopplingen redan kan den vanligaste avresestationen i tidigare
  biljetter läggas fram som ett förslag att bekräfta — som förslag, aldrig som
  slutsats.
- **Hemstation och hemflygplats.** Föreslå utifrån hemorten och be om
  bekräftelse. Här är gissningen rimlig eftersom den följer av svaret ovan.
- **Notifieringsmail.** Dit sammanfattningen går. Föreslå den inloggade adressen.
- **Körschema.** Veckovis är lagom. Föreslå måndag morgon och fråga om dag och tid.

**Omgång 2 — defaults som får ändras**

Lägg fram de här som färdiga värden och fråga om något ska justeras, i stället
för att fråga om vart och ett:

| Inställning | Default |
| --- | --- |
| Dagsresa upp till | 120 km |
| Tidig start kräver natt innan | före 10:00 |
| Sen sluttid kräver natt efter | efter 20:00 |
| Utlandsresa kräver alltid boende | Ja |
| Framme senast | 45 min före start |
| Hemresa tidigast | 30 min efter slut |
| Max gångavstånd till lokalen | 900 m |
| Tåg framför flyg upp till | 600 km |
| Kolla framåt | 75 dagar |
| Kritisk gräns | 14 dagar |
| Sök bakåt i mailen | 240 dagar |
| Tyst när allt är bokat | Ja |

Fråga dessutom om hotellkedjor i prioordning och eventuellt bonusprogram —
det påverkar vilken söklänk som byggs först och är lätt att svara på.

## Steg 5 — Provkörning

Kör `/resekoll` direkt, i **torrläge**: rapportera i chatten utan att skicka
mail eller skriva i filen.

Gå igenom resultatet tillsammans. Den här stunden är värd mer än hela
intervjun — det är nu felmappade kolumner och en missförstådd hemort visar sig,
medan de fortfarande kostar tio sekunder att rätta.

Fråga särskilt: ser luckorna rimliga ut, och saknas något uppdrag som borde
vara med? Rätta och kör om tills bilden stämmer.

## Steg 6 — Schemalägg

Skapa den schemalagda körningen med `create_trigger` på Claude Code Remote
(aldrig med de lokala cron-verktygen — de överlever inte sessionen). Prompten
ska vara fristående och peka ut filen, eftersom varje körning börjar från
ingenting.

Notera schemat i Inställningar och säg vad som händer härnäst: när första
riktiga körningen sker, och att `/resekoll` kan köras manuellt när som helst.

## Om något fattas

Saknas mailkopplingen går uppsättningen ändå att slutföra — säg vad som
återstår och att avstämningen börjar fungera när kopplingen finns på plats.

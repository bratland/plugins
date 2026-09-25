# Kolumnmappning

Rubriker att känna igen när schemabladets kolumner ska tolkas. Matcha utan
hänsyn till versaler och extra blanksteg, och acceptera delträffar.

| Fält | Rubriker som brukar förekomma |
| --- | --- |
| Datum | datum, dag, när, date, startdatum, speldatum |
| Starttid | tid, start, klockan, från, kl, starttid |
| Sluttid | slut, till, sluttid, klar |
| Ort | ort, stad, plats, var, destination, city |
| Land | land, country |
| Lokal | lokal, plats, venue, anläggning, hotell, konferens |
| Lokaladress | adress, address, gatuadress, besöksadress |
| Kund | kund, uppdragsgivare, beställare, företag, arrangör, client |
| Typ | typ, uppdrag, format, kategori |
| Vem bokar | resa, bokas av, bokning, vem bokar |
| Arvode | arvode, pris, belopp, fee, summa |

## Datumformat

Svenska filer blandar `2026-10-15`, `15/10`, `15 okt` och rena Excel-serienummer.
Läs en handfull rader innan formatet slås fast.

Saknas årtal, anta innevarande år för datum som ligger framåt och nästa år för
datum som redan passerat — en fil skriven i december rymmer ofta januari.
Säg vilken tolkning som gjorts.

## Tider

`09:00`, `9.00`, `9`, `kl 9` och ett äkta Excel-klockslag förekommer alla.
Tolka alla till timme och minut. Står ett spann i en enda cell — `09:00–11:00`
— dela upp det i start och slut.

## Udda fall som är värda att fråga om

**Flera rader för samma uppdrag.** Två dagar i samma stad är normalt en resa.
Slå ihop vid avstämningen, men rör aldrig raderna i filen.

**Preliminära uppdrag.** Många markerar dem med färg, ett frågetecken eller
ordet prel. Färg går inte att lita på — fråga om det finns en kolumn för status
och vilka värden som betyder bokat.

**Inställda uppdrag** som ligger kvar i filen. Fråga hur de markeras, så att
verktyget slipper larma om resor till något som ställts in.

**Tomma rader mitt i.** Vanligt som avdelare mellan månader. Hoppa över dem
utan att tolka det som filens slut.

**Uppdrag utan ort.** Digitala föreläsningar. Behöver varken boende eller resa
— fråga hur de markeras, ofta räcker ordet digitalt eller Teams i lokalfältet.

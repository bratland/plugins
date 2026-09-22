# raion — publika Claude-plugins

Verktyg för återkommande arbete som annars görs för hand. Varje plugin bor i
sitt eget repo och installeras med sin egen licens.

## Kom igång

```
/plugin marketplace add bratland/plugins
/plugin install resekoll@raion
```

Uppdatera senare med `/plugin marketplace update`.

## Innehåll

| Plugin | Skills | Vad det gör |
| --- | --- | --- |
| `resekoll` | 2 | Stämmer av ett föreläsningsschema mot resebokningar i mailen |

### resekoll

För den som föreläser och håller sitt schema i Excel. Varje vecka läses schemat,
mailen genomsöks efter bokningsbekräftelser på boende och resa, och det som
saknas kommer i ett mail — med resefönster och färdig söklänk, i tid nog att
hinna boka.

Uppsättningen intervjuar användaren och anpassar sig till kolumnerna i den
befintliga filen. Alla personliga inställningar bor i schemafilen, inget i
pluginet, så nästa person installerar samma plugin och fyller i sitt eget blad.

Kräver en mailkoppling. Källa: [bratland/resekoll](https://github.com/bratland/resekoll) · MIT

> Version 0.2.0 är prövad mot en demofil men aldrig mot riktig maildata. Kör den
> i torrläge mot den medföljande `Resekoll_demo.xlsx` först, och jämför med
> bladet `Facit`.

## Förutsättningar

Plugins installeras i Claude Cowork eller Claude Code. De som läser mail eller
filer kräver att motsvarande koppling finns på plats — varje plugins
`CONNECTORS.md` säger vilka.

## Rapportera problem

Ärenden hör hemma i respektive plugins repo, inte här.

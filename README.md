# raion — publika Claude-plugins

Verktyg för återkommande arbete som annars görs för hand. Varje plugin har sin
egen licens.

Varje plugin bor i det här repot, i en egen mapp, och listas med en relativ
sökväg. Plugins låg först i varsitt eget repo som marketplacen pekade på med en
`github`-källa: terminalen klarar det, men en installation från skrivbordsappen
föll på just den korsreferensen. Pluginens egna repon står kvar som kanonisk
källa för ärenden och historik.

## Kom igång

```
/plugin marketplace add https://github.com/bratland/plugins.git
/plugin install resekoll@raion
```

Uppdatera senare med `/plugin marketplace update`.

**Skriv hela adressen.** Kortformen `bratland/plugins` får Claude att klona över
SSH, vilket kräver att du har en SSH-nyckel registrerad hos GitHub. Med hela
`https://`-adressen hämtas den över vanlig HTTPS och fungerar utan GitHub-konto.

Har du varken git eller lust att hålla på med kommandon: be om en
`.plugin`-fil i stället och öppna den i Claude-appen. Den installerar samma
sak utan att något behöver hämtas från GitHub.

## Innehåll

| Plugin | Skills | Vad det gör |
| --- | --- | --- |
| `resekoll` | 2 | Stämmer av ett föreläsningsschema mot resebokningar i mailen |
| `seo-genomlysning` | 1 | Genomlyser en sajt för sök och AI-synlighet, och bygger åtgärderna |

### resekoll

För den som föreläser och håller sitt schema i Excel. Varje vecka läses schemat,
mailen genomsöks efter bokningsbekräftelser på boende och resa, och det som
saknas kommer i ett mail — med resefönster och färdig söklänk, i tid nog att
hinna boka.

Uppsättningen intervjuar användaren och anpassar sig till kolumnerna i den
befintliga filen. Alla personliga inställningar bor i schemafilen, inget i
pluginet, så nästa person installerar samma plugin och fyller i sitt eget blad.

Kräver en mailkoppling. Filerna bor i det här repot, under `resekoll/`. Kanonisk källa:
[bratland/resekoll](https://github.com/bratland/resekoll) · MIT

> Version 0.2.0 är prövad mot en demofil men aldrig mot riktig maildata. Kör den
> i torrläge mot den medföljande `Resekoll_demo.xlsx` först, och jämför med
> bladet `Facit`.

### seo-genomlysning

För den som vill veta varför sajten inte syns, och sedan få det åtgärdat.
Genomlysningen ställer fyra frågor om syfte, omfång, datakällor och format, och
levererar ett dokument med en åtgärdslista sorterad efter effekt per nedlagd
timme.

GEO-delen är den som skiljer den från en vanlig SEO-rapport: den kontrollerar
om sajten går att läsa och citera av språkmodeller. Konkurrensen där är tunnare
än i Google, och en sajt som bygger sitt innehåll i JavaScript är osynlig för
GPTBot, ClaudeBot och PerplexityBot, som kör noll JavaScript.

Ligger sajtens repo på maskinen bygger skillen också åtgärderna: schema,
titlar, ortsidor och intern länkning, med kontroll hela vägen ut till den
publicerade URL:en.

Läser publik data utan koppling. GA4 och Search Console används om åtkomst ges.
Filerna bor i det här repot, under `seo-genomlysning/`. Kanonisk källa med
ärenden och historik: [bratland/seo-genomlysning](https://github.com/bratland/seo-genomlysning)

## Förutsättningar

Plugins installeras i Claude Cowork eller Claude Code. De som läser mail eller
filer kräver att motsvarande koppling finns på plats — varje plugins
`CONNECTORS.md` säger vilka.

## Rapportera problem

Ärenden hör hemma i respektive plugins repo, inte här.

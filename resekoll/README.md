# Resekoll

Stämmer av ett föreläsningsschema i Excel mot resebokningar i mailen och hör av
sig när något saknas — i tid nog att hinna boka.

## Vad det gör

Varje körning läser schemafilen, plockar ut de uppdrag som ligger inom
bevakningsfönstret, och letar efter bokningsbekräftelser på boende och resa i
mailen. Saknas något kommer ett mail med luckorna sorterade efter hur nära i
tiden de ligger, tillsammans med ett konkret reseförslag: när man måste vara
framme, vilka nätter som behövs, och en färdig söklänk.

Statusen skrivs även tillbaka i Excel-filen, som därmed också fungerar som
verktygets minne mellan körningarna.

## Två skills

**`/resekoll-setup`** körs en gång. Den letar rätt på schemafilen, läser dess
kolumner, bekräftar mappningen, ställer de frågor som behöver besvaras och
sätter upp den schemalagda körningen.

**`/resekoll`** är själva avstämningen. Den körs av schemat, och kan köras
manuellt när som helst.

## Installera

```
/plugin marketplace add https://github.com/bratland/plugins.git
/plugin install resekoll@raion
```

Skriv hela adressen. Kortformen `bratland/plugins` klonar över SSH och kräver
en SSH-nyckel hos GitHub; hela `https://`-adressen fungerar utan GitHub-konto.

Alternativt: öppna en `.plugin`-fil i Claude-appen, vilket installerar samma
sak utan att något hämtas från GitHub.

## Innan första körningen

Koppla en mailtjänst (se CONNECTORS.md). Ha schemafilen tillgänglig — i
OneDrive, SharePoint eller en ansluten mapp.

## Vad som lagras var

Alla personliga inställningar bor i schemafilen, på bladet **Inställningar**.
Pluginet självt innehåller inga personuppgifter, inga sökvägar och inga
inloggningar, vilket är det som gör att nästa person kan installera samma
plugin och fylla i sitt eget blad.

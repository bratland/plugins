# Kopplingar

## Så fungerar verktygsreferenser

Pluginets filer använder `~~kategori` som platshållare för den tjänst användaren
faktiskt kopplar. Det gör pluginet oberoende av leverantör — samma arbetsflöde
fungerar oavsett vilken mailtjänst föreläsaren har.

## Kopplingar för det här pluginet

| Kategori | Platshållare | Alternativ |
| --- | --- | --- |
| Mail | `~~mail` | Microsoft 365 / Outlook, Gmail |
| Fillagring | `~~fillagring` | OneDrive, SharePoint, Google Drive, lokal mapp |

Mailkopplingen är obligatorisk — utan den finns inget att stämma av mot.
Fillagringen behövs bara när schemafilen ligger i molnet. Ligger den i en
ansluten mapp på datorn läses den därifrån i stället.

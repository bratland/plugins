# Blindtest 2026-09-22

Matchningsreglerna kördes av en agent som fick schemat, inställningarna,
leverantörslistan och inkorgen — men **inte** facitbladet. Utfallet jämfördes
efteråt mot `Facit` i `Resekoll_demo.xlsx`.

## Resultat

**14 av 16 uppdrag korrekt bedömda.** Två falska larm. Noll missade luckor.

Inget uppdrag som krävde åtgärd förbisågs, vilket är den enda felsort som
kostar ett inställt uppdrag. Båda felen drog åt det ofarliga hållet.

## Vad som fungerade

Fällorna i demofilen klarades allihop:

- **Avbokningen** (gig 3) fångades. Agenten läste hela tråden, såg att M03
  upphävde M02 på samma referensnummer och rapporterade resan som obokad.
- **Månadsfelet** (gig 8) fångades. Hotellet bokat 28 november för ett uppdrag
  28 oktober rapporterades som felbokat, med kravet att rätta det före ny
  bokning.
- **Den sammanhängande resan** (gig 4 + 5) slogs ihop. Dag två rapporterades
  inte som lucka trots att den saknar egen biljett.
- **Arrangörsbokade uppdrag** (gig 10, 11) hoppades över utan att söka.
- **Bruset** sorterades bort: nyhetsbrev, faktura, incheckningspåminnelse,
  historiskt taxikvitto och två kundmail som nämner boende utan att boka det.

## Vad som brast

**Gig 7, Kungsbacka — falskt larm.** 30 km från hemorten, alltså ingen
övernattning, men reglerna saknade undantag för resor som görs med lokaltrafik
utan förbokning. Verktyget efterlyste en tågbiljett som ingen köper i förväg.

**Gig 9, Jönköping — falskt larm.** Uppdraget ligger ~150 km bort och börjar
09:30, vilket enligt km-regeln kräver natt innan. Det bokade tåget är i själva
verket framme 08:35, i god tid. Avståndet var fel mått: restiden avgör om något
går som dagsresa.

Agenten flaggade själv båda som sina osäkraste bedömningar.

## Vad testet avslöjade om testet självt

**Gig 15, Helsingfors, låg 79 dagar fram medan bevakningsfönstret är 75.**
Agenten lämnade den obedömd, helt korrekt. Facit sa larm. Facit hade fel.

Raden är numera en avsiktlig fälla för fönstergränsen, med en regel som ger en
förvarning om utlandsuppdrag strax utanför fönstret i stället för tystnad.

## Ändringar som följde

Tre regler tillkom i 0.2.0, alla direkt ur det som brast:

1. **Restid slår avstånd.** Går uppdraget att nå i tid och lämna samma dag
   räcker en dagsresa, oavsett kilometer.
2. **Pendelavstånd larmar aldrig om biljett.** Resor inom lokaltrafikens område
   bokas på plats.
3. **Befintliga bokningar valideras mot fönstret.** Testet visade att en bokad
   biljett aldrig kontrollerades mot ankomstkravet — ett tåg som kommer för
   sent hade passerat som godkänt.

Den tredje är en lucka i designen som ingen fälla i demofilen var byggd för att
hitta. Den kom fram för att agenten fick tänka själv.

## Kör om testet

Kör `/resekoll` i torrläge mot `Resekoll_demo.xlsx` och jämför med bladet
`Facit`. Görs det av en agent som ser facit bevisar det ingenting.

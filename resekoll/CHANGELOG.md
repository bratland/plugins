# Ändringar

## 0.2.0 — 2026-09-22

Första blindtestet kört: matchningsreglerna prövades mot demofilen av en agent
som inte fick se facit. 14 av 16 uppdrag korrekt bedömda, två falska larm,
noll missade luckor. Se `TESTNING.md`.

Tre regler tillkom, alla ur det som brast:

- **Restid slår avstånd.** Går uppdraget att nå och lämna samma dag räcker en
  dagsresa, oavsett kilometer. Kilometergränsen är en genväg, aldrig ett skäl
  att boka en onödig natt.
- **Pendelavstånd larmar aldrig om biljett.** Resor inom hemortens lokaltrafik
  köps på plats.
- **Befintliga bokningar valideras mot resefönstret.** En biljett som ankommer
  efter ankomstkravet rapporteras som för tight. Tidigare passerade den som
  godkänd enbart för att den fanns.

Demofilen rättad: uppdrag 15 låg 79 dagar fram med ett fönster på 75 och var
ändå markerat som larm i facit. Raden är nu en avsiktlig fälla för
fönstergränsen, med regel om förvarning för utlandsuppdrag strax utanför.

## 0.1.0 — 2026-09-22

Första versionen. Uppsättningsintervju, avstämning mot mailen, reseförslag med
söklänkar, status tillbaka i Excel.

**Fortfarande oprövat:** matchningen har aldrig körts mot riktig maildata — bara
mot demofilen — och söklänksmallarna är inte verifierade mot leverantörernas
sidor.

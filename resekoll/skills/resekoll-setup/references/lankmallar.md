# Länkmallar

Skrivs till bladet Länkmallar vid uppsättningen. Mönstret ligger i filen och
aldrig i koden: ändrar en leverantör sin söksida redigeras en rad i kalkylbladet.

Jobbet ersätter `{platshållarna}` och lägger länken i förslaget.

| Typ | Leverantör | Mall |
| --- | --- | --- |
| Tåg | SJ | `https://www.sj.se/kop-resa?from={från}&to={till}&date={datum}` |
| Tåg | Resrobot | `https://resrobot.se/?from={från}&to={till}&date={datum}` |
| Flyg | Google Flights | `https://www.google.com/travel/flights?q=Flights%20from%20{från_iata}%20to%20{till_iata}%20on%20{datum}` |
| Hotell | Booking.com | `https://www.booking.com/searchresults.html?ss={adress}&checkin={in}&checkout={ut}` |
| Hotell | Scandic | `https://www.scandichotels.se/hotell/{land}/{ort}` |
| Hotell | Google Maps | `https://www.google.com/maps/search/hotell/@{lat},{lng},15z` |

## Status: otestade

Mönstren är rimliga men **inget av dem är verifierat mot leverantörens
livesida**. Parameternamn ändras med tiden.

Öppna därför varje mall en gång under uppsättningen och se att sökningen landar
rätt. Två minuter här sparar veckor av förslag som leder till en tom söksida —
och det är precis den sortens fel som får någon att sluta lita på verktyget.

Markera i kolumnen Status vilka som testats och när.

## Adress slår ortsnamn

Använd lokaladressen i hotellänken när den finns. Då sorteras träffarna efter
gångavstånd till lokalen i stället för efter avstånd till stadens mittpunkt,
vilket är skillnaden mellan ett användbart och ett meningslöst förslag.

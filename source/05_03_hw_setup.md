## Felépítés
Így, hogy minden alkatrészt kiválasztottunk a következő feladat, hogy össze is rakjuk az eszközeinket.
Az első és legfontosabb lépés, hogy áramot kapjon a fő alkatrészünk, az *Arduino*. Magán az *Arduinon*
elhelyezett feszültségszabályzónak köszönhetően, képesek vagyunk akár egy 9V-os elemmel is megoldani
az áramellátást. A példa kedvéért most maradjunk ennél a megoldásnál, annak okán, hogy ne kelljen
fölösleges feszültségátalakításokkal foglalkozni. Valójában úgy terveztem viszont az eszközt, hogy
egy tölthető, 3.7V-os lithiumion-akkumulátor üzemeltesse a szenzorjainkat, aktorjainkat.

Ha megoldottuk az áramellátás kérdését, jöhet a kommunikáció felállítása a központi szerverrel. Ehhez
az úgynevezett *SPI* ("*Serial Peripheral Interface*") buszt kell használjuk az *Arduino* és az *nRF24L01+*
közötti kommunikációhoz. Sajnos magam sem ismerem mélyrehatóan az *SPI* technológiát, hiszen programtervező
informatikusnak tanulok, viszont röviden összefoglalva az *SPI* egy soros kommunikációs interfész, amely
*master-slave* architektúrára épül. Tehát van egy *master* eszköz és elviekben tetszőleges számú *slave*,
ahol a *master* eszköz irányítja teljes egészében a kommunikációt és a *slave* eszközök csak a *master*
jelére kezdenek el adatot küldeni. Összesen négy kábel elég ahhoz, hogy a kapcsolat létrejöjjön az
*Arduino* és az *nRF24L01+* között. Ahogy összekötöttük őket a full-duplex kapcsolaton keresztül máris
tudunk adatot küldeni más *nRF24L01+-t* használó eszközöknek. [@SPIDatasheet]

Hátra van még az eszköz specifikus rész bekötése. Természetesen ez eszközről eszköre változik, viszont
mivel szükség lesz az *Arduino* megszakítás rendszerére, az egyetlen megszorítás, hogy olyan bemenetre
kell bekötni a specifikus eszközt, ahol az *Arduino* támogatja a megszakításokat. A megszakításokat később,
a \ref{ch:hw_interrupts}. alfejezetben fogom ismertetni.  

### Példák kész eszközökre
Csak felsorolásszerűen bemutatnám képekben annak a pár eszköznek a felépítését, amelyeket korábban
említettem, mint a rendszer által támogatottak. A képek nem feltétlen fedik le a valós, összerakott
eszközök felépítését. Ennek oka annyi, hogy másképp nem feltétlen lennének jól értelmezhetőek az ábrák.
Például bizonyos esetekben az *nRF24L01+* kommunikációs modul nem megfelelő tápfeszültséget kap a képeken,
viszont ez nem változtat semmit a valódi bekötési ábra számunkra is lényeges részeihez képest.

- Hő- és páratartalommérő szenzor
\begin{center}
  \includegraphics[width=0.7\textwidth]{source/figures/05_dht22_sensor}
\captionof{figure}{Hő- és páratartalommérő szenzor}
\end{center}
\newpage

- Mozgásérzékelő szenzor
\begin{center}
  \includegraphics[width=0.7\textwidth]{source/figures/05_pir_sensor}
\captionof{figure}{Mozgásérzékelő szenzor}
\end{center}

- Elektromos relé aktor
\begin{center}
  \includegraphics[width=0.7\textwidth]{source/figures/05_relay_actor}
\captionof{figure}{Elektromos relé aktor}
\end{center}

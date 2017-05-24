## Firmware működés

### Megszakítások
\label{ch:hw_interrupts}
A fejlesztés közben szembesültem azzal, hogy különbséget kell tegyek szenzorok közt is a kívánt
működésüket tekintve. Pontosabban fogalmazva abban különbözhet egyik szenzor a másiktól, hogy mikor
kell adatot küldjön a központi szervernek. Gondoljunk bele abba, hogy egy hőmérő szenzor elég, ha
fix időközönként elküldi az aktuális állapotát. Ha viszont tegyük fel egy mozgásérzékelő szenzorról
beszélünk, akkor azt szeretnénk, hogy ha mozgás van a szobában, akkor egyből elküldje az új adatot
a szenzor. Ugyan ez az helyzet például egy ajtózárt irányító aktor esetében is. Azonnal szeretnék
értesülni arról, hogy kinyitották a bejárati ajtót, nem pedig akkor, ha éppen úgy esik a fix időközönkénti
állapot küldés. A fix időközönként közvetítő szenzort időzített szenzoroknak neveztem el, míg azokat,
amik állapotváltozást igényelnek, hogy elküldjék az adatot, reaktív szenzoroknak.

Az eszközök felépítésének bemutatásánál említettem a megszakítások fogalmát és azt, hogy szükségünk
lesz rájuk. A megszakítások olyan külső vagy belső jelek, amelyek a feldolgozó egység számára szólnak,
hogy azonnali figyelmet igényel valamilyen hardver vagy szoftver. Az *Arduinok* esetében egy ilyen
megszakítás lehet egy bemeneti jel változás vagy esetleg egy belső időzítő lejárta. Másik fontos
tulajdonsága a megszakításoknak az *Arduinoknál*, hogy egy megszakítás hatására az eszköz képes felébredni
alvó állapotból. [@Interrupt; @AtmegaInterrupt] Így viszont meg is oldottuk a szenzorok közti különbség problémáját, hiszen, ha az
*Arduino* normál állapotban van, akkor leolvassuk az aktuális állapotát az eszköznek, elküldjük a központi
szerverhez az adatot, majd alvó módba lépünk. Ahhoz, hogy az időzett szenzorok fix időközönként közvetítsenek
a belső időzítőt kell használni az eszköz felébresztésére, a reaktív szenzorok esetében egy olyan
bemenetre kell kötni az érzékelő modult, ahol az *Arduino* támogatja a megszakításokat. Így elértük azt,
hogy ha például valamilyen mozgást érzékelünk az eszköz egyből felébred és elküldi az új állapotot.

Az aktorok esete szintén eltér az eddigiektől, mivel az aktorok nem csak üzennek a központi szervernek, hanem
fogadnak is onnan érkező parancsokat és az eszközöknek normál állapotban kell működniük ahhoz, hogy a
parancsokat fel tudják dolgozni. Ennek ellenére szeretnénk, ha az aktorok is tudnának alvó állapotban lenni
energiatakarékosság miatt. Szerencsére az *nRF24L01+* képes nekünk megszakításokat generálni, ha valamilyen
üzenet érkezett az eszköz számára. Ez pont tökéletes számunkra, hiszen csak akkor fogjuk így felkelteni
az *Arduinot* alvó állapotból, ha fel kell dolgozni a központtól érkezett parancsot.

### Alacsony fogyasztás
Mindegyik eszköznek, működése folyamán, van olyan időköze, amikor nem kell semmilyen feladatot elvégezzen.
Szenzoroknál például két állapotleolvasás közt vagy aktorok esetén két parancs feldolgozás közt.
Felmerül a kérdés, hogy amíg nincs szükségünk az eszközök teljes számítási kapacitására, addig tudnánk-e
valamilyen módon energiát megtakarítani annak érdekében, hogy kevesebbszer kelljen elemet cserélni
bennük. Korábban már említettem, hogy az *Arduinok* képesek *alvó módba* lépni és ezzel elérjük azt,
hogy spóroljon az energiával. Ha például a legalacsonyabb energiafelhasználású módba tesszük az eszközt
akár 40-szer kevesebb áramot fogyaszthat az *Arduinon* elhelyezett *AVR* mikrokontroller. Ez viszont jelentős
üzemidő javulást jelenthet, így akár pár napnyi működés helyett hónapok is lehetnek. Lehetőségünk van
még arra is, hogy kikapcsoljuk az AVR mikrokontroller nem használt részeit. Kikapcsolhatjuk például
az analóg-digitális átalakítót vagy bizonyos belső időzítőket. Alkalmazhatjuk például úgy ezt a módszert,
hogy mielőtt alvó módba lép az eszköz kikapcsoljuk minden részét a mikrokontrollernek és majd csak akkor
kapcsoljuk vissza azokat, ha felébred. Illetve azokat a részeket véglegesen kikapcsolhatjuk, amik olyan
funkciókért felelnek, amiket sose használunk.

Tovább tudjuk faragni a fogyasztást bizonyos hardveres módosításokkal. Minden *Arduinon* található egy
LED, ami folyamatosan ég amíg az eszköz áramot kap. Ha ezt a LED-et eltávolítjuk szintén jelentős
mennyiségű energiát megspórolhatunk és igazából szükségünk sincs arra a jelző fényre. Található még
az *Arduinokon* egy feszültség szabályzó integrált áramkör, ami általában nem a legjobb hatékonysággal
rendelkezik. Természetesen ennek az az oka, hogy ahol csak lehet az olcsóbb alkatrészeket választják
a gyártási költségek minimalizálása miatt. A feszültségszabályzó cseréje egy hatékonyabbra szintén
segíthet valamennyit. Akár a teljes eltávolítása is megoldás lehet még, csak sajnos a mi esetünkben
szükség van rá, így ez nem járható út. [@ArduinoPower]

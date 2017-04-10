## Firmware működés
\label{ch:hw_firmware_operation}

### Szenzor típusok
A fejlesztés közben szembesültem azzal, hogy különbséget kell tegyek szenzorok közt is a kívánt
működésüket tekintve. Pontosabban fogalmazva abban különbözhet egyik szenzor a másiktól, hogy mikor
kell adatot küldjön a központi szervernek. Gondoljunk bele abba, hogy egy hőmérő szenzor elég, ha
fix időközönként elküldi az aktuális állapotát. Ha viszont tegyük fel egy mozgásérzékelő szenzorról
beszélünk, akkor azt szeretnénk, hogy ha mozgás van a szobában, akkor egyből elküldje az új adatot
a szenzor. Ugyan ez az helyzet például egy ajtózárt irányító aktor esetében is. Azonnal szeretnék
értesülni arról, hogy kinyitották a bejárati ajtót, nem pedig akkor, ha éppen úgy esik a fix időközönkénti
állapot küldés. A fix időközönként közvetítő szenzort időzített szenzoroknak neveztem el, míg a azokat
amik állapotváltozást igényelnek, hogy elküldjék az adatot, reaktív szenzoroknak.

Az eszközök felépítésének bemutatásánál említettem a megszakítások fogalmát és azt, hogy szükségünk
lesz rájuk. A megszakítások olyan külső vagy belső jelek, amelyek a feldolgozó egység számára szólnak,
hogy azonnali figyelmet igényel valamilyen hardver vagy szoftver. Az Arduino-k esetében egy ilyen
megszakítás lehet egy bemeneti jel változás vagy esetleg egy belső időzítő lejárta. Másik fontos
tulajdonsága a megszakításoknak az Arduino-knál, hogy egy megszakítás hatására az eszköz képes felébredni
alvó állapotból. Így viszont meg is oldottuk a szenzorok közti különbség problémáját, hiszen ha az
Arduino normál állapotban van, akkor leolvassuk az aktuális állapotát az eszköznek, elküldjük a központi
szerverhez az adatot, majd alvó módba lépünk. Ahhoz hogy az időzett szenzorok fix időközönként közvetítsenek
a belső időzítőt kell használni az eszköz felébresztésére, a reaktív szenzorok esetében egy olyan
bemenetre kell kötni az érzékelő modult, ahol az Arduino támogatja a megszakításokat. Így elértük azt,
hogy ha például valamilyen mozgást érzékelünk az eszköz egyből felébred és elküldi az új állapotot.

Itt egy picit kitérnék az aktorok működésére is, igaz nem szorosan nem a szenzor típusokhoz kapcsolódik,
viszont a megszakításokhoz annál inkább. Mivel az aktorok nem csak üzennek a központi szervernek, hanem
fogadnak is onnan érkező parancsokat, az eszköznek normál állapotában kell működnie ahhoz, hogy a
parancsot fel tudja dolgozni. Ennek ellenére szeretnénk ha az aktorok is tudnának alvó állapotban lenni
energiatakarékosság miatt. Szerencsére az nRF24L01+ képes nekünk megszakításokat generálni, ha valamilyen
üzenet érkezett az eszköz számára. Ez pont tökéletes számunkra, hiszen csak akkor fogjuk így felkelteni
az Arduino-t alvó állapotból, ha fel kell dolgozni a központól érkezett parancsot.

### Alvó mód

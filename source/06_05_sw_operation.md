## Szoftver működés
A hálózati réteg működése könnyen átlátható és annál is egyszerűbben implementálható. A fejezet korábbi
részei alapján magától értetődő, hogy a program egyszerre kell kezeljen egy vezeték nélküli kapcsolatot
az eszközökkel és egy TCP kapcsolatot a központi szerverrel. Ha ezt a két feladatot egy végrehajtási szálon
szeretnénk elvégezni nagy eséllyel gondokba ütköznénk, aminek az lenne az oka, hogy az egyik kapcsolat
eseményei nem lennének kellően lekezelve, amíg a másik kapcsolat kapja a figyelmet. Példa eset, az
egyik kapcsolaton keresztül épp olvasás történik és a másikon üzenet érkezik. Viszont ez még nem azt
jelenti, hogy a feladat megcsinálhatatlan egy futási szálon. Bizonyára lehetséges, de én úgy döntöttem,
hogy teljesítményi okokból inkább kettő szálon kezelem a kapcsolatokat. A futtató hardverünk nem fog
gátakat szabni e szempontból, tehát nyugodtan használhatunk szálkezelést.

\begin{center}
  \includegraphics[width=\textwidth]{source/figures/06_network_sw.png}
  \captionof{figure}{Hálózati réteg szoftver működési ábra}
\end{center}

Van akkor két végrehajtási szálunk, egy a TCP kapcsolatnak, egy a vezeték nélküli kapcsolatnak. Ha akármelyiken
valamilyen üzenet érkezik, azt át kell alakítani a másik fél üzenetformájára és azon az oldalon tovább
küldeni. Figyelnünk kell viszont, mivel, ha az egyik szál közvetlen bele szól a másik oldal kapcsolatába,
könnyen lehet, hogy elrontja azt. Ami azt illeti elsőnek elkövettem ezt a hibát és nem is
működött túl megbízhatóan a rendszer. Megoldani a problémát nem volt nehéz, hiszen elég volt mindkét
szálon létrehozni egy listát, amelyben ideiglenesen el lesznek tárolva azok az üzenetek, amiket majd
el kell küldeni. Vagyis, ha az egyik szálon érkezik egy üzenet, átalakítja azt, majd belerakja a másik
szál listájába. Mikor a másik szál oda kerül, hogy üzeneteket küld, akkor kiveszi a listából a várakozó
üzeneteket és kézbesíti. A listához való hozzáadás mindig lezárja a hozzáférést a másik oldal számára,
így az csak akkor tudja olvasni és elküldeni az üzeneteket, ha senki nem ad hozzá éppen semmit. Ezzel
így teljes mértékben megoldottuk a szálkezelést és minden üzenet szinte azonnal kézbesítésre kerül.

### Programkönyvtárak
Említésre került a fejezet elején, hogy a *Boost* könyvtárcsomag egy részét használtam a TCP kapcsolat
irányítására. A *Boost.Asio* programkönyvtár adatok aszinkron feldolgozására van fejlesztve és
többek között képes `socket`-ek kezelésére is. Mivel a *Boost* könyvtárcsomag, olyan nagy jelentősséggel
bír a C++ fejlesztésben, hogy szinte egy kiterjesztett Standard könyvtárként lehet tekinteni rá,
egyértelmű választás volt, mint TCP kapcsolatkezelő könyvtár. Jelentős könnyebbséget hoz a C-beli
`socket` programozáshoz képest. Ahhoz, hogy teljesítse a feladatát a TCP kapcsolat kezelő végrehajtási
szálunk elsőnek azt kell megnéznie, hogy van-e beérkező üzenetünk a központi szervertől. Ha van, akkor
addig olvassuk a bejövő adatot, amíg nem találkozunk egy sorvége karakterrel, mert így jelöltem az
üzenet végét. Ehhez a `boost::asio::read_until()` függvényt használtam. A beérkezett
üzenetet átfuttatva az átalakító logikánkon belerakjuk a vezeték nélküli kapcsolatot kezelő szál listájába.
Ha mindez meg volt vagy esetleg nem is volt beérkező üzenet, akkor megpróbáljuk elérni a saját elküldendő
üzenet listánkat. Ennek sikere attól függ, hogy éppen történik-e módosítása a listának. Sikeres elérés
esetén végig megyünk a listán és egyenként elküldünk minden üzenetet. A küldést a
`boost::asio::ip::tcp::socket.write_some()` függvény segítségével lehet megvalósítani. [@BoostAsioDocumentation]

A fenti logikához képest csak egy kicsivel van több dolga a vezeték nélküli kapcsolatot kezelő végrehajtási
szálnak. Az előző alfejezetekből tudhatjuk már, hogy az eszközökkel való kommunikáció az *RF24Mesh*
programkönyvtárnak köszönhető. A különbség a másik oldalhoz képest annyi, hogy az *RF24Mesh* esetén
az érkező üzenetek beolvasását megbonyolítja az üzenetek cimkézése. Már említettem, hogy ezeket a címkéket
arra használjuk, hogy az eszközök kategóriáját (szenzor vagy aktor) meg tudjuk mondani. A beolvasás
azzal kell kezdődjön, hogy megnézzük, milyen címkével rendelkezik az üzenet. Ez úgy történik, hogy az
`RF24Network::peek()` [^rf24_peek] függvény egy `RF24NetworkHeader` objektumba belerakja az üzenet fejléc részét.
A fejléc rész olyan információkat tartalmaz, mint a küldő és a címzett hálózati címe, egy üzenet azonosító
és az üzenet címke. Ezután a `RF24Network::read()` [^rf24_read] függvény a fejléc segítségével az teljes üzenetet
belerakja egy általunk megadott változóba. Az olvasás befejezése után minden a TCP kapcsolat
kezeléséhez hasonlóan zajlik, csak az üzenetküldéshez a `RF24Mesh::write()` [^rf24_write] függvényt kell használni. [@RF24NetworkDocumentation; @RF24MeshDocumentation]

# Összefoglalás
A dolgozatban egy otthon automatizációs rendszer felépítését és a fejlesztési döntéseket ismertettem.
A célja a munkámnak az volt, hogy a rendszerhez kapcsolódó érzékelőktől kezdve, a központi adatgyűjtő
és döntéshozó rendszerig minden általam legyen megtervezve és implementálva. A rendszer képes a felhasználó által megadott
szabályok mentén irányítani egy teljes háztartást. Ehhez a házban és környékén elhelyezett érzékelő
eszközök által szolgáltatott mérési adatokat használja fel. Az adatokat nem csak döntéshozási célokra
használja a rendszer, hanem el is menti, hogy a felhasználó számára esetleges statisztikákkal tudjon
szolgálni. Mindezt a felhasználó egy webes felületen keresztül tudja irányítani.

Elsőnek áttekintés céljából ismertettem nagy vonalakban a teljes rendszert és annak három nagyobb
alrétegét. A három réteg az érzékelőkből és más funkciókkal bíró eszközökből, egy hálózati
összekötő rétegből és egy központi szerverből áll. Az eszközök áttekintésénél megtudtuk, hogy mik a *szenzorok*
és *aktorok*, illetve azt is, hogy hogyan kell működjenek. A hálózati réteg áttekintésében megindokoltam,
hogy miért is van szükség egy ilyen közvetítő rétegre és azt is meg tudhattuk, hogy hardverközelségi
igények miatt ez a réteg C++-ban lett fejlesztve. A központi szerver áttekintése során felvázoltam
a rendszer lényegi részétől elvárt funkciókat, mint például a statisztika megjelenítés vagy az eszközök
élő irányítása.

Ezek után egyenként minden rétegről bővebben is esett szó, ahol már a fejlesztés során felmerülő
döntéshelyzeteket is ismertettem. Természetesen minden döntés mellé indoklás is került, hogy miért
azt az adott választást láttam legoptimálisabbnak a fejlesztés alatt.

Az eszközökről szóló fejezet egyik legfontosabb része a megfelelő hardverek kiválasztásáról szólt.
Megindokoltam, hogy miért az *Arduino* platformot választottam az eszközök alapjaként és azt is, hogy
miért az *nRF24L01+* alapú vezeték nélküli adó-vevő modulok kaptak szerepet. Ezek mellett azt is bemutattam,
hogy hogyan oldottam meg az eszközökön futó szoftver működését hardveres megszakítások és az *AVR*
mikrokontrollerek alvó állapotának segítségével.

Ahogy, az eszközök részletes bemutatása meg volt, a hálózati réteg következett. Ebben a fejezetben
végig vettem azt, hogy miként épül fel egy *mesh* topológiájú hálózat és milyen előnyei vannak. Erre azért
volt szükség, mert az eszközeink és a központi szervert képviselő csomópont egy mesh hálózatot alkotnak.
Itt a központi csomópont egy átjáróként szolgál az eszközök és a szerver között. Ehhez egy külső
programkönyvtárt használtam, ami kifejezetten az általunk választott *nRF24L01+* vezeték nélküli modulhoz
fejlesztettek. Bemutattam, hogy miként működik a *TCP* kapcsolat és hogyan épülnek fel a
JSON üzenetek a központi szerver és a hálózati réteg között, illetve ugyanúgy a hálózati réteg és az
eszközök közötti kapcsolat is, ami már vezeték nélküli összeköttetésen történik. Részletesen belementünk
abba, hogy miért is volt szükség többszálúságra, ahhoz, hogy egyszerre mindkét kapcsolatot kezeljük,
illetve a kapcsolatot fenntartó technológiákat is röviden átnéztük. Különlegessége még a hálózati rétegnek,
hogy egy *Raspberry Pi*-n fut az említett hardverközeli igények miatt.

A dolgozatom végére került a rendszer legfontosabb részének ismertetése. A központi szerver végzi el az összes valódi funkciót,
értve ez alatt az adatok megfelelő elmentését, megjelenítését vagy a felhasználói szabályok betartatását.
A folyamatosan beérkező adatok kezelése számomra újfajta kihívásokat jelentettek, aminek legyőzésére
egy *NoSQL*, dokumentumorientált adatbázist a *MongoDB*-t választottam. A *MongoDB* segítségével könnyedén
tudtam olyan tárolási sémát találni, amelynek köszönhetően megfelelő gyorsasággal tudok lekérdezéseket
futtatni az adatokon és az adatok mentése se igényel rendkívül nagy tárhelyet. Azt is bemutattam, hogy
a *flow rendszernek* elnevezett szabály végrehajtó rendszerem hogyan működik és hogy miként tudtam
elkerülni a bejövő adatok folyamatos figyelését úgy, hogy mégis azonnal végrehajtódjon egy adott szabály
következménye, ha annak előfeltétele teljesül.

Összességében meg tudtam valósítani azokat a funkcionalitásokat, amiket kitűztem magamnak célként, viszont
rengeteg irányba lehetne még tovább fejleszteni a rendszert. Nem véletlen, hogy napjaink legnagyobb
vállalatai is az *IoT*, illetve az otthon automatizálás témájával foglalkoznak, hiszen még nagyon sok
fejlődésre van lehetősége az informatika ezen ágának. Magam is tervezem tovább bővíteni még a rendszer
tudását szabadidőmben. Rengeteg eszköztípus van, amit még meg lehetne valósítani és egyelőre nem volt
még hozzá szerencsém, de a más rendszerekkel való összekötés lehetősége is nagyon ígéretes lehet.

# Hálózati réteg
Az áttekintő fejezeteben említésre került, hogy miért van szükség a hálózati rétegre. Ez a mondhatni
közvetítő réteg annak ellenére, hogy nem végez el sok funkcionalitást, mégis sok munkát igényelt
a fejlesztés során. A nehézség nem feltétlen csak a szoftver előállításából adódott, hanem a megfelelő
hardverek megtalálásából és azok összehangolásából. Ahhoz, hogy fogadni tudjuk az eszközök üzeneteit
ugyanolyan *nRF24L01+* adó-vevő modult kell használni a hálózati rétegben is, mint az eszközökben.
Mivel az ahhoz való kapcsolódás és kommunikáció alacsony szintű hardverhozzáférést igényel és SPI buszt
egy hétköznapi számítógép nem megfelelő számunkra. Léteznek viszont úgynevezett *single-board* számítógépek,
amelyek pont az ilyen esetekre lettek kitalálva. Egy-egy ilyen számítógépen található egy aránylag nagy
teljesítményű processzor, pár megszokott ki- és bemenet perfirériák számára (pl.: USB portok) és ami a
legfontosabb számunkra GPIO pinek vagy másnéven *általános-célú ki- és bemenetek*. A GPIO-n keresztül
lesz lehetőségünk az adó-vevő modullal kommunkálni. Az én választásom a *Raspberry Pi 3* apró
számítógépre esett.

Szó lesz még a fentiek mellett a hálózat felépítéséről, hogy miként is kapcsolódnak és kapnak címet
az eszközök. Habár mindezt az *nRF24L01+* modulhoz fejlesztett külső programkönyvtár végzi, mégis
jelentős részét képezi a hálózati réteg működésének. A hálózat maga *mesh alapú*, viszont
ez csak a programkönyvtárnak köszönhető, ezért nevezném inkább *pszeudo-mesh* hálózatnak, amit meg is
fogok magyarázni miért. Ha már tudjuk, hogyan kapcsolódunk a hálózathoz, akkor már csak az üzenetek
küldése hiányzik. Ismertetni fogom, hogy milyen felépítésűek az állapot jelentő vagy állapot beállító
üzenetek a szerver és hálózati réteg között, illetve a hálózati réteg és az eszközök között.

Ezek után röviden bemutatom, hogy miként is épül fel a hálózati réteg szoftvere, hogy hol voltak
gondjaim a fejlesztés során. Illetve említésre kerül még a C++ *Boost* könyvtárcsomag, mivel rendkívül
megkönnyítette számomra a TCP kapcsolat kezelését a központi szerverrel.

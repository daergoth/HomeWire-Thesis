# Központi rendszer
Elértünk a teljes rendszer legfontosabb és legbonyolultabb részéhez, mivel, ahogy az gondolható
a központi szerver kezel mindent. Itt történik az összes beérkező adat mentése, a
felhasználói felületek biztosítása, az adatok alapján történő döntéshozás és a statisztikák elkészítése.

Az egyik lényegesebb döntést az adatbázis tekintetében kellett meghozni, hiszen a rendkívül nagy
mennyiségű adatot el kell mentenünk. A kihívást az jelentette, hogy miként lehet kevés tárhely
használatával eltárolni a látszólag kevés rendszerességet tartalmazó beérkező állapotjelentéseket.
Tehát lesz szó arról, hogy milyen adatbáziskezelő rendszerek jöhettek szóba és melyiknek milyen előnyei,
illetve hátrányai lettek volna a fejlesztésnél. Mivel már említettem a \ref{ch:software_overview}.
alfejezetben, hogy a *MongoDB*-t választottam az adatok perzisztens tárolására, ezért kitérek majd
arra is, hogy milyen séma alapján lesznek mégis rendszerezve az eszköz adatok és hogy miért jó az
említett adatbáziskezelő rendszer az ilyen szituációkban.

Ahhoz, hogy a központi szerver létrejöhessen több keretrendszerre is szükségem volt. Így például a
szerver alapját a *Spring* alkalmazásfejlesztési keretrendszer előre felkonfigurált verziója a
*Spring Boot* adja. Nagy előnye a *Spring Boot*-nak, hogy nagyon gyorsan el lehet kezdeni fejleszteni
az alkalmazás lényegi részét és nem kell sok időt tölteni a konfigurációval vagy éppen úgynevezett
boilerplate kód írásával. A felhasználói felület megvalósításához a *Vaadin* webes UI keretrendszer
volt segítségemre. Használata előre megírt komponensek felhasználásával felépített felületek tervezéséből
áll. A *Vaadin* mellett szólt még, hogy könnyedén használható *Springes* webalkalmazások készítéséhez,
mivel a készítők figyelmet fordítottak a kompatibilitásra és külön támogatják *Spring Boot* alapú
webalkalmazások fejlesztését.

Nagy hangvételű részt kap a *Flow-rendszer*, hiszen csak említve voltak a fogalmak hozzá kapcsolódóan,
viszont rendesen megmagyarázva nem. A *Flow-rendszer* felelős a felhasználó által megadott szabályok
kezeléséért és betartásáért. A működés gyors menetének megtartásáért kellett alkalmazni néhány megoldást,
amik akár gyorsítótáraknak is tekinthetőek, amelyekre azért volt szükség, hogy ne kelljen túl nagy
terhelést rakni az adatbázisra feleslegesen, még akkor se, ha az adatbázis sokkal nagyobb terhelést is
kibírna. Szintén lesz szó arról, hogy mi történik akkor, ha beérkezik új adat a rendszerbe, milyen
folyamatok indulnak el.

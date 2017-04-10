## Központi szerver
A legnagyobb funkcionalitást természetesen a központi szerver végzi, hiszen ott
lesznek feldolgozva az eszközöktől érkező állapotjelentések és a szerver küldhet
állapot beállítási parancsokat is.

Mint említve volt már a központi szerver TCP kapcsolaton keresztül kommunikál a
hálózati réteggel. Az onnan érkező üzeneteket a rendszer például a megfelelő
formában elmenti az adatbázisban vagy az üzenet hatására elindulhat

\noindent  
A központi szervert is tovább tudjuk bontani több darabra funkcionalitása alapján:

- Flow-rendszer
- Statisztika
- Kezelőpanel

### Flow-rendszer
A *Flow-rendszer* onnan kapta a nevét, hogy a felhasználó által megadható szabályokat
"flow"-knak neveztem el. A név a beérkező adatok folyamából és azoknak folyamatos
feldolgozásából jön. Minden *flow*-nak van feltétele és hatása. Ez a hatás akkor fog
bekövetkezni, ha a feltétel teljesül. Részletesebben a \ref{ch:flow_system}.
alfejezetben lesz szó a *flow*-król.

A *Flow-rendszer* biztosítja az új szabályok létrehozásának, régebbiek módosításának
lehetőségét és elmenti az így létrejött változásokat. Másik lényeges feladata ennek
a rendszernek még, hogy be is tartassa ezeket a szabályokat. A szabályok betartásáért
felelős mechanizmust szintén a \ref{ch:flow_system}. alfejezetben fogom taglalni.
Röviden összefoglalva, ha új adat érkezik, a rendszer megnézi melyik szabályokat
érintheti az esemény és kiértékeli azokat. Tulajdonképpen ez a rendszer felelős
a szerver legfontosabb és legbonyolultabb funkcióiért.

### Statisztika
A rendszer használata során természetesen szeretnék néha visszanézni régebbi
méréseket. Lehet ennek sok oka, például szeretnénk megnézni mennyivel volt hidegebb
az előző hónapban, vagy leellenőrizhetjük mennyit volt bekapcsolva a fűtés. Tehát
elég könnyen adja magát az elvárás, hogy tudjunk statisztikákat nézni a múltbeli adatokról.

Ehhez viszont el kell tárolni minden adatot, hogy később is tudjunk valamit mutatni
a felhasználónak. Az adatbázis-rendszer kiválasztásánál ez volt az egyik legnagyobb
szempont. A választás a  **MongoDB**-re jutott, viszont a döntés mögött álló érveket majd
a \ref{ch:database}. alfejezetben fogom ismertetni. Mindemellett ahhoz, hogy az a
sok adat, amit elmentenénk ne foglaljon sok tárhelyet, nem külön mentődnek el, ahogy
beérkeznek a szerverhez, hanem percenként átlagolva kerül az adatbázisba. Ezzel
igaz, hogy egy kicsi pontosságot vesztünk, mikor visszanézzük a statisztikákat,
viszont ha jobban belegondolunk nincs is szükségünk arra, hogy például 10 másodpercenkénti
részekre lebontva lássuk a múlt heti hőmérsékletet. Ami azt illeti még a percenkénti
lebontás is túl aprónak tűnik bizonyos helyzetekben, de természetesen a lementett
adatok segítségével elő tudunk állítani akár napi vagy havi átlagokat is.
Mindezt grafikonokon és idő-soros diagrammokon mutatva, a felhasználó könnyedén tud
következtetések levonni magának.

### Kezelőpanel
A kezelőpanel szolgáltatja a felhasználó számára a valós idejű adatokat. Itt jelennek
meg a beérkező állapotjelentések az eszközöktől legelőször, illetve aktorok esetében
itt lesz lehetőségünk kézzel átállítani az eszköz állapotát, vagyis állapot beállító
parancsokat küldeni.

Lehetővé válik így, hogy a felhasználó akár a munkahelyéről is megnézhesse hogy
áll az otthona, esetleg nem hagyta-e nyitva az ajtót reggel. Ez a pár másopercenként
frissülő kezelő felület minden eszköztípushoz külön mini megjelenítő modulokat
rendel, aminek köszönhetően akár pár pillanat alatt megtalálhatjuk a számunkra
fontos információkat.

## Eszközök
Mint már említve volt, ide tartoznak azok a hardverközeli rendszerek, amik vagy
adatgyűjtő szerepet töltenek be, vagy valamilyen funkcionalitást képesek végrehajtani.
Lehet egy ilyen eszköz például egy hőmérő, légnyomásmérő, földnedvességmérő vagy
éppen egy relé.

\noindent
Ezek az eszközök két csoportba sorolhatók:

- Érzékelők, másnéven *szenzorok* vagy
- Cselekvők, másnéven *aktorok* [^actor_naming]

[^actor_naming]: Ezt az elnevezést magam adtam, mivel nem találkoztam kutatásaim
során más megfelelő elnevezéssel.

Valójában viszont ez a két csoport nem diszjunkt és később kiderül
miért is nem, ha látjuk pontosan melyik mit jelent. Közös bennük, hogy mindegyikben
van egy mikrokontroller és egy kommunikációért felelős modul. E mellett mindegyik
eszköz tartalmaz még egy specifikus modult is, ami meghatározza, hogy mit is csinál
az adott eszköz (pl.: hőmérő esetén egy hőmérséklet- és páratartalom érzékelő),
illetve, hogy melyik csoportba tartozik.

### Szenzorok
Magától értetődően a szenzorok felelősek az adatok szolgáltatásáért. Működését
tekintve a mikrokontroller vezérli az adatgyűjtés és küldés folyamatát. Az idő nagy részében
alvó állapotban van energiatakarékossági szempontok miatt, viszont fix időközönként
felébred. Mikor felébred lekéri a saját specifikus moduljától a jelenlegi mért
állapotot, majd azt megpróbálja közvetíteni a központba. Ezt addig próbálja, amíg
nem sikerül, vagyis a központ vissza nem küld egy automatikus jelzést, hogy megérkezett
az adat. Szenzorok közé sorolhatók például a sok féle mérőeszközök vagy akár egy
mozgásérzékelő is.

### Aktorok
Az aktorok a rendszer olyan részei, amik segítségével a rendszer képes változtatásokat
végbe vinni a fizikai világban. Hasonló a helyzet a szenzorok esetéhez, hiszen
ugyanúgy a mikrokontroller kezel mindent. Ugyanúgy alvó állapotban van az eszköz
az idő nagy részében, viszont jelen esetben nem fix időközönként ébred fel, hanem
amikor üzenetet kap a központtól. Ezek az üzenetek állapotbeállítási parancsok,
vagyis utasítják az eszközt, hogy állítson át vagy cselekedjen valamit. Ha megtörtént
a parancs feldolgozása, az eszköz egyből elküldi a központi rendszernek az új állapotát,
állapotjelentés formájában, akárcsak a szenzorok, majd megint alvó állapotba kerül
a következő parancsig. Aktor lehet például egy elektromos relé vagy egy termosztát is.

Tehát láthatjuk, hogy miért is nem szétválasztható a két csoport. Az aktorokra
tekinthetünk olyan szenzorokként, amik képesek még egyéb funkcionalitást is végezni.
Így tekintve a szenzorok csoportja tágabb és magába foglalja az aktorok csoportját.

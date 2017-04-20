## Flow rendszer
\label{ch:flow_system}
A *Flow-rendszer* legfontosabb elemei a már említett *flowk*, melyekre gondolhatunk úgy akár egy-egy
szabályra. Ezek a szabályok két listából állnak, egy feltétel listából és egy hatás listából.
A feltétel lista olyan kikötéseket tartalmaz, amik ha teljesülnek, akkor végrehajtódik a *flow*
hatás listája. A hatás lista pedig tekinthető egy akció sorozatnak, amit szekvenciálisan végigjárva
megkapjuk, hogy mi kellett történjen a *flow* teljesülésének időpontjában.

A feltétel lista részei lehetnek például egyes eszközök állapotára vonatkozó megszorítások
(pl.: 20°C-nál nagyobb a hőmérsékletet mutat a nappaliban elhelyezett hőmérő) vagy
a felhasználó vagy külső rendszer által indított kérés az alkalmazáshoz
(pl.: gomb nyomás a kezelőfelületen vagy HTTP kérés egy bizonyos címen), illetve
egyéb rendszer állapot feltétel (pl.: időponthoz kötődő feltétel).
Fontos megjegyezni, hogy a feltétel lista elemei között logikai ÉS kapcsolat áll, tehát mindnek
teljesülnie kell, ahhoz, hogy a *flow* hatása végrehajtódjon.

A hatás lista elemei között szerepelhetnek eszközöknek célzott állapot beállító parancs küldés
(pl.: kapcsoljon fel egy lámpa), más rendszerhez történő kérés (pl.: HTTP kérés) és egyéb segéd akció
(pl.: késleltetés). Ezeknek a hatás elemeknek egymás után történő végrehajtása adja az adott flow hatását.

Azonban a fejlesztési idő végessége miatt nem jutott idő az összes feltétel és hatás típus implementálására.
A feltételek típusok közül az eszköz állapot megszorítások és a hatás típusok közül az eszköz állapot
beállítások lettek megvalósítva. Szerencsére ezzel az egy-egy feltétel és hatás típussal is teljes
mértékben működő képes a rendszer. Azzal, hogy például nincs lehetőség HTTP kérések indítására és
fogadására csak annyit vesztettünk, hogy egyelőre nem tudunk külső rendszerekkel együtt dolgozni.

A rendszer célja az, hogy a *flowk* segítségével a felhasználó szabályokat/feladatokat tud leírni,
amelyeket a rendszer majd végrehajt és így lehetséges lesz bizonyos házkörüli feladatok automatizálása.
Azzal, hogy pár egyszerű *flowt* létrehozunk megkönnyíthetjük a mindennapjainkat. Elsőre mindig csak
kényelmi funkciókra gondolnánk, amit elvégeztethetünk a rendszerrel (pl.: automatikusan lekapcsoló
lámpák), viszont megfelelő mennyiségű idő feláldozása után jelentős pénzösszeget is megspórolhatunk
(pl.: időzített fűtésrendszer, ami csak akkor fűt ha otthon vagyunk).

Pár példa a rendszer használatára:

* ha adott szobában nincs érzékelt mozgás, akkor lekapcsolódik a villany
* ha több hőmérő is alacsony értéket mutat, akkor automatikusan fentebb megy a fűtés
* ha egy virág földje kiszáradna, akkor víz engedődik a virág alá
* ha reggeli időpont van, akkor beindul a kávéfőző
* ha a levegő szén-monoxid tartalma átlép egy határt, akkor elindul egy jelző berendezés
* ha besötétedik és van mozgás, akkor a sötétítők leengednek és felkapcsol egy villany

### Flow végrehajtás
A *Flow-rendszer* működésének talán legizgalmasabb része a folyamatos ellenőrzés, hogy melyik *flow*
lett aktív egy éppen beérkezett üzenet hatására. Az első megoldás, ami eszünkbe juthat, hogy egy végtelen
ciklusban futna egy ellenőrző algoritmus, de tudjuk, hogy nem ez a legelegánsabb megoldás. Jelen esetben
viszont csak állapot jelentés üzenetek formájában érheti a rendszert olyan külső hatás, ami miatt le
kell ellenőrizni a *flowkat*. Ezért ahelyett, hogy lenne egy folyamatosan futó ellenőrző algoritmusunk
sokkal hatékonyabb megoldást jelent az, ha az állapot jelentés üzenetek beérkezése váltaná ki az
ellenőrzés egyszeri lefutását. Ezzel a mechanikával nem fektetünk folytonosan magas terhelést a futtató
hardverre és adatbázisra.

Azzal, hogy csak beérkező adat esetén indítunk ellenőrzést tovább egyszerűsíthetjük a helyzetünket.
Ha egy üzenet érkezik természetesen azt is tudjuk, hogy melyik eszköz küldte. Ezen plusz információ
segítségével akár célirányosan tudjuk ellenőrizni azokat a *flowkat*, amikben "szerepel" a küldő
eszköz. Egy eszköz akkor "szerepel" egy *flowban*, ha annak a *flownak* a feltétel listája tartalmaz,
olyan megszorítást, ami az adott eszköz állapotára vonatkozik. Tehát egy beérkezett állapot jelentés
hatására csak azokat a *flowkat* kell megnézni, hogy aktívak lettek-e, amelyeknek a feltétel listájában
van hivatkozás az üzenetet küldő eszközre. Ezen egyszerűsítés hatására jelentősen kevesebb *flowt*
kell végignézzünk, hogy végre kell-e hajtani.  

Ha tovább szeretnénk optimalizálni az ellenőrzés folyamatát, még van rá lehetőségünk. Azzal, hogy
célirányosan kérjük le a kiértékelendő *flowkat* az adatbázistól jelentős javulást értünk el az adatbázis
terhelés tekintetében, de még mindig sok lehet, ha akár másodpercenként érkezik új adat. Valószínűleg
ezen szintű terhelést nevetve kibírna a választott adatbáziskezelő rendszerünk, viszont mindig jobb
előre megoldani problémákat, minthogy később valós gondokat szüljön. Ha bevezetünk egy gyorsítótárként
funkcionáló mechanizmust tulajdonképpen teljesen megszüntethetjük az adatbázishoz irányuló azon
lekérdezéseket, amik egy adott eszközhöz tartozó *flow* listát kérnek le. Ez a gyorsítótár egy olyan
táblázat, melynek kulcsai az eszköz azonosítók és a kulcsokhoz tartozó értékek egy-egy listát tartalmaznak,
melyekben a kulcsban szereplő eszközhöz tartozó *flowk* találhatóak. Lényegében listákat tartunk arról,
hogy melyik eszköz állapot jelentése esetén melyik *flowkat* kell leellenőrizni. A listák frissen
tartása se nagy feladat, hiszen egy *flow* csak akkor változhat, ha azt a felhasználó módosította.
Ezáltal ha egy *flowt* módosítanak vagy törölnek csak annyi a dolgunk, hogy kivesszük az összes gyorsítótár
listából és végigfutva a feltétel listáján belerakjuk a megfelelő eszköz azonosítójú kulcshoz tartozó
listába. Így már csak annyi dolgunk lesz egy állapot jelentés beérkezése esetén, hogy a gyorsító táblázatból
lekérjük a küldő eszköznek megfelelő kulcs-érték párt és az abban lévő *flow* listát egyenként ellenőrizzük.

### Flow létrehozás
A *flow* létrehozás fejlesztése során több lehetőség közül kellett válasszak, melyek egymáshoz képest
könnyebbek-nehezebbek voltak fejlesztési szempontból és felhasználó használati szempontból is. Megpróbáltam
egy közép utat találni a lehetőségek közt, hogy ne is legyen sok munkát igénylő fejlesztés és mégis
hamar megérthető legyen a felhasználóknak.

A lehető legflexibilisebb beviteli módszer valamilyen egyedi programozási nyelvszerű bemenet lenne.
Tehát lehetne egy saját szintaxisa annak, hogy miként kell megadni egy *flowt*. Ezzel a lehetőséggel
inkább azokat a felhasználókat lehetne megcélozni, akik hajlandóak akár órákat is tölteni azzal, hogy
megvalósítsák a maguknak tökéletes rendszert és ezért képesek lennének megtanulni egy rendkívül
leegyszerűsített programozási nyelvet, amit csak erre a célra találtak ki. Természetesen ez nem mindenkinek
megfelelő módja annak, hogy *flowt* hozzon létre, nem sokak akarnak ennyi időt elfecsérelni egyszerű
szabályok létrehozására. De nem csak a felhasználó szemszögéből rendkívül bonyolult ez a bemenet,
hanem fejlesztési szempontból is. Egy saját kódelemzőt kéne írni, ami képes lenne feldolgozni a neki
beadott programot. Fel kellene oldani az eszköz neveket magukra az eszközökre vagy esetleg elvégezni
matematikai feladatokat, amiket a felhasználó beleírhat a kódba. Sejthető, hogy nem ezt a megoldást
választottam, mint végleges *flow* beviteli mód.
```javascript
{
  name: "Nappali fűtés"
  when:
    "Nappali hőmérő" is under '20' AND
    TIME is later than 13:50
  then:
    set "Nappali fűtés \#1" to '1',
    set "Nappali fűtés \#2" to '1'
}
```
\begin{center}
\captionof{figure}{Példa program egy lehetséges megvalósításhoz}
\end{center}

Ha a felhasználónak akarunk nagyon kedvezni, akkor létrehozhatnánk egy drag'n'drop felülettel a
létrehozást. Minden feltétel vagy hatás típus a képernyő szélén létrehozhatnánk külön, majd behúzzuk
egérrel oda, ahova szeretnénk tenni. Rendkívül átlátható lenne és pár perc alatt össze lehet rakni
vele akár aránylag bonyolult szabályokat is. Ami mégis ezen mód ellen szól, hogy elég bonyolult feladat
lehet a felületen ezt megvalósítani. Sajnos a felületet kezelő *Vaadin* keretrendszer nem támogat
egyszerű drag'n'drop elemeket, ezért úgy döntöttem, hogy ez a bemeneti mód se felel meg elvárásaimnak.
Túl bonyolult és aránytalanul hosszas lett volna implementálni számomra a rendszer többi részéhez képest.

Végül egy olyan felülettel oldottam meg a problémát, ahol egy-egy oszlop jelképezi a feltételek és
hatások listáját minden *flowban* egyenként. A \ref{fig:flow_creation}. ábrán látható egy példán
keresztül is. Az oszlopokba egyesével lehet hozzáadni és törölni a feltételeket és hatásokat.
Mivel minden feltétel vagy hatás típusnak más bemenetekre van szüksége, ezért ahogy változtatjuk egy
feltétel vagy hatás típusát, úgy változik a mellette megjelenő bemeneti mezők kinézete is. Meg lehet
még adni egy rendezési számot is, ami amolyan prioritási sorrendnek tekinthető. Erre azért van szükség,
ha egy beérkező adat kivált több *flow* végrehajtást is, akkor ne nem-determinisztikus sorrendben lesznek
lefuttatva, hanem elsőnek a kisebb rendezési számmal rendelkező és felfele haladva a többi.

\begin{center}
  \includegraphics[width=\textwidth]{source/figures/07_flow_creation.png}
  \captionof{figure}{Képernyőkép flow létrehozásról}
  \label{fig:flow_creation}
\end{center}

# Flow rendszer
Ezek az úgynevezett "flow"-k olyanok akárcsak egy-egy szabály. Olyan szabályok
melyeknek van egy feltétele és egy hatása. Egy flow akkor lép életbe,
ha a hozzátartozó feltétel a rendszer éppen aktuális állapota mellett teljesül.
Ekkor a flow hatása végrehajtódik a rendszer által.

A feltétel része lehet egyes eszközök állapotára vonatkozó megszorítások
(pl.: 20°C-nál nagyobb a hőmérsékletet mutat a nappaliban elhelyezett hőmérő),
a felhasználó vagy külső rendszer által indított kérés az alkalmazáshoz
(pl.: gomb nyomás a kezelőfelületen vagy HTTP kérés egy bizonyos címen),
egyéb rendszer állapot feltétel (pl.: időponthoz kötődő feltétel)
és ezeknek logikai ÉS-sel összekötött kombinációja.
A hatása egy flow-nak állhat eszközök állapotának módosításából,
más rendszerhez történő kérésből és egyéb segéd akciókból (pl.: késleltetés).
Ezeknek a "hatás elemeknek" egymás után történő végrehajtása adja az adott
flow hatását.

A rendszer célja az, hogy a fenti flow-k segítségével a felhasználó
szabályokat/feladatokat tud leírni, amelyeket a rendszer majd végrehajt.
Így tehát lehetséges bizonyos házkörüli dolgok automatizálása.

Pár példa a rendszer használatára:
* ha adott szobában nincs érzékelt mozgás, akkor lekapcsolódik a villany
* ha több hőmérő is alacsony értéket mutat, akkor automatikusan fentebb megy a fűtés
* ha egy virág földje kiszáradna, akkor víz engedődik a virág alatt
* ha reggeli időpont van, akkor beindul a kávéfőző
* ha a levegő szén-monoxid tartalma átlép egy határt, akkor elindul egy jelző berendezés
* ha besötétedik és van mozgás, akkor a sötétítok lengednek és felkapcsol egy villany

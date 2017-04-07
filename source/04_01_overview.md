# Rendszer áttekintés

Ebben a fejezetben egy kezdeti képet szeretnék adni a teljes rendszer alapvető
felépítéséről, illetve arról, hogy az egyes rendszer részek felé milyen elvárások
vannak. Minden részt kifejtek ebben a fejezetben, de csak annyira, hogy átláthatóvá
tegye a dolgozat többi részét. Későbbi fejezetekben viszont majd mélyrehatolóbban
nézzük meg a rendszer alapjait.

\noindent
Alapvetően három részre bontható a rendszer:  

- Eszközök, amely magába foglalja az összes mérőeszközt és egyéb kisebb beágyazott
  rendszereket. Ezek egyszerű mikrokontrollereket használó rendszerek, amiknek
  annyi szerepe van, hogy vagy adatot szolgáltatnak, vagy valamilyen funkcionalitást
  végeznek el.
- Hálózati réteg, amely egy egyszerű közvetítőként funkcionál a központi szerver
  és az eszközök között. Lényegében átalakítja az üzeneteket a két rész között
  a megfelelő formátumra.
- Központi szerver, amely végzi az adatok feldolgozását és biztosít egy felületet
  a felhasználó számára, hogy láthassa a rendszer állapotát vagy aktuális adatokat.

\input{source/figures/04_fig_overview.md}

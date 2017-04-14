## Üzenetek felépítése
A hálózati réteg legfontosabb funkciója, hogy átalakítsa az üzeneteket megfelelő formátumra a központi
szerver és az eszközök között. Erre azért van szükség, mert míg a központi szerverrel egy TCP kapcsolaton
keresztül JSON üzenetekkel kommunikálunk, addig az eszközökkel nyers bitfolyamokon keresztül C-beli
`struct`-okat használva tudunk. Egy üzenet típusa vagy állapot jelentés vagy állapot beállítás lehet.

Az üzenetekben használt csomópont azonosító megegyezik a \ref{ch:network_structure}. alfejezet
csomópont azonosítóival. Az adat típus sztring literálok megmondják, hogy az adott eszköz milyen
specifikus hardverrel rendelkezik, például a `temperature` adat típust a hőmérő szenzor használja.
Fontos még megemlíteni, hogy logikusan az állapot jelentő üzenetek mindig egy eszköztól indulnak és
az állapot beállító üzenetek mindig a központi szervertől.

### Kommunkáció a központi szerverrel
Azért esett a választásom a TCP kapcsolaton keresztüli kommunikációra a központi szerver és a hálózati
réteg közt, mert jelentősen egyszerűbb megoldani, mint más technológiákkal. Például megoldás lett volna
még, hogy közös megosztott memória területen dolgozzon a központi szerver és a hálózati réteg. A hátránya
az lett volna az osztott memóriának, hogy kötelező egy eszközön futnia a kapcsolat mindkét felének.
Hasonló indokok állnak a mögött is, hogy miért JSON-t választottam az üzenetek formai alapjának.
Szintén ez volt a legkönnyebben és leghamarabb megvalósítható megoldás. Habár itt már voltak más életképes
megoldások is, mint például a Google által fejlesztett *Protobuf*, ami egy programozási nyelvfüggetlen
adatcsere formátum. A JSON, azért tűnt mégis jobbnak számomra, mert könnyebben tudtam változtatni az
üzenetek formátumán és kevesebb velejáró programozást igényelt.

- Állapot jelentés üzenet:
```javascript
{
  id: [csomópont azonosító],
  type: [adat típusa],
  value: [adat érték],
  category: [aktor vagy szenzor]
}
```

- Állapot beállító üzenet:
```javascript
{
  id: [csomópont azonosító],
  targetState: [kívánt állapot]
}
```
### Kommunkáció az eszközökkel
Több különbség is észrevehető a központi szerver üzeneteihez képest. Elsőnek azt láthatjuk meg, hogy
sehol nincs `id` mező, de nincs is rá szükség, mivel az üzenetet csak `id` csomópont azonosítóval
rendelkező eszköznek küldjük el. Második különbség, hogy nincs `category` mező az állapot jelenő
üzenetben. Erre azért nincs szükség, mert a hálózati kapcsolatot irányító programkönyvtár megengedi,
hogy felcimkézzük az üzeneteket. Ezt kihasználva minden eszköz, mivel tudja magáról, hogy melyik
kategóriába tartozik megfelelően cimkézi az üzeneteket. Ez a cimke lesz átalakítva mezővé a központi
szervernek küldött üzenetekben.

- Állapot jelentés üzenet:
```c
struct state_update_message {
  float data; // adat érték
  char type[15]; // adat típus
};
```

- Állapot beállító üzenet:
```c
struct state_set_message {
  float targetState; // kívánt állapot
};
```

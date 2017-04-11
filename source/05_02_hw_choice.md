## Hardver választás

### Arduino
Az Arduino-k egyszerűen használható AVR mikrokontroller alapú fejlesztői platfromok, melyeknek az a
céljuk, hogy megkönnyítsék az elektronikával való ismerkedést az átlagemberek számára. Ezeket az
eszközöket az Arduino, mint vállalat tervezte, illetve tervezi, hiszen folyamatosan újabb és újabb
Arduino platformok kerülnek napvilágra. Az Arduino vállalatnak fő célja az volt az Arduino eszközök
tervezésénél, hogy minél több embernek legyen elérhető az elektronikus eszközökkel való barkácsolás.
Mindezt úgy próbálják elérni, hogy olcsón előállíthatóak az eszközök, hogy leegyszerűsítették a
programozást, illetve, hogy minden hardver tervet nyílt forrásúvá tettek.

Az én választásom is a fent említett előnyök miatt esett az Arduino fejlesztői platformokra. A nyílt
források miatt kínai Arduino klónok lepték el a piacot. Akár 1000 forintért [^arduino_clone_price] is hozzájuthatunk egy ilyen
klónhoz, de akár az eredetit is beszerezhetjük szintén nem túl drágán körülbelül 6000 forintért [^arduino_hungarian_price]
magyarországi internetes boltokból.

[^arduino_clone_price]:
Arduino Uno klón: <https://tinyurl.com/ArduinoUnoClone> (2017. április 11.)

[^arduino_hungarian_price]:
Eredeti Arduino Uno Magyarországon: <https://tinyurl.com/ArduinoUnoHun> (2017. április 11.)

A legfontosabb érv számomra mégis az, hogy nagyon egyszerű programozni az Arduinokat. Ezért is
figyeltem fel rájuk és töltöm sokszor szabadidőmet Arduinokat használó saját projektekkel. A platformok
mellé az Arduino vállalat aktívan fejleszt egy saját programozási környezetet és egy programozási
nyelvet is. Az "Arduino" programozási nyelv valójában egy olyan C/C++ könyvtár, amely egyszerűsíti
az eszközök használatóhoz kapcsolódó programozási feladatokat. Például ahhoz, hogy egy LED-et
villogtassunk elég a következő kód:
``` {.cpp .numberLines}
// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  // turn the LED on (HIGH is the voltage level)
  digitalWrite(LED_BUILTIN, HIGH);
  // wait for a second
  delay(1000);                       
  // turn the LED off by making the voltage LOW
  digitalWrite(LED_BUILTIN, LOW);
  // wait for a second
  delay(1000);                       
}
```
\begin{empty}
\captionof{figure}{Hivatalos Blink.ino Arduino példakód}
\end{empty}
\noindent
Továbbá mivel C/C++ az alapnyelv az Arduino "nyelv" támogat minden olyan C vagy C++-beli nyelvi
elemet, amit a `avr-gcc` fordítóprogram támogat.

### Vezetéknélküli modulok
\label{ch:wireless_modul}


### Eszköz specifikus hardver
Közös megszorítás volt természetesen minden eszköz specifikus modul kiválasztásánál, hogy könnyedén
lehessen használni Arduinokkal. Ez általában abban nyilvánult meg, hogy az interneten találtam-e
Arduinokhoz írt könyvtárakat, amik az adott modul képességeinek használatát teszik lehetővé. Választási
szempontom volt még, hogy mennyire drága az eszköz. Néhol érdemes feláldozni a mérési pontosságot ahhoz,
hogy ne kelljen sok pénzt kiadni. Mivel minden eszköztípus más érzékelőt vagy más funkciót elvégző
alkatrészt tartalmaz, illetve mivel konkrétan akármilyen funkciójú eszközt megtervezhetünk, ezért
nem tudok minden eszköz specifikus modult bemutatni. Viszont korábban említettem, hogy kiválasztottam
pár szenzort és aktort, amit megterveztem és támogattam a rendszer fejlesztése során. Ezek a következőek:

- Hő- és páratartalom szenzor
- Mozgásérzékelő szenzor
- Föld nedvesség szenzor
- Elektromos relé aktor

A *hő- és páratartalom szenzor* elkészítéséhez az úgynevezett `DHT22` modult használtam, ami
egyszerre képes hőmérsékletet és páratartalmat mérni. Eredetileg nem terveztem, hogy egy szenzor akár
többféle adatot is tud szolgáltatni a központi rendszernek, de mivel egy ilyen kettő-az-egyben modult
találtam, ezért hamar változtattam az eredeti elképzeléseimen. Azért ezt a DHT22 modult választottam,
mert szinte csak ehhez találtam Arduinot használó példákat. Pontosságát tekintve a \textpm 2%-os
páratartalom és \textpm 0.5°C-os hőmérséklet hibahatár bőven megfelelt az én céljaimra. A modul ára
is aránylag kedvező, hiszen körülbelül 3000 forintért [^dht22_price] beszerezhető egy-egy darab.
\begin{center}
\includegraphics[scale=0.6]{source/figures/05_dht22_module.jpg}
\captionof{figure}{DHT22 hő- és páratartalom érzékelő modul}
\end{center}

[^dht22_price]:
DHT22 hő- és páratartalom érzékelő: <https://www.adafruit.com/product/385> (2017. április 11.)

A *mozgásérzékelő szenzor* elkészítéséhez `PIR szenzort` használtam. A PIR feloldása a "*passive infrared*",
vagyis az infravörös fények változását képes érzékelni a szenzor. Ilyen PIR szenzorokat használnak
például a mozgásra felkapcsoló kertilámpák is. Ezek egyszerű eszközök, általában működésük annyiból áll,
hogy amikor mozgást érzékelnek elindul egy időzítő és az amíg le nem jár, addig a modul magas jelet
ad. Ahogy az időzítő lejárt eltűnik a jel. Ennek a jelnek az értelmezése rendkívül egyszerű Arduinokkal.
Elég, ha rákötjük a PIR szenzor kimenetét az Arduino egyik bemenetére és figyeljük mikor változik a
bemeneten a jel. Az időzítő hossza és a modul érzékenysége kézzel állítható az eszközön. Ahogy az előző
esetben is, most se magas az ára egy ilyen modulnak. Körülbelül ezek a szenzor modulok is 3000 forintba [^pir_price]
kerülnek.
\begin{center}
\includegraphics[scale=0.6]{source/figures/05_pir_module.jpg}
\captionof{figure}{PIR mozgásérzékelő modul}
\end{center}

[^pir_price]:
PIR mozgásérzékelő: <https://www.adafruit.com/product/189> (2017. április 11.)

A *föld nedvesség szenzor* még a *mozgásérzékelő szenzornál* is egyszerűbb mérőeszközt igényel.
A legegyszerűbb módja annak, hogy a föld nedvességét megmérjük az, hogy két föld alá dugott fémlemez
között megmérjük a föld ellenállását. Minél kisebb az ellenállás, annál nedvesebb a föld. Ezt az elvet
használó érzékelő modulokat könnyedén lehet találni és nem is drágán. Egy-egy ilyen érzékelő ára
1500 forint [^soil_price] körül mozog. Ahhoz, hogy működésre bírjük ezeket az eszközöket,
szintén csak annyi a dolgunk a kimenetet az Arduino egyik bemenetére kötjük és onnan leolvassuk az
ellenállás értékét. Abból tudunk következtetni mennyire száraz vagy nedves a föld.
\begin{center}
\includegraphics{source/figures/05_soil_moisture_module.jpg}
\captionof{figure}{Föld nedvességmérő modul}
\end{center}

[^soil_price]:
Föld nedvességmérő: <https://www.sparkfun.com/products/13322> (2017. április 11.)

Az *elektromos relé aktor* esetében egy 5 voltos relére volt szükségem. Ha magasabb feszültséget
igénylő relét akartam volna használni, akkor plusz alkatrészekre lett volna szükségem ahhoz,
hogy Arduinoval irányíthassam. Egy relé alapvetően úgy működik, mint egy akármilyen villanykapcsoló,
a különbség csak annyi, hogy elektromos jellel lehet kapcsolni. A relé egyik oldalára az
irányítani kívánt áramkört kell bekötni, a másikra a mi esetünkben az Arduino egy kimenetét
és az említett 5 voltot. Így a kimenet fel- és lekapcsolásával a bekötött áramkört is kapcsolgatjuk.
Az ára a reléknek rendkívül változó tud lenni, annak függvényében, hogy mekkora teljesítményt bírnak
ki. Én egy 10 ampert kibíró relé mellett döntöttem, ami 2500 forint [^relay_price] körüli összegbe kerül.
\begin{center}
\includegraphics{source/figures/05_relay_module.jpg}
\captionof{figure}{Elektromos relé modul}
\end{center}

[^relay_price]:
Elektromos relé: <https://www.sparkfun.com/products/13815> (2017. április 11.)

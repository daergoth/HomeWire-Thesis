## Keretrendszerek

### Spring Boot
A teljes szerver architektúrális alapját a *Spring Boot* adja, amely a *Spring* alkalmazásfejlesztési
keretrendszer egy olyan verziója, ami előre megoldja nekünk a *Spring* konfigurációját. Természetesen
ez csak úgy lehetséges, ha elfogadunk néhány konvenciót, hogy miként épüljön fel az elkészített alkalmazásunk.
A fő célja a *Spring Bootnak*, hogy a fejlesztés a lehető leghamarabb elérje azt a fázist, ahol már
az alkalmazás logikát lehet készíteni. Java webalkalmazások fejlesztésénél időről-időre előfordul,
hogy nagyon hasonló kódot kell írni bizonyos háttérműködések futásához. Ezen boilerplate kód írásának
a kihagyása sok időt és ezáltal pénzt tud megspórolni vállalati fejlesztési környezetben. Én is azért
választottam a *Spring Bootot*, mert csak arra kellett figyeljek, ami lényeges volt a szerver működésének
szempontjából és így nem vesztegettem az örökké szűkös fejlesztési időt.

A *Spring* keretrendszer magában nem több olyan konfigurációs és programozási modelleknél, amik
hasznosak tudnak lenni Java-alapú vállalati alkalmazások fejlesztésénél. Önmagában a *Spring* olyan
feladatokat lát el számunkra, mint a tranzakció-kezelés, adatkapcsolat elérése, függőség kezelés és
injektálás, illetve webes servletek létrehozása. Ezek közül számomra főleg a függőség injektálás és
a webes servletek voltak nagy hasznomra, igaz a servletek esetében plusz absztrakciót ad a *Vaadin*
keretrendszer. Azonban ez csak a *Spring* alapcsomagja, emellett léteznek olyan modulok hozzá, mint
a *Spring Data*, amely többféle adatelérési technológiát tesz egységesen használhatóvá vagy éppen
a *Spring Security*, ami gondoskodik az alkalmazásunk autentikációjáról és autorizációjáról. Ezeknek
a moduloknak a segítségévél és az alap *Spring* csomaggal tudunk igazán gyorsan és könnyedén
webalkalmazásokat létrehozni. [@SpringGuide]

A *Spring Boot* tudja mindazt, amit fent leírtam és ahhoz, hogy használni tudjuk elég egy *Maven*
függőséget és egy szülő projektet megadni a mi alkalmazásunk `pom.xml`-jében. Az \ref{fig:springboot_starter_pom}.
ábrán látható `pom.xml` részlet megadása mellett további *starter* függőségek hozzáadásával a már
korábban említett külső modulokat is konfiguráció-mentesen tudjuk használni az alkalmazásunkban. Ha
például a *Spring Data JPA* implementációjának *Spring Boot* verzióját szeretnénk hozzáadni az
alkalmazásunkhoz, akkor a `spring-boot-starter-data-jpa` *Maven* függőségre lesz csak szükségünk. [@SpringBootGuide]
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.3.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
\begin{center}
  \captionof{figure}{Szükséges Maven pom.xml részlet Spring Boot használatához}
  \label{fig:springboot_starter_pom}
\end{center}    

### Vaadin
A felhasználói felület teljes egészében a *Vaadin* UI keretrendszer segítségével lett összerakva.
Azért összerakva, mivel a *Vaadin* egy komponens alapú rendszer, vagyis különféle előre elkészített
felület darabok segítségével és Java kód írásával, rövid idő alatt komplex megjelenítést
vagyunk képesek létrehozni az alkalmazásunkhoz. Például az \ref{fig:vaadin_example}. ábrán látható
Java kódrészlet egy szöveg dobozt és egy gombot fog megjeleníteni a felületen. Észrevehetjük még, hogy
a gomb kattintásának lekezelése is *Java-ban* van implementálva és nem valamilyen kliens-oldali nyelven.
Ezt a *Vaadin* szerver-vezérelt fejlesztési modelljének köszönhetjük, ami azt jelenti, hogy a böngészőben
futó kliens kód folyamatosan kommunikál a szerverrel *HTTP* vagy *WebSocket* üzenetek formájában.
Tehát, ha a felhasználó rákattint a gombra, egy üzenet generálódik a szervernek a gomb lenyomásról és
erre a szerver egy értesítés megjelenítési üzenetet küld vissza, amit a kliens egyből végrehajt. A
szerver-vezérelt modell rengeteg előnnyel járhat, javíthatja akár az alkalmazásunk biztonságát vagy
éppenséggel könnyen változtathatóvá teszi felületet működtető logikát. [@VaadinArchitecture; @VaadinHome]
```java
TextField name = new TextField("Your Name", "Vaadin");

Button greetButton = new Button("Greet");
greetButton.addClickListener(event ->
    Notification.show("Hello " + name.getValue());

VerticalLayout layout = new VerticalLayout(name, greetButton);
```
\begin{center}
  \captionof{figure}{Példa kód a Vaadin használatára}
  \label{fig:vaadin_example}
\end{center}

Természetesen vannak bonyolultabb beépített komponensek, mint egy szövegdoboz, például lenyíló menük
vagy akár táblázatok. Azonban, ha valamilyen oknál fogva nem találjuk a megfelelő komponenst, akkor
sajátokat is létrehozhatunk. Az \ref{fig:statistics_page}. ábrán a rendszer statisztikákért felelős
felülete látható, aminek fejlesztése során szükségem volt egy idősor grafikont megjelenítő modulra.
Egy kliens-oldali grafikonrajzoló külső programkönyvtár segítségével sikerült is létrehozni az egyedi
grafikon megjelenítő komponenst. Ez a példa tökéletesen bemutatja a *Vaadin* rugalmasságát és könnyű bővíthetőségét.

\begin{center}
  \includegraphics[width=\textwidth]{source/figures/07_statistics.png}
  \captionof{figure}{Eszköz statisztikák felülete}
  \label{fig:statistics_page}
\end{center}

Számomra azért volt jó választás a *Vaadin*, mert a komponens alapúsága tényleg egyszerűvé tette a
felület létrehozását. Illetve, mivel teljes egészében *Java-ban* lehet fejleszteni, nem volt szükséges
megtanuljak egy számomra teljesen új kliens-oldali nyelvet, mint például a *JavaScript*. A beépített
komponensekhez előre meg voltak írva a kinézetek, így azzal se kellett sok időt vesztegessek, hogy a
felület stílusát formázgassam.

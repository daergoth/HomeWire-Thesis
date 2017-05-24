# Eszközök
Az előző fejezetben megtudtuk, mit csinálnak az eszközeink. Szintén megismertük, hogy mit is jelent
a *szenzor* és az *aktor* fogalma, illetve, hogy miben különböznek. Ebben a fejezetben ecsetelve lesz,
milyen hardverek lettek kiválasztva az eszközök összeállítására, illetve meg lesznek indokolva ezen
választások is.

Ugye tudjuk szintén az előző fejezetből, hogy minden eszköz "agya" egy mikrokontroller. Ez a kicsi és
alacsony teljesítményű processzor felelős az adatgyűjtés folyamatának koordinálásáért. Az *Arduino*
fejlesztői platformok mellett tettem le a voksom, mert nem igényel elmélyült hardverismeretet a programozásuk.

Másik fontos alkotóelem, a vezetéknélküli modul. Ebben a választásban már természetesen megszorítás
volt, hogy a kiválasztott modul kompatibilis legyen Arduinokkal. Habár nem volt egyszerű döntés,
egy *nRF24L01+* adó-vevő integrált áramkört felhasználó modult választottam.

Ahogy korábban említettem harmadik része az eszközeinknek változó, mégpedig az határozza meg, hogy
mit szeretnénk elérni az adott szenzorral vagy aktorral. A fejlesztés elején kiválasztottam pár
eszköztípust, amit el szerettem volna készíteni és támogatni a kész rendszerben. A teljesség igénye
nélkül be fogok mutatni pár szenzor és aktor modult, mint például a hőmérséklet mérésére szolgáló
*DHT22* hő- és légnedvesség mérő szenzor modult.

Mindezek után kitérek arra, hogy a fent említett hardvereket hogyan kell összekötni, hogy egy működő
eszközt kaphassunk. Szerencsémre az összerakási folyamat nem igényelt jelentős villamosmérnöki tudást,
elég volt néhány kábellel összekötni a megfelelő ki- és bemeneteket.

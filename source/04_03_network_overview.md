## Hálózati réteg
A hálózati réteg léte elsőnek nem látszik logikusnak. Feleslegesnek tűnhet egy
plusz szereplőt bevonni a központi szerver és az eszközök közé. A probléma, ami
mégis megköveteli azt, hogy legyen egy köztes réteg, az egy eszközökhöz kötődő
döntésből fakad.   

A hardveres tervezés alatt találkoztam több vezetéknélküli
kommunikációs modullal is. Volt olyan, ami WiFi-t használ a kommunikációhoz, volt,
ami csak simán rádiófrekvenciás modul volt. Bizonyos okok miatt, amit majd a \ref{ch:wireless_modul}.
alfejezetben fogok leírni, a rádiófrekvenciás modulok mellett döntöttem. Így viszont
szükség van egy központi eszközre, ami egy ugyanolyan kommunikációs modullal kell
rendelkezzen, mint a többi eszköz. Ahhoz, hogy ezt a modult használni tudjuk, viszont
alacsonyabb szintű eszközökhöz kell folyamodni, mint amit a Java nyújtani tud.
Pont ezért a hálózati réteg egésze C++-ban íródott.

Lényegében a hálózati réteg tartja fent a kapcsolatot az eszközökkel és alakítja
át az üzeneteket a fogadó félnek megfelelő formátumra. Ez az alakítás azért
szükséges, mert a kommunikáció a hálózati réteg és a központi szerver között egy
TCP kapcsolaton keresztül történik JSON objektumok küldésével, viszont a hálózati
réteg és az eszközök között C-beli `struct`-okat küldünk rádiófrekvenciás jelek
segítségével.

## Eszközök
Mint már említve volt, ide tartoznak azok a hardverközeli rendszerek, amik vagy
adatgyűjtő szerepet töltenek be, vagy valamilyen funkcionalitást képesek végrehajtani.
Lehet egy ilyen eszköz például egy hőmérő, légnyomásmérő, földnedvességmérő vagy
éppen egy relé.

Közös bennük, hogy mindegyikben van egy mikrokontroller és egy kommunikációért felelős
modul. E mellett mindegyik eszköz tartalmaz még egy specifikus modult is, ami
meghatározza, hogy mit is csinál az adott eszköz (pl.: hőmérő esetén egy hőmérséklet-
és páratartalom érzékelő).  

A mikrokontroller vezérli az adatgyűjtés és küldés folyamatát. Az idő nagy részében
alvó állapotban van energiatakarékossági szempontok miatt, viszont fix időközönként
felébred. Mikor felébred 

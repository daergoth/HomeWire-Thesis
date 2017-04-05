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

\begin{figure}
  \centering
  \begin{tikzpicture}
    \draw  (-8,0.5) rectangle (-4,-1.5);
    \draw  (0,3) rectangle (2,-4);
    \draw  (6,2.5) rectangle (9.5,1.5);
    \draw  (6,0.5) rectangle (9.5,-0.5);
    \draw  (6,-2) rectangle (9.5,-3.5);

    \node (v1) at (-4,0) {};
    \node (v2) at (0,0) {};
    \draw [->] (v2) edge (v1);
    \node (v4) at (-4,-1) {};
    \node (v3) at (0,-1) {};
    \draw [->] (v4) edge (v3);

    \node at (-2,0.5) {Állapotjelentés};

    \node at (-2,-1.5) {Állapot beállítás};
    \node (v5) at (6,2) {};
    \node (v6) at (2,2) {};
    \node (v7) at (6,0) {};
    \node (v8) at (2,0) {};
    \node (v9) at (6,-2.5) {};
    \node (v10) at (2,-2.5) {};
    \node (v11) at (2,-3) {};
    \node (v12) at (6,-3) {};
    \draw [->] (v5) edge (v6);
    \draw [->] (v7) edge (v8);
    \draw [->] (v9) edge (v10);
    \draw [->] (v11) edge (v12);
    \node at (-6,-0.5) {Központi szerver};
    \node at (1,0) {Hálózati};
    \node at (1,-1) {réteg};
    \node at (7.75,2) {Hőmérő};
    \node at (7.75,0) {Légnyomásmérő};
    \node at (7.75,-2.5) {Elektromos};
    \node at (7.75,-3) {relé};

    \node at (4,2.5) {Állapotjelentés};
    \node at (4,0.5) {Állapotjelentés};
    \node at (4,-2) {Állapotjelentés};
    \node at (4,-3.5) {Állapot beállítás};
  \end{tikzpicture}
  \caption{Rendszer felépítés. Jól látható az ábrán, hogy miként is kommunikálnak
  egymással a rétegek és eszközök.}
  \label{fig:04_overview}
\end{figure}

\FloatBarrier

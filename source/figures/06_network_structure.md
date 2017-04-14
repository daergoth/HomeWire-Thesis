\begin{center}
  \begin{tikzpicture}
    \node[draw] (v1) at (0.5,0) {Központi csomópont};

    \node[draw] (v4) at (-2.5,2) {Csomópont 1};
    \node[draw] (v2) at (0.5,-2.5) {Csomópont 2};
    \node[draw] (v3) at (3.5,2) {Csomópont 3};

    \draw  (v1) edge (v2);
    \draw  (v1) edge (v3);
    \draw  (v1) edge (v4);

    \node[draw] (v5) at (-2.5,4.5) {Csomópont 1.1};
    \node[draw] (v6) at (-5,3.5) {Csomópont 1.2};
    \node[draw] (v7) at (-6.5,1.5) {Csomópont 1.3};

    \node[draw] (v8) at (5,4) {Csomópont 3.1};

    \node[draw] (v9) at (-1.5,-5) {Csomópont 2.1};
    \node[draw] (v10) at (2.5,-5) {Csomópont 2.2};

    \draw  (v4) edge (v5);
    \draw  (v4) edge (v6);
    \draw  (v4) edge (v7);
    \draw  (v3) edge (v8);
    \draw  (v2) edge (v9);
    \draw  (v2) edge (v10);
  \end{tikzpicture}
  \captionof{figure}{A hálózat felépítése}
  \label{fig:06_network_structure}
\end{center}

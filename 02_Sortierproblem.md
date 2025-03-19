# Sotierproblem

## Definition

Es ist eine Folge von Objekten gegeben, welche nach deren Schlüsselwerten sortiert werden sollen.
Es muss also eine totale Ordnung $\leq$ auf der Menge $M$ aller möglichen Schlüsselwert geben. 

## Totale Ordnung

Sei $M$ eine nicht leere Menge und $\leq$ $\subseteq M \times M$ eine binäre Relation auf $M$.

Die Relation $\leq$ auf $M$ ist genau dann eine totale Ordnung, wenn gilt:

- Reflexivität: $\forall{x \in M}:xRx$
- Transitivität: $\forall{x,y,z \in M}:xRy \wedge yRz \Rightarrow xRz$
- Antisymmetrie: $\forall{x,y \in M}:xRy \wedge yRx \Rightarrow x=y$
- Totalität: $\forall{x,y \in M}:xRy \vee yRx$

## Laufzeitanalyse

Beschreibt wieviele Schritte der Algorithmus in Abhängigkeit von der Eingabekomplexität macht.
Also:

> $T(n)=max\{\text{Anzahl Schritte für } x\}$

## Asymptotische Komplexität

Ermöglicht es Algorithmen nach ihrer Effizienz zu vergleichen und hilft dabei zu identifizieren, bei welchen Problemen welcher Algorithmus am besten geeignet ist.
Außerdem gelten folgende Aussagen:

1. Asymptotische Notation "versteckt" konstante Faktoren und Summanden der Laufzeitfunktionen.
2. Asymptotische Notationen helfen dabei, die Entwicklung der Laufzeit abhängig von der Eingabelänge für große
Eingabelängen abzuschätzen.

## Landau-Symbole

### $\Theta$-Notation

Beschränkt eine Funktion $f$ asymptotisch nach oben und unten.

- $f(n)=\Theta(g(n))$, wobei $g(n)$ eine scharfe Schranke von $f(n)$ ist

$$\Theta(g)=\{f : \exists c_1, c_2 \in \mathbb{R}_{>0}, n_0 \in \mathbb{N}, \forall{n \geq n_0}, 0 \leq c_1g(n)\leq f(n) \leq c_2g(n)\}$$

![Theta-Notation Beispiel\label{fig:theta-notation}](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/theta-notation.png){ width=12rem }

### O-Notation - Big-O-Notation

Beschränkt eine Funktion $f$ asymptotisch nach oben. Wird vor allem deshalb in der Informatik viel verwendet. (Ziel: **effizienter Algorithmus**)

- Sprechweise: $f$ wächst höchstens so schnell wie $g$
- Schreibweise: $f=O(g)$ oder $f \in O(g)$

$$O(g)=\{f : \exists c \in \mathbb{R}_{>0},n_0 \in \mathbb{N},\forall{n\geq n_0,0 \leq f(n) \leq cg(n)}\}$$

![O-Notation Beispiel](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/o-notation.png){ width=12rem }

#### Rechenregeln

- **Konstante**: $f(a)=O(1)$
- **Skalare Multiplikation**: $a \cdot f=O(g)$ 
- **Addition**: $f_1+f_2=O(max(g_1,g_2))$
- **Multiplikation**: $f_1 \cdot f_2=O(g_1 \cdot g_2)$

#### $\Omega$-Notation

Beschränkt eine Funktion $f$ asymptotisch nach unten.

- Sprechweise: $f$ wächst mindestens so schnell wie $g$
- Schreibweise: $f=\Omega(g)$ oder $f \in \Omega(g)$

$$\Omega(g)=\{f : \exists c \in \mathbb{R}_{>0},n_0 \in \mathbb{N}, \forall{n\geq n_0}, 0\leq cg(n) \leq f(n)\}$$

![Omega-Notation Beispiel](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/omega-notation.png){ width=12rem }

### $\omega$-Notation

Ist keine asymptotisch scharfe Schranke.

$$\omega(g)=\{f : \forall{c \in \mathbb{R}_{>0}}, \exists{n_0 \in \mathbb{N}}, \forall{n \geq n_0}, 0 \leq cg(n) < f(n)\}$$

### $o$-Notation

Ist keine asymptotisch scharfe Schranke.

$$o(g)=\{f : \forall{c \in \mathbb{R}_{>0}}, \exists{n_0 \in \mathbb{N}}, \forall{n \geq n_0}, 0 \leq f(n) < cg(n)\}$$

### Zusammenhang der Landau-Symbole

Für beliebige $f(n),g(n):\mathbb{N}\rightarrow \mathbb{R}_{>0}$ gilt:

$$f(n)=\Theta(g(n)) \Rightarrow f(n)=O(g(n)) \wedge f(n)=\Omega(g(n))$$

Weiterhin gilt

- $f(n) = g(n) \rightarrow f(n) \in \Theta([g(n)])$
- $f(n) \leq g(n) \rightarrow f(n) \in O([g(n)])$
- $f(n) \geq g(n) \rightarrow f(n) \in \Omega([g(n)])$

, wobei $[g(n)]$ die asymptotische Notation von $g(n)$ ist.

### Gleichheit

Gleichheit zwischen den Landau-Symbolen sollte nicht verwendet werden, da sie von z.B. $O(n^3)=O(n^4)$ auf $O(n^4)=O(n^5)$ schließen lassen würde.
Die richtige Schreibweise ist:

$$5 \cdot n^2+n^4 \in O(n^2)+n^4 \subseteq O(n^4) \subseteq O(n^5)$$

### Ungleichungen

Wie bereits im Zusammenhang der Landau-Symbole definiert, wird für $\leq$-Ungleichungen $O$
und für $\geq$-Ungleichungen $\Omega$ verwendet. Bspw.

$$5 \cdot n^2+n^4 \leq 6 \cdot n^4=O(n^4)$$

## Schleifeninvariante

### Definition

Eine Schleifeninvariante ist eine Bedingung, die vor, während und nach jeder Iteration einer Schleife in einem Computerprogramm unverändert bleibt. Es handelt sich also praktisch um eine Regel, die immer befolgt wird, egal was innerhalb der Schleife passiert. Damit kann sichergestellt werden, dass die Schleife ordnungsgemäß funktioniert und die gewünschte Ausgabe liefert.

### Beispiel

Diene eine Schleife dazu, alle Zahlen in einer Liste zu addieren.
Die Schleifeninvariante könnte lauten: *"Die Summe der Zahlen in der Liste ist gleich der bisher berechneten Gesamtsumme."*

Diese Bedingung bleibt vor dem Start der Schleife (die Summe ist 0), während jeder Iteration (die Summe wird korrekt aktualisiert) und nach dem Ende der Schleife (die Endsumme ist korrekt) erfüllt.

### Relevanz

Schleifeninvarianten sind in der Programmierung wichtig, da sie dazu beitragen, die Korrektheit und Zuverlässigkeit von Algorithmen zu gewährleisten.

## Stabilität

Ein Sortierverfahren ist dann stabil, wenn es die ursprüngliche Reihenfolge der Elemente einer Liste mit gleichem Schlüsselwert beibehält.

## Insertionsort

Insertionsort ist ein stabiler Sortieralgorithmus.

### Algorithmus

```
insertionSort(A):
    FOR i=1 TO A.length-1 DO
        key=A[i];
        j=i-1;
        WHILE j>=0 AND A[j]>key DO 
            A[j+1]=A[j];
            j=j-1;
        A[j+1]=key;
```

### Terminierung

- Jede Ausführung der `WHILE`-Schleife erniedrigt $j<n$ in jeder Iteration um 1 und bricht ab, wenn $j<0$ $\rightarrow$ terminiert also immer
- `FOR`-Schleife wird nur endlich oft durchlaufen

### Korrektheit

#### Schleifeninvariante

Bei jedem Eintritt für Zählerwert `i` (und nach letzter Ausführung) entsprechen die aktuellen Einträge in `A[0],...,A[i-1]` den sortierten ursprünglichen Eingabewerten `a[0],...,a[i-1]`. Ferner gilt `A[i]=a[i],...,A[n-1]=a[n-1]`.

##### Beweis

- Induktionsbasis $i=1$: 

> Beim ersten Eintritt ist $A[0]=a[0]$ und "sortiert". Ferner gilt noch $A[1]=a[1],...,A[n-1]=a[n-1]$.

- Induktionsschritt von $i-1$ auf $i$:

    - Vor der ($i-1$)–ten Ausführung galt Schleifeninvariante nach Voraussetzung. Inbesondere war $A[0],...,A[i-2]$ sortierte Version von $a[0],...,a[i-2]$ und $A[i-1]=a[i-1],...,A[n-1]=a[n-1]$ 
    - Durch die WHILE-Schleife wurde $A[i-1]=a[i-1]$ nach links einsortiert und größere Elemente von $A[0],...,A[i-2]$ um jeweils eine Position nach rechts verschoben. 
    - Elemente $A[i],...,A[n-1]$ wurden nicht geändert

        > Also gilt Invariante auch für $i$

    - Aus Schleifeninvariante folgt für letzte Ausführung (also quasi vor gedanklichem Eintritt der Schleife für $i=n$): 
    
        > $A[0],...,A[n-1]$ ist sortierte Version von $a[0],...,a[n-1]$ und somit am Ende das Array sortiert

### Laufzeit

Code | Aufwand | Anzahl
--|--|--
`FOR i=1 TO A.length-1 DO` | $c_1$ | $n$
`key=A[i];` | $c_2$ | $n-1$
`j=i-1;` | $c_3$ | $n-1$
`WHILE j>=0 AND A[j]>key DO` | $c_4$ | $z_5+n-1$
`A[j+1]=A[j];` | $c_5$ | $n(n-1)/2$
`j=j-1;` | $c_6$ | $n(n-1)/2$
`A[j+1]=key;` | $c_7$ | $n-1$

1. $T(n)=c_1 \cdot n+(c_2+c_3+c_4+c_7) \cdot (n-1)+(c_4+c_5+c_6) \cdot \frac{n(n-1)}{2}$
2. $T(n)=n+4 \cdot (n-1) + 3 \cdot \frac{n(n-1)}{2}$
3. **Dominanter Term** von $T(n)$: $D(n)=3 \cdot \frac{n(n-1)}{2}$
4. **Vereinfachung**: $A(n)=\frac{n(n-1)}{2}$

Diese Vereinfachungen kann man machen, da mit der Menge der Daten auch die konstanten bzw. sehr langsam ansteigenden Werte der Funktion weniger relevant werden.

Insertionsort macht maximal $T(n)$ viele Schritte und damit ist $T(n)=\Theta(n^2)$. 

Mit korrekter Anwendung kann die Laufzeit auch $\leq T(n)=O(n^2)$ sein.

**Ziel**: Finde schlechteste Eingabe um Algorithmus verlässlich zu testen.

### Beispiel

Der Comparator von Insertion-Sort vergleicht hier die Strings nach ihrer Länge.
Je länger ein String, desto weiter vorne in der sortierten Liste befindet er sich. 

![Initialarray](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/insertion_sort_initial.png)

![Insertion-Sort Schritte](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/insertion_sort_steps.png)



## Bubble Sort

Bubble Sort ist ein stabiler Sortieralgorithmus.

### Algorithmus

```
bubbleSort(A):
    FOR i=A.length-1 DOWNTO 0 DO
        FOR j=0 TO i-1 DO
            IF A[j]>A[j+1] THEN SWAP(A[j], A[j+1]);
```

**Laufzeit**: $O(n^2)$

#### Herleitung
In der ersten `FOR`-Schleife wird eine Variable $i$ mit `A.length-1` initiiert und stetig dekrementiert. Die zweite `FOR`-Schleife wird $i-1$ Male durchlaufen. Somit werden für jeden `FOR`-Schleifen Durchlauf $1+i-1=i$ Schritte benötigt. Dies entspricht dann:

$$n+n-1+...+0=\sum_{i=0}^{n}i=\frac{n(n+1)}{2}$$


## Mergesort

Mergesort ist ein stabiler Sortieralgorithmus.

### Vorgehen

- **Teilen**:
    - Teile das Array in zwei Hälften (in unserem Fall immer **abrunden**).
    - Wiederhole das Teilen rekursiv, bis jedes Teilarray nur noch ein Element enthält.
- **Sortieren**:
    - Vergleiche die Elemente der Teilarrays paarweise.
    - Sortiere die Elemente und füge sie zu einem neuen, sortierten Array zusammen.
- **Zusammenfügen**:
    - Füge die sortierten Teilarrays zusammen, indem du die kleinsten Elemente zuerst auswählst.
    - Wiederhole das Zusammenfügen, bis das gesamte Array sortiert ist.

### Algorithmen

```
merge(A,l,m,r):
    pl = l;
    pr = m+1;
    FOR i = 0 TO r-l DO
        IF pr > r OR (pl <= m AND A[pl] =< A[pr]) THEN
            B[i] = A[pl];
            pl = pl+1;
        ELSE
            B[i] = A[pr];
            pr = pr+1;
    FOR i = 0 TO r-l DO A[i+l] = B[i];
```

```
mergeSort(A,l,r):
    IF l < r THEN
        m = floor((l+r)/2);
        mergeSort(A,l,m);
        mergeSort(A,m+1,r);
        merge(A,l,m,r);
```

**Laufzeit**: $O(n \log {n})$

### Korrektheit

#### Schleifeninvariante

Bei jedem Eintritt (bzw. nach Ende) gilt $i=pl-l+pr-(m+1)$, $pl \leq m+1$, $pr \leq r+1$, `B[0...i-1]` ist sortiert und besteht aus `A[l..pl-1]`, `A[m+1...pr-1]`.
Ferner gilt: $B[i-1] \leq A[pl],A[pr]$

#### Beweis

- Induktionsbasis $i=0$ (`p=left`, `q=mid+1`):

    - Gilt mit richtiger Interpretation, da alle Arrays "leer" und `B[-1]=`$-\infty$

    - Ferner `i=0=p-left+q-(mid+1)` und `p=<mid+1`, `q=<right+1`, da `left=<mid=<right`

- Induktionsschritt von `i-1` nach `i`:

    - Invariante gilt nach Voraussetzung vor (`i-1`)-ter Iteration
    - Iteration setzt `B[i-1]` auf kleineren bzw. einzigen Wert `A[p],A[q]`
    - Nach Voraus. `B[i-2]=< A[p],A[q]`, so dass `B[i-2]=<B[i-1]`, also ist `B[0...i-1]` nach Iteration auch sortiert
    - Da Teil-Arrays vorsortiert, gilt nach 4 bzw. 7 (vor Erhöhen von p bzw. q): 
        > `B[i-1]=min{A[p],A[q]} =< A[p],A[p+1],A[q],A[q+1]`
    - Zähler des kopierten Werts wird erhöht, also `B[0...i-1]` wegen Voraussetzung aus angegebener Menge
    - Zähler `i-1` wird um eins erhöht, und entweder `p` oder `q` auch um eins
    -  Wenn `p>=mid+1` bzw. `q>=right+1` wird die Teilliste nicht mehr gewählt, also Zählerwerte nicht mehr erhöht

- Abschluss

    - Nach Ende der FOR-Schleife (`i=right-left+1`) folgt aus `i=p-left+q-(mid+1)` und `p=<mid+1, q=<right+1`, dass `q=right+1` und `p=mid+1`
    - Also besteht `B[0...right-left]` aus `A[left...mid]`,`A[mid+1...right]` und ist sortiert



### Beispiel

![Beispiel Mergesort\label{fig:merge-sort}](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/mergesort.png){ width="80%", max-width="min(35rem, 90%)"}



## Quicksort

Quicksort ist **kein** stabiler Sortieralgorithmus

### Vorgehen

- **Pivotelement wählen**:
    - Wähle ein Pivotelement aus dem Array (in unserem Fall ``left``).
- **Partitionieren**:
    - Teile das Array in zwei Teile:
        - Elemente kleiner als das Pivotelement.
        - Elemente größer oder gleich dem Pivotelement.
- **Rekursiv anwenden**:
    - Wende Quicksort rekursiv auf die beiden Teilarrays an.
- **Zusammenfügen**:
    - Kombiniere die sortierten Teilarrays und das Pivotelement zu einem sortierten Array.

### Algorithmen

```
quicksort(A,left,right):
    IF left < right THEN
        q=partition(A,left,right);
        quicksort(A,left,q);
        quicksort(A,q+1,right);
```

**Laufzeit** - Worst Case: $\Theta(n^2)$

**Laufzeit** - Randomisiert: $\Theta(n \log n)$
\end{minipage}

```
partition(A,left,right):
    pivot=A[left];
    p=left-1; q=right+1;
    WHILE p < q DO
        REPEAT p=p+1 UNTIL A[p] >= pivot; 
        REPEAT q=q-1 UNTIL A[q] =< pivot; 
        IF p < q THEN Swap(A[p],A[q]);
    return q
```

### Korrektheit von `partition`

#### Schleifeninvariante

Beim Eintritt in die `WHILE`-Schleife enthalten `A[left...p]` nur Elemente $\leq pivot$ und `A[q...right]` nur Elemente $\geq pivot$

### Beispiel

Siehe Abbildung \ref{fig:quick-sort}

![Beispiel Quicksort\label{fig:quick-sort}](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/quicksort.jpg){ width="70%", max-width="min(40rem, 90%)"}

## Radix Sort

Radix Sort ist ein stabiler Sortieralgorithmus

### Algorithmen

```
radixSort(A):
// keys: d digits in range [0,D-1]
//           B[0][],..., B[D-1][] buckets 
// (init: B[k].size=0)
    FOR i=0 TO d-1 DO // 0 least, d-1 most sign. digit
        FOR j=0 TO n-1 DO putBucket(A,B,i,j);
        a=0;
        FOR k=0 TO D-1 DO // rewrite to array
            FOR b=0 TO B[k].size-1 DO
                A[a]=B[k][b]; // read out bucket in order
                a=a+1;
            B[k].size=0; // clear bucket again
    return A
```

```
putBucket(A,B,i,j):
    z=A[j].digit[i];
    b=B[z].size;
    B[z][b]=A[j];
    B[z].size=B[z].size+1;
```

### Korrektheit

#### Schleifeninvariante von `radixSort`

Nach Eintritt in die erste Schleife ist der Array gemäß der letzten `i` Ziffern sortiert.

#### Induktion



**Induktionsanfang** $i=1$: Gilt nach erster Iteration, weil nach letzter Ziffer sortiert wird

**Induktionsschritt** $i \rightarrow i+1$:

**1. Fall**:

Wenn $(i+1)$-te Ziffer zweier beliebiger Werte verschieden, dann wird Wert mit kleinerer $(i+1)$-ten Ziffer weiter vorne einsortiert, zuerst wieder ausgelesen und steht somit auch im Array vorher


**2. Fall**:

Wenn $(i+1)$-te Ziffer gleich, dann steht nach Induktionsvoraussetzung der auf letzten $i$ Ziffern kleinere Wert weiter links, wird zuerst einsortiert und auch zuerst wieder ausgelesen

### Laufzeit

- Gesamtlaufzeit: $O(d\cdot(n+D))$
- Größe Zifferbereich $D$ oft als konstant angesehen: $O(dn)$
- Wenn auch $d$ als konstant angesehen: $O(n)$

**Aber**: eindeutige Schlüssel für $n$ Elemente benötigen $d=\Theta(\log_D{n})$ Ziffern!

- Laufzeit: $O(n\cdot \log{n})$

### Beispiel

Siehe Abbildung \ref{fig:radix-sort}

![Beispiel Radix Sort\label{fig:radix-sort}](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/radix-sort-example.png){ width="70%", max-width="min(25rem, 90%)"}

## Mastertheorem

Seien $a \geq 1$ und $b > 1$ Konstanten. Sei $f(n)$ eine positive Funktion und $T(n)$ über den nicht-negativen ganzen Zahlen durch die Rekursionsgleichung

> $T(n)=aT(n/b) + f(n)$ , $T(1) = \Theta (1)$ 

definiert, wobei wir $n/b$ so interpretieren, dass damit entweder $\lceil n/b \rceil$ oder $\lfloor n/b \rfloor$ gemeint ist. Dann besitzt $T(n)$ die folgenden asymptotischen Schranken:

1. Gilt $f(n) = O(n^{\log_b {(a)}-\varepsilon})$ für eine Konstante $\varepsilon > 0$, dann $T(n) = \Theta (n ^{\log_b a})$

2. Gilt $f(n) = \Theta (n ^{\log_b a})$, dann gilt $T(n) = \Theta (n ^{\log_b a} \cdot \log_2 n)$

3. Gilt $f(n) = \Omega(n ^{\log_b {(a) + \varepsilon}})$ für eine Konstante $\varepsilon > 0$ und $af(n/b) \leq cf(n)$ für eine Konstante $c < 1$ und hinreichend große $n$, dann ist $T(n) = \Theta(f(n))$

### Beispiele

(a) $T(n)=3 \cdot T(\frac{n}{2})+n^2$ (für $n > 1$)

> Es sind $a=3\geq 1$ und $b=2>1$ konstant, und es gilt $f(n) = n^2 \geq 0$ für alle $n \in N$. 
Daraus folgt $\log_{b}{(a)} < 1,59 < 2$. Wir sind also im Fall 3 des Mastertheorems, denn $f(n) \in \Omega (n^{1,59 + \varepsilon })$ für ein $\varepsilon  > 0$ (z. B. mit $\varepsilon  > 1/10$), vorausgesetzt, dass die Regularitätsbedingung erfüllt ist. Diese gilt aber für $c = \frac{3}{4} < 1$ und alle $n \in N$ (da $3(\frac{n}{2})^2= \frac{3}{4} n^2$) und wir erhalten $T(n) \in \Theta(n^2)$

(b) $T(n)=\sqrt{2} \cdot T(\frac{n}{2})+\log{n}$ (für $n > 1$)

> Es sind $a = \sqrt{2} \geq 1$ und $b = 2 > 1$ konstant, und es
gilt $f(n) = \log{(n)} \geq 0$ für alle $n \in N$. Daraus folgt $\log_{b}{(a)} = \frac{1}{2}$. Wir sind also im Fall 1 des Mastertheorems denn $f(n) \in O(n^{1/2-\varepsilon})$ für ein $\varepsilon > 0$ (z. B. mit $\varepsilon = 1/10$), und wir erhalten $T(n) \in \Theta(\sqrt{n})$

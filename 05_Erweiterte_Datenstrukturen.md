# Erweiterte Datenstrukturen

## Rot-Schwarz-Baum

### Definition 
Ein Rot-Schwarz-Baum ist ein binärer Suchbaum, so dass gilt:

1. Jeder Knoten hat die Farbe rot oder schwarz,
2. Die Wurzel ist schwarz (sofern der Baum nicht leer),
3. Wenn ein Knoten rot ist, sind seine Kinder schwarz ("Nicht-Rot-Rot"-Regel),
4. Für jeden Knoten hat jeder Pfad im Teilbaum zu einem Blatt oder Halbblatt die gleiche Anzahl von schwarzen Knoten ("gleiche Anzahl schwarz")

Außerdem gilt, dass

- rote Knoten genau 0 oder 2 Kinder haben
- Halbblätter im RSB schwarz sind

### Beispiel

![Rot-Schwarz Baum Beispiel](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rot_schwarz_baum.png){ max-width="min(40rem, 90%)"}

### Schwarzhöhe

Die **Schwarzhöhe** $SH$ eines Knoten ``x`` ist die (eindeutige) Anzahl von schwarzen Knoten auf dem Weg zu einem Blatt oder Halbblatt im Teilbaum des Knoten.  
Für den leeren Baum gilt: 

$$SH(nil)=0$$

### Höhe eines Rot-Schwarz-Baums

Ein Rot-Schwarz-Baum mit $n$ Knoten hat maximale Höhe $h \leq 2 \cdot \log_2 {(n+1)}$.

### Algorithmen

```
insert(T,z):
    x=T.root; px=T.sent;
    WHILE x != nil DO
        px=x;
        IF x.key > z.key THEN
            x=x.left
        ELSE
            x=x.right;
    z.parent=px;
    IF px==T.sent THEN 
        T.root=z
    ELSE
        IF px.key > z.key THEN
            px.left=z
        ELSE
            px.right=z;
    z.color=red;
    fixColorsAfterInsertion(T,z);
```
**Laufzeit**: $\Theta (h)=O(\log{n})$

**Vorgehen**:
- Finde Elternknoten $y$ wie im BST
- Färbe neuen Knoten $z$ rot
- Stelle RS-Baum-Bedingungen wieder her

```
rotateRight(T,x):
    y=x.left;
    x.left=y.right;
    IF y.right != nil THEN
        y.right.parent=x;
    y.parent=x.parent;
    IF x.parent==T.sent THEN
        T.root=y
    ELSE
        IF x==x.parent.right THEN
            x.parent.right=y
        ELSE
            x.parent.left=y;
    y.right=x;
    x.parent=y;
```
**Laufzeit**: $\Theta(1)$

```
rotateLeft(T,x):
    // Analog zu rotateRight(T,x)
    // Tausche links und rechts
```
**Laufzeit**: $\Theta(1)$

![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rbt_rotate.png){ max-width="min(25rem, 90%)"}

```
fixColorsAfterInsertion(T,z):
    WHILE z.parent.color==red DO
        IF z.parent==z.parent.parent.left THEN
            y=z.parent.parent.right;
            IF y!=nil AND y.color==red THEN
                z.parent.color=black;
                y.color=black;
                z.parent.parent.color=red;
                z=z.parent.parent;
            ELSE
                IF z==z.parent.right THEN
                    z=z.parent;
                    rotateLeft(T,z);
                z.parent.color=black;
                z.parent.parent.color=red;
                rotateRight(T,z.parent.parent);
        ELSE
            // Analog zu vorherigem IF-Block
            // Tausche links und rechts
    T.root.color=black
```
**Laufzeit**: $O(h)=O(\log{n})$

**Vorgehen**:
- ``z.parent`` ist rot:
    - ``z.parent`` ist linker Knoten:
        - Bruder ``y`` von ``z.parent`` ist rot:
            - Farben entsprechend Z. 5-7 anpassen
            - ``z`` auf ``z.parent.parent`` setzen
        - Andernfalls:
            - ``z`` ist rechter Knoten:
                - ``z`` auf Elternknoten setzen
                - ``z`` nach links rotieren
            - Farben entsprechend Z. 13-14 anpassem
            - ``z.parent.parent`` nach rechts rotieren
    - Andernfalls:
        - Tausche "links" und "rechts"
- Wurzel schwarz einfärben

**Hinweis**: Der Einfachheit halber solltest du ``z`` immer im Baum einzeichnen/markieren!

![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rbt_fixColorsAfterInsertion.png){ width=100%, max-height=50rem }

**Schleifeninvariante**

1. `z.color==red`
2. Wenn `z.parent` Wurzel, dann `z.parent.color==black`
3. Wenn der aktuelle Baum kein Rot-Schwarz-Baum ist, dann weil `z` als Wurzel die Farbe rot hat, oder weil "Nicht-Rot-Rot-Regel" für `z`, `z.parent` verletzt ist.

```
delete(T,z):
    a=z.parent; dsh=nil; 
    IF z.left==z.right==nil THEN // z leaf
        IF z.color==black AND z!=T.root THEN
            IF z.parent.left==z THEN dsh=right ELSE dsh=left;
        transplant(T,z,nil);
    ELSE IF z.left==nil THEN // z half leaf
        y=z.right;
        transplant(T,z,z.right);
        y.color=z.color;
    ELSE IF z.right==nil THEN // z half leaf
        y=z.left;
        transplant(T,z,z.left);
        y.color=z.color;
    ELSE
        y=z.right; a=y; wentleft=false; 
        WHILE y.left != nil DO 
            a=y; y=y.left; wentleft=true;
        IF y.parent != z THEN
            transplant(T,y,y.right);
            y.right=z.right;
            y.right.parent=y;
        transplant(T,z,y);
        y.left=z.left;
        y.left.parent=y;
        IF y.color==black THEN
            IF wentleft THEN dsh=right ELSE dsh=left;
        y.color=z.color;
    IF dsh!=nil THEN fixColorsAfterDeletion(T,a,dsh);
```
**Laufzeit**: $O(h)=O(\log{n})$

**Vorgehen**: Beim Löschen der Wurzel wird das kleinste Element aus dem rechten Teilbaum bzw. das größte Element aus dem linken Teilbaum zur neuen Wurzel.


```
transplant(T,u,v):
    IF u.parent==T.sent THEN
        T.root=v
    ELSE
        IF u==u.parent.left THEN
            u.parent.left=v
        ELSE
            u.parent.right=v;
    IF v != nil THEN
        v.parent=u.parent;
```
**Laufzeit**: $\Theta (1)$  

```
fixColorsAfterDeletion(T,a,dsh):
    IF dsh==right THEN // extra black node on the right
        x=a.left; b=a.right; c=b.left; d=b.right;
        IF x!=nil AND x.color==red THEN
            x.color=black;
        ELSE IF a.color==black AND b.color==red THEN
            rotateLeft(T,a);
            a.color=red; b.color=black;
            fixColorsAfterDeletion(T,a,dsh);
        ELSE IF a.color==red AND b.color==black
                AND (c==nil OR c.color=black) 
                AND (d==nil OR d.color=black) THEN
            a.color=black; b.color=red;
        ELSE IF a.color==black AND b.color==black
                AND (c==nil OR c.color==black) 
                AND (d==nil OR d.color==black) THEN
            b.color=red;
            IF a==a.parent.left THEN dsh=left
            ELSE IF a==a.parent.right THEN dsh=right ELSE dsh=nil;
            fixColorsAfterDeletion(T,a.parent,dsh);
        ELSE IF b.color==black AND c!=nil AND c.color==red
                AND (d==nil OR d.color==black) THEN
            rotateRight(T,b);
            c.color=black; b.color=black;
            fixColorsAfterDeletion(T,a,dsh);
        ELSE IF b.color==black AND d!=nil AND d.color==red THEN
            rotateLeft(T,a);
            b.color=a.color; a.color=black; d.color=black;
    ELSE // dsh==left, extra black node on the left
        ... // exchange left and right
```

**Fälle**:

$a$ schwarz, $b$ rot
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rbt_fixColorsAfterDeletion_case1.png){ max-width="min(25rem, 90%)"}

$a$ rot, $b$ schwarz, $c, d$ nicht rot
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rbt_fixColorsAfterDeletion_case2a.png){ max-width="min(25rem, 90%)"}

$a$ schwarz, $b$ schwarz, $c, d$ nicht rot
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rbt_fixColorsAfterDeletion_case2b.png){ max-width="min(25rem, 90%)"}

$a$ beliebig, $b$ schwarz, $c$ rot, $d$ nicht rot
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rbt_fixColorsAfterDeletion_case3.png){ max-width="min(25rem, 90%)"}

$a$ beliebig, $b$ schwarz, $c$ beliebig, $d$ rot
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rbt_fixColorsAfterDeletion_case4.png){ max-width="min(25rem, 90%)"}

### Worst-Case Laufzeiten

Operation | Laufzeit
-|-
Einfügen | $\Theta(\log{n})$
Löschen | $\Theta(\log{n})$
Suchen | $\Theta(\log{n})$

### Vorteile gegenüber anderen binären Suchbäumen

**Bessere Laufzeit**: Rot-Schwarz-Bäume garantieren für alle Operationen (Suchen, Einfügen, Löschen) eine Laufzeit von $\Theta (log n)$. Durch die Restriktionen bleibt der Baum balanciert. Demzufolge können im Gegensatz zu herkömmlichen BSTs keine Links- bzw. Rechtslastigen Bäume entstehen. Deswegen gilt auch $h = O(log n)$.

## AVL-Baum

### Defintition

Ein AVL-Baum ist ein binärer Suchbaum, so dass für die Balance $B(x)$ in jede Knoten $x$ gilt: $B(x) \in \{-1,0,+1\}$.

Ein Baum gilt als **linkslastig**, wenn nur ein linkes Blatt bzw. als **rechtslastig**, wenn nur ein rechtes Blatt am Knoten hängt.

### Balance

$$B(x)=Höhe(\text{rechter Teilbaum})-Höhe(\text{linker Teilbaum})$$

### Höhe AVL-Baum

Ein AVL-Baum mit $n$ Knoten hat maximale Höhe $h \leq 1,441 \cdot \log_2{n}$.

Per Konvention gilt außerdem, dass die Höhe eines **leeren Baumes** $-1$ entspricht.

### AVL $\subset$ Rot-Schwarz

Jeder nicht-leere AVL-Baum der Höhe $h$ lässt sich als Rot-Schwarz-Baum mit Schwarzhöhe $\lceil \frac{h+1}{2} \rceil$ darstellen.

Es gilt allerdings auch AVL $\neq$ Rot-Schwarz, da es für jede Höhe $h \geq 3$ einen Rot-Schwarz-Baum gibt, der kein AVL-Baum ist. 

### Algorithmen

```
insert(T,z):
    x=T.root; 
    px=T.sent;
    WHILE x != nil DO
        px=x;
        IF x.key > z.key THEN
            x=x.left
        ELSE
            x=x.right;
    z.parent=px;
    IF px==T.sent THEN 
        T.root=z
    ELSE
        IF px.key > z.key THEN
            px.left=z
        ELSE
            px.right=z;
    fixBalanceAfterInsertion(T,z);
```
**Laufzeit**: $O(h)=O(\log{n})$

``fixBalanceAfterInsertion(T,z)``

1. Fall: $z$ ist ein rechtes Kind von $y$, und $y$ ist ein rechtes Kind von $x$ (Rechts-Rechts-Fall). Dann rotiere einmal nach rechts um den Knoten $x$.

![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rebalance_case1.png){ max-height=15rem }

2. Fall: $z$ ist ein linkes Kind von $y$, und $y$ ist ein rechtes Kind von $x$ (Links-Rechts-Fall). Dann wird eine Doppelrotation durchgeführt: Zuerst wird einmal nach links um den Knoten $y$, und anschließend einmal nach rechts um den Knoten $x$ rotiert.

![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rebalance_case2.png){ width=20rem }

3. Fall: $z$ ist ein linkes Kind von $y$, und $y$ ist ein linkes Kind von $x$ (Links-Links-Fall). Dann rotiere einmal nach
rechts um den Knoten $x$.

![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rebalance_case3.png){ max-height=15rem }

4. Fall: $z$ ist ein rechtes Kind von $y$, und $y$ ist ein linkes Kind von $x$ (Rechts-Links-Fall). Dann wird wieder eine Doppelrotation durchgeführt: Zuerst wird einmal nach rechts um den Knoten $y$, und anschließend einmal nach links um den Knoten $x$ rotiert.

![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/rebalance_case4.png){ max-height=15rem }

**Laufzeit**: $O(h)$, weil Rebalancieren nur einmal nötig

```
delete(T,z):
    // analog zum binaeren Suchbaum
    // rebalancieren (je nachdem aufsteigend)
```
**Laufzeit**: $O(h)=O(\log{n})$

### Worst-Case Laufzeiten

Operation | Laufzeit
-|-
Einfügen | $\Theta(\log{n})$
Löschen | $\Theta(\log{n})$
Suchen | $\Theta(\log{n})$

## AVL vs. Schwarz-Rot

AVL-Bäume sind geeigneter bei mehr Such-Operationen und weniger Einfüge- und Lösch-Operationen.

## String Matching

### Naiver Algorithmus

```
NaiveStringMatching(T,P):
    lenTxt = T.lenght; lenPat = P.length;
    L = [];
    FOR sft = 0 TO lenTxt - lenPat DO
        isValid = true;
        FOR j = 0 TO lenPat - 1 DO
            IF P[j] != T[sft + j] THEN
                isValid = false;
        IF isValid THEN
            L.Add(sft);
    return L;
```

### Rapin-Karb Algorithmus

```
RabinKarpMatchBasic(T, P):
    n = T.length; m = P.length; 
    h = 10^(m-1);
    p = 0; t0 = 0; L = [];
    FOR i = 0 TO m - 1 DO
        p = (10p + P[i]), t_0 = (10t_0 + T[i]);
    FOR s = 0 TO n - m DO
        IF p == t_s THEN
            L.Add(s);
        IF s < n - m THEN
            t_(s+1) = 10(t_s - T[s]h) + T[s + m];
    return L;
```

### FSM-Matching Algorithmus

```
FSMMatching(T,func,lenPat):
    n = T.length; L = []; p = 0;
    FOR s = 0 TO n - 1 DO
        p = func(p,T[s]);
        IF p == lenPat THEN
            L.Add(s - n + 1);
    return L
```

#### Beispiel

Existiere ein Automat mit Alphabet $\Sigma =\{a,e,m,r\}$ und Textmuster $P=[m,e,e,r]$.

![Automat](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/String_matching_automate.png){ max-width="min(40rem, 90%)"}

Nun sei $$T=[a, m, e, r, m, e, m, e, e, r, m, a, m, e, e, r, a]$$. Genau dann sieht eine Zustandstabelle wie folgt aus:

$i$ | $T[i]$ | Zustand | L
--- | ------ | ------- | -
$0$   | $a$ | $0$      | $[]$
$1$   | $m$ | $1$      | $[]$
$2$   | $e$ | $2$      | $[]$
$3$   | $r$ | $0$      | $[]$
$4$   | $m$ | $1$      | $[]$
$5$   | $e$ | $2$      | $[]$
$6$   | $m$ | $1$      | $[]$
$7$   | $e$ | $2$      | $[]$
$8$   | $e$ | $3$      | $[]$
$9$   | $r$ | $4$      | $[6]$
$10$  | $m$ | $1$      | $[6]$
$11$  | $a$ | $0$      | $[6]$
$12$  | $m$ | $1$      | $[6]$
$13$  | $e$ | $2$      | $[6]$
$14$  | $e$ | $3$      | $[6]$
$15$  | $r$ | $4$      | $[6,12]$
$16$  | $s$ | $0$      | $[6,12]$

## Splay-Baum

### Vorteil

Ist besonders schnell für Elemente, die mehrmals gesucht werden.

### Algorithmen

```
insert(T,z):
    // 1. Suche analog zum Einfuegen bei BST Einfuegepunkt
    // 2. Spuele neu eingefuegten Knoten per Splay-Operation an die Wurzel
```
**Laufzeit**: $O(h)$

```
search(T,k):
    x=T.root;
    WHILE x != nil AND x.key != k DO
        IF x.key < k THEN
            x=x.right
        ELSE
            x=x.left;
    
    IF x==nil THEN 
        return nil
    ELSE
        splay(T,x);
        return T.root
```
**Laufzeit**: $O(h)$
**Vorgehen**: Spüle gesuchte Knoten an die Wurzel

```
zigZig(T,z):
    IF z==z.parent.left THEN
        rotateRight(T,z.parent.parent);
        rotateRight(T,z.parent);
    ELSE
        rotateLeft(T,z.parent.parent);
        rotateLeft(T,z.parent);
```
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/splay_tree_zigZig.png){ max-width="min(30rem, 90%)"}

ist eine **Rechts-Rechts-** bzw. **Links-Links-Rotation**

```
zigZag(T,z):
    // analog zu zigZig
```
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/splay_tree_zigZag.png){ max-width="min(30rem, 90%)"}

Ist eine **Rechts-Links-** bzw. **Links-Rechts-Rotation**

```
zig(T,z):
    // analog zu zigZig
```
![](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/splay_tree_zig.png){ max-width="min(30rem, 90%)"}

ist eine **einfach Links-** bzw. **Rechts-Rotation**

```
splay(T,z):
    WHILE z != T.root DO
        IF z.parent.parent==nil THEN
            zig(T,z);
        ELSE
            IF z==z.parent.parent.left.left OR z==z.parent.parent.right.right THEN
                zigZig(T,z);
            ELSE
                zigZag(T,z);
```
**Laufzeit**: $O(h)$

```
delete(T,z)
// 1. Spuele z per Splay-Operation an die Wurzel
// 2. Loesche z
// 3. Spuele groessten Knoten y im linken Teilbaum per Splay-Operation an die Wurzel
// 4. Haenge rechten Teilbaum rechts an y an
```
**Laufzeit**: $O(h)$

### Laufzeit

Für $m \geq n$ Operationen auf einem Splay-Baum mit maximal $n$ Knoten ist die Worst-Case-Laufzeit $O(m \cdot \log_2{n})$, also $O(\log_2{n})$ pro Operation.

## Heaps

### Definition

Ein **binärer Max-Heap** ist ein binärer Baum, der

1. bis auf das unterste Level vollständig und im untersten Level von links gefüllt ist und
2. Für alle Knoten `x` $\neq$ `T.root` gilt:

> `x.parent.key` $\geq$ `x.key`

Bei **Min-Heaps** sind die Werte in den Elternknoten jeweils kleiner.

**Achtung**: **Heaps** sind **keine BSTs**, linke Kinder können größere Werte als rechte Kinder haben!

### Darstellung

![Array](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/heap1.png){ width="45%", max-width="min(30rem, 90%)"} ![Baum](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/heap2.png){ width="40%", max-width="min(25rem, 90%)"}

### Algorithmen

```
insert(H,k):
    H.size=H.size+1;
    H.A[H.size-1]=k;
    
    i=H.size-1;
    WHILE i>0 AND H.A[i] > H.A[i.parent]
        SWAP(H.A,i,i.parent);
        i=i.parent;
```
**Laufzeit**: $O(h)=O(\log{n})$

```
extract-max(H):
    IF isEmpty(H) THEN
        return error 'underflow'
    ELSE
        max=H.A[0];
        H.A[0]=H.A[H.size-1];
        H.size=H.size-1;
        heapify(H,0);
        return max;
```
**Laufzeit**: $O(h)=O(\log{n})$

```
heapify(H,i):
    maxind=i;
    IF i.left < H.size AND H.A[i] < H.A[i.left] THEN
        maxind=i.left;
    IF i.right < H.size AND H.A[maxind] < H.A[i.right] THEN
        maxind=i.right;

    IF maxind != i THEN
        SWAP(H.A,i,maxind);
        heapify(H,maxind);
```
**Laufzeit**: $O(h)=O(\log{n})$

```
buildHeap(H):
    // Array A schon nach H.A kopiert

    H.size=A.size;
    FOR i = ceil((H.size-1)/2)-1 DOWNTO 0 DO
        heapify(H,i);
```
**Laufzeit**: $O(n \cdot h)=O(n \cdot \log{n})$

**Beispiel**

![Anwendung von buildHeap](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/heap3.png)

```
heapSort(H):
    buildHeap(H);
    WHILE !isEmpty(H) DO 
        PRINT extract-max(H);
```
**Laufzeit**: $O(n \cdot h)=O(n \cdot \log{n})$

## Priority Queue

### Operationen

- `new(Q)`
erzeugt neue (leere) Priority Queue namens `Q`
- `isEmpty(Q)`
gibt an, ob Queue `Q` leer
- `max(Q)`
gibt größtes Element aus Queue `Q` zurück (bzw. Fehlermeldung, wenn Queue leer)
- `extract-max(Q)`
gibt größtes Element aus Queue `Q` zurück
und löscht es aus Queue (bzw. Fehlermeldung, wenn Queue leer)
- `insert(Q, k)`
fügt Wert `k` zu Queue `Q` hinzu

## B-Baum

### Definition

Ein B-Baum (vom Grad $t$) ist ein Baum, bei dem

1. Jeder Knoten (außer der Wurzel) zwischen $t-1$ und $2t-1$ Werte ``key[0], key[1], ...`` hat und die Wurzel zwischen $1$ und $2t-1$,
2. die Werte innerhalb eines Knoten aufsteigend geordnet sind,
3. die Blätter alle die gleiche Höhe haben, und
4. jeder innerer Knoten mit $n$ Werten $n+1$ Kinder hat, so dass für alle 
Werte $k_j$ aus dem $j$-ten Kind gilt:
$$k_0 \leq ``key[0]`` \leq k_1 \leq ``key[1]`` \leq \cdots \leq k_n-1 \leq ``key[n-1]`` \leq k_n$$

### Höhe B-Baum

Ein B-Baum vom Grad $t$ mit $n$ Werten 
hat maximale Höhe $h \leq \log_t{\frac{n+1}{2}}$.

### Algorithmen

```
search(x,k):
    WHILE x != nil DO
        i=0;
        WHILE i < x.n AND x.key[i] < k DO i=i+1;
            IF i < x.n AND x.key[i]==k THEN
                return (x,i);
            ELSE
            x=x.child[i];
    return nil;
```
**Laufzeit**: $O(t \cdot h)=O(\log_t{n})$

```
insert(T,z)
// Wenn Wurzel w schon 2t-1 Werte, dann split(T,w)
// Suche rekursiv Einfuegeposition:
    // Wenn zu besuchendes Kind k 2t-1 Werte, dann split(T,k)
// Fuege z in Blatt ein
```
**Laufzeit**: $O(t \cdot h)=O(\log_t{n})$

**Wichtig**: Denke an die Rekursion. Wenn durch Einfügen Elternknoten mehr als $2t-1$, dann rekursiv nach oben!

```
split(T,i):
    // Teile Blatt i in zwei Blaetter mit je t-1 Werten
    // Fuege mittleren Wert in Elternknoten von Blatt i ein
```

**Beispiel**

![B-Baum](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/b_tree-insert1.png){ width=45rem }

![B-Baum nach Hinzufügen von 45](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/b_tree-insert2.png){ width=45rem }

```
delete(T,k):
    // Wenn Wurzel nur 1 Wert und beide Kinder t-1 Werte, verschmelze Wurzel und Kinder (reduziert Hoehe um 1)
    // Suche rekursiv Loeschposition:
        // Wenn zu besuchendes Kind nur t-1 Werte, verschmelze es oder rotiere/verschiebe
    // Entferne Wert k in inneren Knoten/Blatt
```

**Beispiel**

![B-Baum](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/b_tree-delete1.png){ width=45rem }

![B-Baum nach Löschen von 4](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/b_tree-delete2.png){ width=45rem }

### Worst-Case Laufzeiten

Operation | Laufzeit
-|-
Einfügen | $\Theta(\log_t{n})$
Löschen | $\Theta(\log_t{n})$
Suchen | $\Theta(\log_t{n})$

**Achtung**: $O$-Notation versteckt $t$ für Suche innerhalb eines Knoten
Somit nur vorteilhaft, wenn blockweises einlesen

## Traversierung

### Preorder

**Schema**: Wurzel, linker Teilbaum, rechter Teilbaum

### Inorder

**Schema**: linker Teilbaum, Wurzel, rechter Teilbaum

### Postorder

**Schema**: linker Teilbaum, rechter Teilbaum, Wurzel

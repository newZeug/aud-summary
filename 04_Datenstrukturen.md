# Datenstrukturen

## Definition

Eine Datenstruktur ist eine Methode, Daten für den Zugriff und die Modifikation zu organisieren.

## Abstrakte Datentypen vs Datenstrukturen

Abstrakte Datentypen (ADTs) sind näher an der Anwendung während Datenstrukturen näher an der Maschine sind. 
Im Allgemeinen gilt, dass ADTs das *"was"* beschreiben, während Datenstrukturen das *"wie"* beschreiben.

## Stack

Funktioniert nach dem last-in-first-out Prinzip.

### Operationen

- `new(Stack)`
Erzeugt neuen Stack
- `isEmpty(Stack)`
Prüft ob der Stack leer ist 
- `pop(Stack)`
Gibt oberstes Element vom Stack zurück löscht es vom Stack (`Exception`, wenn Stack leer)
- `push(Stack, value)`
Fügt den Wert `value` als neues oberstes Element vom Stack hinzu (`Exception`, wenn Stack voll)

### Herausforderungen

Bei einfacher Feldarbeit wird von einer festen Größe ausgegangen. Das führt zu einer Laufzeit von $\Omega(n)$ pro Kopiervorgang. Um dies zu reduzieren, wird "unendlich" viel Speicher für die Stacks reserviert.

### Lösung

Wenn Grenze erreicht, verdopple Speicher und kopiere um. Schrumpfe und kopiere um, wenn weniger als $1/4$ des Stacks belegt. Damit liegt die Laufzeit im Schnitt bei $\Theta(1)$.

### Beispiel

Sei ein Stack `S` wie folgt initialisiert: 

\tilerow{3,5,4,}



**Operationen auf** `S`:

\begin{tabular}{lll}
1) & ``pop(S)`` & \tilerow{3,5,,} \\
2) & ``pop(S)`` & \tilerow{3,,,} \\
3) & ``push(S,1)`` & \tilerow{3,1,,} \\
4) & ``push(S,7)`` & \tilerow{3,1,7,} \\
\end{tabular}

## Queues

Funktioniert nach dem first-in-first-out Prinzip.

### Operationen

- `new(Queue)`
Erzeugt neue Queue
- `isEmpty(Queue)`
Prüft ob die Queue leer ist
- `dequeue(Queue)`
Gibt vorderstes Element der Queue zurück und entfernt es
(`Exception`, wenn Queue leer)
- `enqueue(Queue, value)`
Fügt den Wert `value` als neues hinterstes Element der Queue hinzu (`Exception`, wenn Queue voll)

### Beispiel

Sei eine Queue `Q` wie folgt initialisiert: 

\tilerow{1,2,5,}

**Operationen auf** `Q`:

\begin{tabular}{lll}
1) & ``dequeue(Q)`` & \tilerow{,2,5,} \\
2) & ``enqueue(Q,3)`` & \tilerow{,2,5,3} \\
3) & ``enqueue(Q,4)`` & \tilerow{4,2,5,3} \\
4) & ``dequeue(Q)`` & \tilerow{4,,5,3} \\
\end{tabular}

## Verkettete Listen

### Aufbau

Jede Liste verfügt über einen Kopf, auch `head` genannt. Dieser zeigt auf das erste Element der List.

Jedes der einzelnen Elemente verfügt über folgende Attribute:

- `key`: Wert
- `prev`: Zeigt auf Vorgänger (bzw. `nil`)
- `next`: Zeigt auf Nachfolger (bzw. `nil`)

## Einfach verkettete Listen

Es wird wenig Speicherplatz benötigt, da nur der Zeiger zum Nachfolger gespeichert werden muss. 
Außerdem ist das einfügen eines neuen Elements durch die einfache Verkettung mit weniger Aufwand verbunden.

### Algorithmen

```
reverseList(L):
    a=L.head;
    b=a.next;
    a.next=nil;
    WHILE b != nil DO
        t=b.next;
        b.next=a;
        a=b;
        b=t;
        L.head=a;
```
**Laufzeit**: $O(n)$  

## Doppelt verkettete Listen

Die Liste kann sowohl vorwärts als auch rückwärts durchlaufen werden.
Das Löschen eines Elementes befindet sich - wenn der Zeiger auf dem zu löschenden Element ist - in $O(1)$.

### Algorithmen

```
search(L,k):
    current=L.head;
    WHILE current != nil AND current.key != k DO
        current=current.next;
    return current;
```
**Laufzeit**: $\Theta (n)$

```
insert(L,x):
    x.next=L.head;
    x.prev=nil;
    IF L.head != nil THEN
        L.head.prev=x;
    L.head=x;
```
**Laufzeit**: $\Theta (1)$

```
delete(L,x):
    IF x.prev != nil THEN
        x.prev.next=x.next
    ELSE
        L.head=x.next;
    IF x.next != nil THEN
        x.next.prev=x.prev;
```
**Laufzeit**: $\Theta (1)$

```
deleteSent(L,x):
    x.prev.next=x.next;
    x.next.prev=x.prev;
```
**Laufzeit**: $\Theta (1)$  

## Binärer Baum

### Begrifflichkeiten

![Begrifflichkeiten](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/b-tree-begrifflichkeiten.png){ width="70%", max-width="min(30rem, 90%)"}

### Algorithmen

```
search(x,k):
    IF x==nil THEN 
        return nil;
    IF x.key==k THEN 
        return x;
    y=search(x.left,k);
    IF y != nil THEN 
        return y;
    return search(x.right,k);
```
**Laufzeit**: $\Theta (n)$  

```insert(T,x):
    IF T.root != nil THEN
        T.root.parent=x;
        x.left=T.root;
    T.root=x;
```
**Laufzeit**: $\Theta (1)$

**Vorgehen**: ``x`` wird die neue Wurzel und die vorherige Wurzel wird linkes Kind von ``x``

```
transplant(T,y,w):
    v=y.parent;
    IF y != T.root THEN
        IF y == v.right THEN
            v.right=w;
        ELSE
            v.left=w;
    ELSE
        T.root=w;
    IF w != nil THEN
        w.parent=v;
```
**Laufzeit**: $\Theta (1)$

```
delete(T,x):
    y=T.rooz; 
    WHILE y.right!=nil DO
        y=y.right;
    transplant(T,y,y.left);
    IF x != y THEN 
        y.left=x.left;
        IF x.left != nil THEN
            x.left.parent=y;
        y.right=x.right;
        IF x.right != nil THEN
            x.right.parent=y;
        transplant(T,x,y);
```
**Laufzeit**: $\Theta (h)$, wobei h Höhe des Baumes

**Vorgehen**: 
 
Nehme rechtesten Knoten von Wurzel ``r``, verbinde Kinder von ``r`` mit ``r.parent`` und ersetze ``x`` durch ``r``.

## Binärer Suchbaum

### Algorithmen

```
search(x,k):
    // 1.Aufruf x=root

    IF x==nil OR x.key==k THEN
        return x;
    IF x.key > k THEN
        return search(x.left,k)
    ELSE
        return search(x.right,k);
```
**Laufzeit**: $\Theta (h)$, wobei h Höhe des Baumes

```
iterative-search(x,k):
    // Aufruf x=root

    WHILE x != nil AND x.key != k DO
        IF x.key > k THEN
            x=x.left
        ELSE
            x=x.right;
    return x;
```
**Laufzeit**: $\Theta (h)$, wobei h Höhe des Baumes

```
insert(T,z):
    // may insert z again
    // z.left==z.right==nul;
    
    x=T.root; px=nil;

    WHILE x != nil DO
        px=x;
        IF x.key > z.key THEN
            x=x.left
        ELSE
            x=x.right;
    z.parent=px;
    IF px==nil THEN 
        T.root=z
    ELSE
        IF px.key > z.key THEN
            px.left=z
        ELSE
            px.right=z;
```
**Laufzeit**: $\Theta (h)$, wobei h Höhe des Baumes

```
transplant(T,u,v):
    IF u.parent==nil THEN
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
delete(T,z):
    IF z.left==nil THEN
        transplant(T,z,z.right)
    ELSE
        IF z.right==nil THEN
            transplant(T,z,z.left)
        ELSE
            y=z.right;
            WHILE y.left != nil DO
                y=y.left;
            IF y.parent != z THEN
                transplant(T,y,y.right);
                y.right=z.right;
                y.right.parent=y;
            transplant(T,z,y);
            y.left=z.left;
            y.left.parent=y;
```
**Laufzeit**: $O(h)$

**Vorgehen**:

- Knoten ``z`` hat nur ein Kind: Ersetze ``z`` durch sein Kind.
- Knoten ``z`` ist ein Blatt: Knoten ``z`` löschen.
- Knoten ``z`` hat zwei Kinder:
    - ``z.right`` hat kein linkes Kind: ``z`` durch ``z.right`` ersetzen
    - Andernfalls: Ersetze ``y``, den linkesten Knoten von ``z.right``, durch ``y.right`` und ``z`` durch ``y``.

### Beispiel

Der binären Suchbaum mit $BS=\{14,24,20,23,21,22\}$, sieht wie folgt aus:

![Binärer Suchbaum Beispiel](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/binary_searchtree.png){ width=90% }

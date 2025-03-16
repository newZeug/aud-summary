# Randomisierte Datenstrukturen

## Skip Lists

**Idee**: Arbeitet mit Express-Listen. Diese enthält nicht alle Elemente aus der ürsprünglichen Liste, sondern nur einen Teil.

### Implementierung

- `L.head` Erstes/oberstes Element der Liste
- `L.height` Höhe der Skiplist
- `x.key` Wert
- `x.next` Nachfolger
- `x.prev` Vorgänger
- `x.down` Nachfolger Liste unten
- `x.up` Nachfolger Liste oben
- `nil` kein Nachfolger / leeres Element

### Worst-Case Laufzeiten

Operation | Laufzeit
-|-
Einfügen | $\Omega(n)$
Löschen | $\Omega(n)$
Suchen | $\Omega(n)$

### Algorithmen

```
search(L,k):
    current=L.head;
    WHILE current != nil DO
        IF current.key == k THEN 
            return current; 
        IF current.next != nil AND current.next.key <= k THEN 
            current=current.next
        ELSE 
            current=current.down;
    return nil;
```
**Beachte**: Im schlimmsten Fall wird Suche in ursprünglicher Liste beendet.

```
insert(L,k):
    // Folge Suchpfad von k
    // Erstelle Element E mit Wert k und fuege es in die unterste Liste ein
    // Solange Zufallszahl z<p ist:
        // Gehe eine Liste nach oben
        // Fuege k dort hinzu
```

### Laufzeiten

Operation | Laufzeit
-|-
Einfügen | $\Theta(\log_{1/p}{n})$
Löschen | $\Theta(\log_{1/p}{n})$
Suchen | $\Theta(\log_{1/p}{n})$

### Speicherbedarf
Im Durchschnitt $\frac{n}{1-p}$

### Beispiel

![Beispiel Skip List](https://raw.githubusercontent.com/newZeug/aud-summary/refs/heads/main/utils/skip_list.png){ max-width="min(45rem, 90%)"}

## Bloom-Filter

### Definition

- $n$ Elemente $x_0,...,x_{n-1}$ beliebiger Komplexität
- $m$ Bits Speicher, üblicherweise in einem Bit-Array
- $k$ "gute" Hash-Funktionen $H_0,...H_{k-1}$ mit Bildbereich $0,1,...,m-1$
    - empfohlene Wahl: $k=\frac{m}{n} \cdot \ln{2}$
- Es gibt keine "false negatives"
    - Negatives Ergebnis sagt aus, dass das Element in keinem Fall vorhanden ist.
    - Positives Ereignis sagt aus, dass das Element wahrscheinlich vorhanden ist. Es kann sich allerdings um ein false-positive handeln, d.h. das Element wurde also evtl. gar nicht hinzugefügt.

### Algorithmen

```
initBloom(X,BF,H):
    // H array of functions H[j]

    FOR i=0 TO BF.length-1 DO BF[i]=0;
    FOR i=0 TO X.length-1 DO
        FOR j=0 TO H.length-1 DO
            BF[H[j](X[i])]=1;
```

```
searchBloom(BF,H,y):
    // H array of functions H[j]

    result=1;
    FOR j=0 TO H.length-1 DO
        result=result AND BF[H[j](y)];
    return result;
```
**Laufzeit**: $O(1)$

```
bloomUnion(BF1, BF2):
    BF = [];
    FOR i=0 TO n-1 DO
        IF BF1[i] == 1 OR BF2[i] == 1 THEN
            BF[i] = 1
        ELSE
            BF[i] = 0;
    return BF;
```

### Löschproblem

Das Löschen eines Eintrages ist nicht ohne weiteres möglich da man ggfs. auch andere Einträge mit dem Löschen eines Elements löschen würde. 
Deshalb sollte man den Counting Bloom-Filter verwenden. Dieser speichert nicht Einsen sondern inkrementiert die Einträge. 

## Hash Tables

### Definition

Verteilt auf einem Array Werte mithilfe einer Hashfunktion. Diese Hashfunktion sollte möglichst gut verteilen (also wenig Kollisionen).

#### Kollisionsauflösung

Bilde verkettete Liste und füge neues Element **vorne** ein.

#### Gute Hashfunktionen

Es werden am meisten **SHA-2** oder **SHA-3** verwendet.

### Mit verketteten Listen

- Einfügen immer noch konstante Anzahl Array-/Listen-Operation
- Suchen/Löschen benötigen so viele Schritte, wie jeweilige Liste lang ist
- Wenn Hashfunktion uniform verteilt, dann hat jede Liste im Erwartungswert $\frac{n}{T.length}$ viele Einträge

### Laufzeiten
Operation | Laufzeit
-|-
Einfügen | $\Theta(\log{1})$ - Sogar Worst-Case
Löschen | $\Theta(\log{1})$
Suchen | $\Theta(\log{1})$

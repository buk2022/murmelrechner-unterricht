# Arbeitsblatt 2: Beispiele und Übungen für den Murmelrechner

## Beispiel 1: Addition 90 + 45 = 135

### **Aufgabe mit dem Murmelrechner:**
- Gefäß 0: wird leer (enthält anfangs 90)
- Gefäß 1: wird leer (enthält anfangs 45)
- Gefäß 2: wird unser Ergebnis (leer am Anfang)
- **Ziel:** Alle Murmeln aus Gefäß 0 und 1 nach Gefäß 2 bringen

### **Das Programm:**

```
Adr. | Befehl | Operand | Erklärung
─────┼────────┼─────────┼─────────────────────────────────────
  0  │  tst   │    0    │ TEST: Sind noch Murmeln in Gefäß 0?
  1  │  jmp   │    3    │ WENN JA: Springe zu Adresse 3
  2  │  jmp   │    6    │ WENN NEIN: Springe zu Adresse 6
  3  │  dec   │    0    │ Entferne 1 Murmel aus Gefäß 0
  4  │  inc   │    2    │ Füge 1 Murmel zu Gefäß 2 hinzu
  5  │  jmp   │    0    │ Springe zurück zu Adresse 0
  6  │  tst   │    1    │ TEST: Sind noch Murmeln in Gefäß 1?
  7  │  jmp   │    9    │ WENN JA: Springe zu Adresse 9
  8  │  jmp   │   12    │ WENN NEIN: Springe zu Adresse 12
  9  │  dec   │    1    │ Entferne 1 Murmel aus Gefäß 1
 10  │  inc   │    2    │ Füge 1 Murmel zu Gefäß 2 hinzu
 11  │  jmp   │    6    │ Springe zurück zu Adresse 6
 12  │  hlt   │    -    │ STOP - Programm beendet
```

### **Schritt-für-Schritt Ausführung:**

#### **Startposition:**
```
Gefäß 0: 90 Murmeln    (zu verarbeiten)
Gefäß 1: 45 Murmeln    (zu verarbeiten)
Gefäß 2:  0 Murmeln    (Ergebnis)
Flag: --
```

#### **Phase 1: Gefäß 0 leeren (Adressen 0-5)**

| Schritt | Steuermann | Rechenkünstler | Laufbursche | Flag | Gefäß 0 | Gefäß 1 | Gefäß 2 |
|---------|-----------|-----------------|------------|------|---------|---------|---------|
| Start   | - | - | - | - | 90 | 45 | 0 |
| 1. | Befehl 0: tst 0 lesen | "Teste Gefäß 0" | Geht zu Gefäß 0, schaut rein | TRUE | 90 | 45 | 0 |
| 2. | Befehl 1: jmp 3 (Flag=TRUE) | - | - | TRUE | 90 | 45 | 0 |
| 3. | Springe zu Adresse 3 | - | - | TRUE | 90 | 45 | 0 |
| 4. | Befehl 3: dec 0 lesen | "Entferne 1 Murmel" | Nimmt 1 Murmel aus Gefäß 0 | TRUE | 89 | 45 | 0 |
| 5. | Befehl 4: inc 2 lesen | "Füge 1 Murmel hinzu" | Bringt Murmel zu Gefäß 2 | TRUE | 89 | 45 | 1 |
| 6. | Befehl 5: jmp 0 | - | - | TRUE | 89 | 45 | 1 |
| 7. | Springe zu Adresse 0 | - | - | TRUE | 89 | 45 | 1 |

**→ Schleife wiederholt sich 90x**

```
Nach Wiederholung 90x:
Gefäß 0:  0 Murmeln    ✓ LEER
Gefäß 1: 45 Murmeln
Gefäß 2: 90 Murmeln
```

#### **Phase 2: Gefäß 1 leeren (Adressen 6-11)**

| Schritt | Steuermann | Aktion | Flag | Gefäß 0 | Gefäß 1 | Gefäß 2 |
|---------|-----------|--------|------|---------|---------|---------|
| 1. | Befehl 6: tst 1 | Test Gefäß 1 | TRUE | 0 | 45 | 90 |
| 2. | Befehl 7: jmp 9 (Flag=TRUE) | Springe zu 9 | TRUE | 0 | 45 | 90 |
| 3. | Befehl 9: dec 1 | Entferne 1 Murmel | TRUE | 0 | 44 | 90 |
| 4. | Befehl 10: inc 2 | Füge zu Gefäß 2 | TRUE | 0 | 44 | 91 |
| 5. | Befehl 11: jmp 6 | Springe zu 6 | TRUE | 0 | 44 | 91 |

**→ Schleife wiederholt sich 45x**

```
Nach Wiederholung 45x:
Gefäß 0:  0 Murmeln    ✓ LEER
Gefäß 1:  0 Murmeln    ✓ LEER
Gefäß 2: 135 Murmeln   ✓ ERGEBNIS
```

#### **Phase 3: Programmende**

| Schritt | Steuermann | Aktion |
|---------|-----------|--------|
| 1. | Befehl 6: tst 1 | Test Gefäß 1 - LEER! Flag=FALSE |
| 2. | Befehl 7: jmp 9 | Flag=FALSE → Springe NICHT, gehe zu 8 |
| 3. | Befehl 8: jmp 12 | Springe zu 12 |
| 4. | Befehl 12: hlt | **STOPP - PROGRAMM BEENDET** |

### **ERGEBNIS: 135** ✓

---

## Beispiel 2: Wert verdoppeln (90 → 180)

### **Aufgabe:**
- Gefäß 0: enthält 90 (zu verdoppeln)
- Gefäß 1: wird leer (Arbeitsregister)
- Gefäß 2: wird das Ergebnis (2 × 90 = 180)

### **Idee:**
Gefäß 0 zweimal zu Gefäß 2 hinzufügen!

### **Das Programm:**

```
Adr. | Befehl | Operand | Erklärung
─────┼────────┼─────────┼─────────────────────────────────────
  0  │  tst   │    0    │ TEST: Sind Murmeln in Gefäß 0?
  1  │  jmp   │    3    │ WENN JA: Gehe zu 3
  2  │  jmp   │    9    │ WENN NEIN: Gehe zu 9 (Ende Phase 1)
  3  │  dec   │    0    │ Entferne aus Gefäß 0
  4  │  inc   │    1    │ Füge zu Arbeitsgefäß 1
  5  │  inc   │    2    │ Füge auch zu Ergebnis Gefäß 2
  6  │  jmp   │    0    │ Springe zurück zu 0
  
  7  │  (leer)│ (leer)  │ (reserviert)
  8  │  (leer)│ (leer)  │ (reserviert)
  
  9  │  tst   │    1    │ TEST: Sind Murmeln in Gefäß 1?
 10  │  jmp   │   12    │ WENN JA: Gehe zu 12
 11  │  jmp   │   15    │ WENN NEIN: Gehe zu 15 (Ende)
 12  │  dec   │    1    │ Entferne aus Gefäß 1
 13  │  inc   │    2    │ Füge nochmal zu Gefäß 2
 14  │  jmp   │    9    │ Springe zurück zu 9
 15  │  hlt   │    -    │ STOP
```

### **Ausführung - Vereinfacht:**

```
START:
Gefäß 0: 90    Gefäß 1: 0     Gefäß 2: 0

Phase 1 (Adr. 0-6): Leere Gefäß 0, verteile auf 1 und 2
  ↓ 90x wiederholen: dec 0, inc 1, inc 2

Nach Phase 1:
Gefäß 0: 0     Gefäß 1: 90    Gefäß 2: 90

Phase 2 (Adr. 9-14): Leere Gefäß 1, füge zu 2
  ↓ 90x wiederholen: dec 1, inc 2

ENDE:
Gefäß 0: 0     Gefäß 1: 0     Gefäß 2: 180 ✓
```

---

# ÜBUNGEN FÜR SCHÜLER

## Übung 1: Umladen (Transfer)

### **Aufgabe:**
Übertrage alle Murmeln von Gefäß 0 nach Gefäß 2 (ohne Veränderung der Anzahl)

**Startposition:**
```
Gefäß 0: 50 Murmeln
Gefäß 2:  0 Murmeln
```

**Geforderte Endposition:**
```
Gefäß 0:  0 Murmeln
Gefäß 2: 50 Murmeln
```

### **Schreibe das Programm:**

| Adr. | Befehl | Operand |
|------|--------|---------|
| 0 | | |
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

### **LÖSUNG:**

```
Adr. | Befehl | Operand | Erklärung
─────┼────────┼─────────┼──────────────────────────────
  0  │  tst   │    0    │ Teste: Murmeln in 0?
  1  │  jmp   │    3    │ JA → Gehe zu 3
  2  │  jmp   │    5    │ NEIN → Programmende
  3  │  dec   │    0    │ Entferne aus 0
  4  │  inc   │    2    │ Füge zu 2
  5  │  hlt   │    -    │ STOP
```

**Ausführung:**
- Loop (0→1→3→4): 50x wiederholen
- Jedes Mal: -1 von Gefäß 0, +1 zu Gefäß 2
- Am Ende: Gefäß 0=0, Gefäß 2=50 ✓

---

## Übung 2: Tausch mit Hilfs-Gefäß

### **Aufgabe:**
Tausche die Werte zwischen Gefäß 0 und Gefäß 1

**Startposition:**
```
Gefäß 0: 30 Murmeln
Gefäß 1: 60 Murmeln
```

**Geforderte Endposition:**
```
Gefäß 0: 60 Murmeln
Gefäß 1: 30 Murmeln
```

**Hinweis:** Benutze Gefäß 2 als Zwischenspeicher!

### **Schreibe das Programm:**

| Adr. | Befehl | Operand |
|------|--------|---------|
| 0-20 | ? | ? |

### **LÖSUNG:**

```
Adr. | Befehl | Operand | Erklärung
─────┼────────┼─────────┼────────────────────────���─────
  0  │  tst   │    0    │ Phase 1: Gefäß 0 → Gefäß 2
  1  │  jmp   │    3    │ JA
  2  │  jmp   │    6    │ NEIN
  3  │  dec   │    0    │
  4  │  inc   │    2    │
  5  │  jmp   │    0    │
  
  6  │  tst   │    1    │ Phase 2: Gefäß 1 → Gefäß 0
  7  │  jmp   │    9    │ JA
  8  │  jmp   │   12    │ NEIN
  9  │  dec   │    1    │
 10  │  inc   │    0    │
 11  │  jmp   │    6    │
 
 12  │  tst   │    2    │ Phase 3: Gefäß 2 → Gefäß 1
 13  │  jmp   │   15    │ JA
 14  │  jmp   │   18    │ NEIN
 15  │  dec   │    2    │
 16  │  inc   │    1    │
 17  │  jmp   │   12    │
 
 18  │  hlt   │    -    │ STOP
```

**Ausführung in 3 Phasen:**
```
START:    Gef.0=30  Gef.1=60  Gef.2=0

Phase 1:  Gef.0=0   Gef.1=60  Gef.2=30  (0→2)
Phase 2:  Gef.0=60  Gef.1=0   Gef.2=30  (1→0)
Phase 3:  Gef.0=60  Gef.1=30  Gef.2=0   (2→1)

ENDE: Tausch vollzogen! ✓
```

---

## Übung 3: Summe von drei Zahlen

### **Aufgabe:**
Addiere drei Zahlen und speichere das Ergebnis:
- Gefäß 0: 25 Murmeln
- Gefäß 1: 35 Murmeln
- Gefäß 2: 40 Murmeln
- **Gefäß 3: Summe (?)** 

**Geforderes Ergebnis:** Gefäß 3 = 100

### **Schreibe das Programm:**

Hinweis: Du brauchst 3 Phasen (je eine für jedes Gefäß)

### **LÖSUNG:**

```
Adr. | Befehl | Operand | Erklärung
─────┼────────┼─────────┼──────────────────────────────
  0  │  tst   │    0    │ Phase 1: Gefäß 0 → Gefäß 3
  1  │  jmp   │    3    │
  2  │  jmp   │    6    │
  3  │  dec   │    0    │
  4  │  inc   │    3    │
  5  │  jmp   │    0    │
  
  6  │  tst   │    1    │ Phase 2: Gefäß 1 → Gefäß 3
  7  │  jmp   │    9    │
  8  │  jmp   │   12    │
  9  │  dec   │    1    │
 10  │  inc   │    3    │
 11  │  jmp   │    6    │
 
 12  │  tst   │    2    │ Phase 3: Gefäß 2 → Gefäß 3
 13  │  jmp   │   15    │
 14  │  jmp   │   18    │
 15  │  dec   │    2    │
 16  │  inc   │    3    │
 17  │  jmp   │   12    │
 
 18  │  hlt   │    -    │ STOP
```

**Ausführung:**
```
START:   0=25   1=35   2=40   3=0
Ph.1:    0=0    1=35   2=40   3=25
Ph.2:    0=0    1=0    2=40   3=60
Ph.3:    0=0    1=0    2=0    3=100 ✓
```

---

## Übung 4: "Weniger oder gleich" überprüfen

### **Aufgabe:**
Vergleiche zwei Zahlen:
- Gefäß 0: 45 Murmeln
- Gefäß 1: 60 Murmeln

**Frage:** Ist Gefäß 0 ≤ Gefäß 1?

**Ergebnis markieren:**
- Wenn JA: Gefäß 2 bekommt 1 Murmel (TRUE)
- Wenn NEIN: Gefäß 2 bleibt leer (FALSE)

### **Strategisches Vorgehen:**
1. Kopiere Gefäß 0 in Arbeitsregister
2. Subtrahiere Gefäß 1 davon
3. Wenn das Ergebnis ≥ 0, war die Frage FALSCH
4. Wenn das Ergebnis negativ wäre, war die Frage WAHR

*Hinweis für diese Übung: Wir verwenden eine "Hilfsmethode":*
- Versuche, Murmeln aus Gefäß 1 zu nehmen, während du Gefäß 0 leerst
- Wenn Gefäß 0 zuerst leer ist → JA (≤), markiere mit inc 2

### **LÖSUNG (Vereinfachte Variante):**

```
Adr. | Befehl | Operand | Erklärung
─────┼────────┼─────────┼──────────────────────────────
  0  │  tst   │    0    │ Teste Gefäß 0
  1  │  jmp   │    3    │ JA - gehe zu 3
  2  │  jmp   │    9    │ NEIN - Gefäß 0 ist leer, also ≤
  
  3  │  dec   │    0    │ Verringere Gefäß 0
  4  │  dec   │    1    │ Verringere Gefäß 1
  5  │  tst   │    1    │ Teste Gefäß 1
  6  │  jmp   │    0    │ JA - noch vorhanden, schleife weiter
  7  │  jmp   │    8    │ NEIN - Gefäß 1 leer zuerst!
  
  8  │  inc   │    2    │ Markiere als FALSE
  9  │  hlt   │    -    │ STOP
```

**Ausführung für 45 ≤ 60 (TRUE):**
```
Loop (0-4): 45x wiederholen
  - dec 0: 45→44→...→0
  - dec 1: 60→59→...→15

Nach Loop: 0=0, 1=15 (noch Murmeln da!)
→ tst 1 = TRUE
→ Springe zu 0, weiterschleifen

Nach 15 weiteren: 0=0, 1=0
→ tst 1 = FALSE
→ Gehe zu 8: inc 2
→ Gefäß 2 = 1 (TRUE) ✓
```

---

## Übung 5: Zähle Durchläufe (Schleifenzähler)

### **Aufgabe:**
Wie oft laufen wir durch eine Schleife?

- Gefäß 0: 8 Murmeln (Schleifenanzahl)
- Gefäß 1: wird unser Zähler (0 Murmeln am Anfang)

**Gefordertes Ergebnis:** Gefäß 1 = 8 (Wir sind 8x durch die Schleife gelaufen)

### **LÖSUNG:**

```
Adr. | Befehl | Operand | Erklärung
─────┼────────┼─────────┼──────────────────────────────
  0  │  tst   │    0    │ Solange Gefäß 0 nicht leer
  1  │  jmp   │    3    │
  2  │  jmp   │    6    │
  
  3  │  dec   │    0    │ Verringere Zähler
  4  │  inc   │    1    │ Erhöhe Durchlauf-Counter
  5  │  jmp   │    0    │ Zurück zur Prüfung
  
  6  │  hlt   │    -    │ STOP
```

**Ausführung:**
```
Durchlauf 1: 0=8 → dec → 0=7, inc → 1=1
Durchlauf 2: 0=7 → dec → 0=6, inc → 1=2
...
Durchlauf 8: 0=1 → dec → 0=0, inc → 1=8
Schleife endet: 0=0 → tst FALSE → jmp zu 6 → hlt

ERGEBNIS: 1=8 ✓
```

---

## Übungs-Arbeitsblatt für Schülergruppen

### **Gruppe 1 (Anfänger):**
- Übung 1: Umladen
- Übung 5: Schleifenzähler
- Kontrolliert gegenseitig die Lösungen

### **Gruppe 2 (Mittelstufe):**
- Übung 2: Tausch mit Hilfsgefäß
- Übung 3: Summe von drei Zahlen
- Vergleicht Ergebnisse

### **Gruppe 3 (Fortgeschrittene):**
- Übung 4: Vergleich ≤
- Entwickelt eigene Erweiterung: Was würde > aussehen?

---

## Beobachtungsaufgaben für alle:

1. **Wie viele Befehle braucht die Addition?** (Antwort: 13 Befehle)
2. **Warum gibt es zwei jmp-Befehle nacheinander (Adr. 1-2)?**
3. **Was passiert, wenn wir Gefäß 1 mit nur 10 Murmeln füllen?**
4. **Kann man das Programm schneller schreiben?** (Tipp: Kombination von Gefäßen)

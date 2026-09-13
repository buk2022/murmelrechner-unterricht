# Arbeitsblatt 1: Der Murmelrechner - Architektur, Befehlssatz und Akteure

## Das Murmelrechner-Modell: Von-Neumann-Architektur mit Leben

Der Murmelrechner visualisiert die Von-Neumann-Architektur durch ein funktionales Modell mit drei Akteuren:

### **Komponenten des Systems:**

#### **1. Der Speicher**
- **Gefäße** (durchnummeriert: 0, 1, 2, 3, ...)
- **Inhalt:** Murmeln (Anzahl = Datenwert)
- **Funktion:** Speichert Daten und das Programm

#### **2. Die drei Akteure:**

| Akteur | Rolle | Aufgaben |
|--------|-------|----------|
| **Steuermann** | Programm-Kontrolle | • Liest Befehle aus dem Speicher<br>• Entscheidet, welcher Befehl als nächster ausgeführt wird<br>• Leitet die anderen Akteure an<br>• Führt Sprünge durch (jmp-Befehle)<br>• Beendet das Programm (hlt) |
| **Rechenkünstler** | Daten-Verarbeitung | • Führt Rechenoperationen durch<br>• Führt Tests durch (tst)<br>• Speichert Testergebnis ("ist leer?" ja/nein)<br>• Führt inc (Inkrement) durch<br>• Führt dec (Dekrement) durch |
| **Laufbursche** | Transport | • Transportiert Murmeln vom Speicher zum Rechenkünstler<br>• Transportiert Murmeln vom Rechenkünstler zurück in den Speicher<br>• Folgt den Anweisungen des Rechenkünstlers |

---

## Der Befehlssatz des Murmelrechners

```
Programmspeicher (Adressen 0, 1, 2, 3, ...)
│
├─ Adresse | Befehl | Operand
├─────────┼────────┼─────────
│    0    │  tst   │   1     ← Teste Gefäß 1
│    1    │  jmp   │   3     ← Springe zu Adresse 3
│    2    │  jmp   │   6     ← Springe zu Adresse 6
│    3    │  dec   │   1     ← Dekrementiere Gefäß 1
│    4    │  inc   │   0     ← Inkrementiere Gefäß 0
│    5    │  jmp   │   0     ← Springe zu Adresse 0
│    6    │  hlt   │   -     ← Halt (Programmende)
```

### **Befehlssätze im Detail:**

#### **1. TST (Test) - Befehl**
```
tst <Adresse>
```
**Was macht der Steuermann:**
- Befiehlt dem Laufbursche: "Geh zu Gefäß <Adresse> und schau nach!"

**Was macht der Laufbursche:**
- Geht zum angegebenen Gefäß
- Prüft: "Sind noch Murmeln drin?"

**Was macht der Rechenkünstler:**
- Empfängt die Info vom Laufbursche
- **Wenn Gefäß leer:** Setzt Flag auf "0" (FALSE)
- **Wenn Murmeln drin:** Setzt Flag auf "1" (TRUE)

**Steuermann merkt sich:** "Letzter Test war TRUE/FALSE"

---

#### **2. JMP (Jump/Sprung) - Befehl**
```
jmp <Adresse>
```
**Was macht der Steuermann:**
- **BEDINGTER Sprung:** Nur wenn der letzte Test TRUE war (Gefäß nicht leer)
- Springt zur angegebenen Adresse
- Liest von dort den nächsten Befehl

**Wichtig:** Wenn der letzte Test FALSE war, wird der Sprung ignoriert!
- Der Steuermann liest dann einfach den nächsten Befehl von Adresse+1

---

#### **3. DEC (Dekrementieren) - Befehl**
```
dec <Adresse>
```
**Was macht der Steuermann:**
- Befiehlt dem Rechenkünstler: "Entferne eine Murmel aus Gefäß <Adresse>"

**Was macht der Laufbursche:**
- Geht zu Gefäß <Adresse>
- Nimmt 1 Murmel heraus
- Bringt sie zum Rechenkünstler

**Was macht der Rechenkünstler:**
- Nimmt die Murmel entgegen
- "Verbraucht" sie (wirft sie weg oder legt sie beiseite)

---

#### **4. INC (Inkrementieren) - Befehl**
```
inc <Adresse>
```
**Was macht der Steuermann:**
- Befiehlt dem Rechenkünstler: "Füge eine Murmel zu Gefäß <Adresse> hinzu"

**Was macht der Rechenkünstler:**
- Nimmt 1 Murmel von seinem Platz (oder nimmt eine vorgefasste Murmel)

**Was macht der Laufbursche:**
- Empfängt die Murmel vom Rechenkünstler
- Bringt sie zu Gefäß <Adresse>
- Legt sie hinein

---

#### **5. HLT (Halt) - Befehl**
```
hlt
```
**Was macht der Steuermann:**
- Beendet die Programm-Ausführung
- "Stopp! Niemand bewegt sich mehr!"

---

## Zusammenfassung: Der Ablauf eines Programms

```
START
  ↓
Steuermann liest nächsten Befehl
  ↓
┌─────────────────────────────────────┐
│ BEFEHLSTYP?                         │
├─────────────────────────────────────┤
│ tst → Laufbursche/Rechenkünstler    │
│        prüfen Gefäß (Flag setzen)   │
│                                     │
│ jmp → Wenn Flag=1: Springe          │
│       Wenn Flag=0: Nächster Befehl  │
│                                     │
│ dec → Laufbursche/Rechenkünstler    │
│        entfernen 1 Murmel aus Gefäß │
│                                     │
│ inc → Rechenkünstler/Laufbursche    │
│        fügen 1 Murmel zu Gefäß hinzu│
│                                     │
│ hlt → ENDE                          │
└─────────────────────────────────────┘
  ↓
Flag=0 (FALSE)? → nächste Adresse + 1
Flag=1 (TRUE)?  → abhängig vom Befehl
  ↓
BACK TO START
```

---

## Praktische Durchführung im Unterricht

### **Rollen-Verteilung (4er-Gruppe):**
1. **Steuermann** - koordiniert das Programm
2. **Rechenkünstler** - führt Tests und Berechnungen durch
3. **Laufbursche** - transportiert Murmeln
4. **Protokollführer** - notiert jeden Schritt auf einem Arbeitsblatt

### **Materialien pro Gruppe:**
- 8-10 Gefäße (Becher), durchnummeriert
- ~150 Murmeln
- Programm-Karte (Befehlsliste)
- Schritt-für-Schritt Arbeitsblatt
- "Flagge" (Papierstreifen mit "TRUE/FALSE") für den Rechenkünstler

### **Wichtige Spielregeln:**
- Der Steuermann darf NICHT direkt Murmeln anfassen
- Der Laufbursche darf Murmeln nur transportieren, nicht "erfinden"
- Der Rechenkünstler darf nicht selbst zum Speicher gehen
- Jeder Schritt wird laut angesagt ("Befehl 0: tst 1 - TEST! Gefäß 1 ist nicht leer - FLAG=TRUE")


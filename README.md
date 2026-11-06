# OpenLearnBrain: LearnBrain

Ein lokaler, privater **Learning Agent Harness** (nutzbar mit Antigravity, Claude Code oder Codex), der als geführter KI-Tutor agiert. Er hilft beim Verstehen und Lernen von Uni-Inhalten, ohne Tokens zu verschwenden oder Lösungen einfach nur vorzukauen.

---

### 1. Das Ziel
Ein **lokaler, privater „Learning Agent Harness“** (nutzbar mit Antigravity, Claude Code oder Codex), der als geführter KI-Tutor agiert. Er hilft beim Verstehen und Lernen von Uni-Inhalten, ohne Tokens zu verschwenden oder Lösungen einfach nur vorzukauen.

---

### 2. Das 3-Stufen-Gedächtnis (L1 bis L3)

| Stufe | Ebene | Inhalt | Abruf & Token-Budget |
| :--- | :--- | :--- | :--- |
| **L3** | **Uni / Meta** | Studiengang, aktuelles Semester, Prüfungsordnung, Lernfortschritt, Stärken/Schwächen. | **Always-in-Context** (`AGENTS.md` / `state/`), ca. **100–150 Tokens**. |
| **L2** | **Semester** | Modulübersichten, Querverbindungen zwischen Fächern, Klausurphasen, Modul-Zusammenfassungen. | **On-Demand** bei Semesterthemen (`README.md`), ca. **200 Tokens**. |
| **L1** | **Atomar / Fach** | Konkrete Definitionen, Theoreme, Formeln, Code-Snippets, Übungsaufgaben aus Skripten. | **Selektive Vektorsuche** via Tool, ca. **50–150 Tokens** pro Treffer. |

---

### 3. Tool-Entscheidung: **Qdrant (lokal)**

* **Warum Qdrant?** 
  * Besitzt das beste **Payload-Filtering**: Der Agent kann gezielt filtern (z. B. `level == L1` UND `fach == Mathe_1`), wodurch null Token-Rauschen aus anderen Fächern entsteht.
  * Extrem schnell (< 5 ms), ressourcenschonend (in Rust geschrieben), läuft wahlweise als kleiner Docker-Container oder direkt dateibasiert in Python.
* **Warum keine Alternativen?**
  * *Letta:* Verursacht ein „Harness-in-a-Harness“-Problem (Antigravity/Claude Code *ist* bereits der Harness).
  * *LightRAG:* Zu rechenintensiv beim lokalen Indizieren hunderter Skript-Seiten via Ollama.

---

### 4. Der Aufbau des Harness-Ordners

```text
uni-learning-harness/
├── AGENTS.md                   # [L3] Globale Instruktionen, Profil, Sokratische Lehrmethode
├── state/                      # [L3 & L2] Dynamischer Status (Lernfortschritt, Noten, Deadlines)
│   ├── student_profile.md
│   └── current_semester.json
├── skills/                     # Didaktische Workflows für die KI
│   ├── socratic-tutor/         # Lehrmethode (Fragen stellen statt vorsagen)
│   └── exam-simulation/       # Generierung von Probeprüfungen
├── tools/                      # Das Gedächtnis-Interface
│   ├── indexer.py              # Skript zum Einlesen von PDFs/Notizen in Qdrant (mit L1-L3 Tags)
│   └── memory_tool.py          # CLI-Tool für die KI zum gezielten Suchen
└── library/                    # Deine originalen Unterlagen
    └── Semester_X/
        ├── README.md           # [L2] Modul-Zusammenfassungen
        └── Fach_Y/             # [L1] Skripte, Notizen, Übungen
```

---

### 5. Die Kernvorteile auf einen Blick

1. **Tokensparend:** Das System lädt niemals ganze Skripte, sondern nur punktuell 2–3 Absätze (oft unter 200 Tokens Gesamtoverhead).
2. **Didaktisch geführt:** Der Harness zwingt die KI über Skills dazu, dich mit gezielten Fragen zur Lösung zu führen, statt Hausaufgaben für dich zu erledigen.
3. **100 % lokal & privat:** Alle Skripte, Notizen und Embeddings laufen offline (z. B. über Ollama + Qdrant) auf deinem Rechner.
4. **Harness-Agnostisch:** Funktioniert nahtlos mit Antigravity, Claude Code, Cursor oder Codex.

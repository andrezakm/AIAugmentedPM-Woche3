---
disable-model-invocation: true
---

# Kurs: Woche 3 — Skills & Agents

Willkommen in Woche 3. Du lernst heute die drei Schichten eines Claude-Code-Systems kennen und baust einen eigenen Analyse-Agenten. Der Kurs hat 7 Schritte. Du navigierst mit "weiter" (nächster Schritt) oder "Schritt X" (direkt springen).

---

## Schritt 1: Architektur verstehen

Claude Code kennt drei Ebenen:

| Ebene | Datei | Zweck |
|-------|-------|-------|
| **Prinzipien** | `CLAUDE.md` | Was gilt immer — Kontext, Qualitätsstandards, Pfade |
| **Skills** | `.claude/skills/*/SKILL.md` | Was Claude kann — aufrufbar mit `/skill-name` |
| **Agents** | `.claude/agents/*.md` | Wie Claude handelt — eigenständige Prozesse mit Tools |

Der Unterschied: Eine plain `.md`-Datei wird passiv gelesen. Ein Skill wird aktiv aufgerufen. Ein Agent läuft autonom und orchestriert andere Skills.

**Frage zum Nachdenken:** Welche Ebene brauchst du wenn du willst, dass Claude immer in einem bestimmten Ton antwortet? Und welche wenn du einen mehrstufigen Prozess automatisieren willst?

Sag "weiter" für Schritt 2.

---

## Schritt 2: Dateien erkunden

Schau dir jetzt die Struktur an:

1. Lies `CLAUDE.md` — was definiert NeoEmployee als Qualitätsstandard?
2. Öffne `.claude/skills/feedback-synthesizer/SKILL.md` — wie ist ein Skill aufgebaut?
3. Öffne `.claude/agents/pattern-agent.md` — wie orchestriert ein Agent mehrere Skills?

Achte auf die Frontmatter-Felder (`allowed-tools`, `disable-model-invocation`, `name`, `description`). Diese kontrollieren was Claude darf und wie der Skill/Agent gefunden wird.

**Frage:** Was passiert wenn ein Skill `allowed-tools: Read` hat aber du ihm sagst er soll eine Datei löschen?

Sag "weiter" für Schritt 3.

---

## Schritt 3: Pattern Agent starten

Starte jetzt den Agenten:

> "Starte den pattern-agent"

Beobachte was passiert:
- Welche Skills werden nacheinander aufgerufen?
- In welcher Reihenfolge entstehen die Output-Dateien?
- Was gibt der Agent am Ende als Zusammenfassung aus?

Schau in `output/` während der Agent läuft. Die Dateien entstehen sequenziell: erst `01_clusters_YYYYMMDD_HHMM.md`, dann `02_scored_YYYYMMDD_HHMM.md`, dann `03_decision_brief_YYYYMMDD_HHMM.md`.

Sag "weiter" sobald alle drei Dateien in `output/` liegen.

---

## Schritt 4: Output beurteilen

Öffne die drei Output-Dateien und beantworte für dich:

**`output/01_clusters.md`**
- Sind die Cluster trennscharf? Oder überlappen sie sich?
- Sind die Belege echte Direktzitate oder Paraphrasen?
- Gibt es Widersprüche im Feedback die der Synthesizer dokumentiert hat?

**`output/02_scored.md`**
- Sind die Scores differenziert (nicht alle gleich)?
- Sind die Begründungen rückverfolgbar auf `context/company.md` oder `context/strategy.md`?
- Welcher Cluster hat den höchsten Gesamt-Score?

**`output/03_decision_brief.md`**
- Ist die Empfehlung eindeutig (BUILD / SKIP / MEHR DATEN)?
- Gibt es konkrete Zahlen aus dem Feedback-Material?
- Fehlt dir eine Sektion die du gerne hättest?

Die letzte Frage führt direkt zu Aufgabe (a).

Sag "weiter" für Schritt 5.

---

## Schritt 5: Decision Brief anpassen (Aufgabe a)

Öffne `.claude/skills/decision-brief/SKILL.md`.

**Deine Aufgabe:** Füge mindestens eine neue Sektion zum Output-Format hinzu. Ideen:
- **Risiken** — Was könnte schiefgehen? Was haben wir nicht geprüft?
- **Alternativen** — Was wäre die zweitbeste Option?
- **Nächste Schritte** — Was müsste in den nächsten 2 Wochen passieren?
- **Stakeholder-Auswirkungen** — Wen betrifft diese Entscheidung wie?

Editiere die Datei direkt. Dann starte den pattern-agent neu:

> "Starte den pattern-agent"

Vergleiche den neuen `output/03_decision_brief.md` mit dem alten. Was hat sich verändert?

Sag "weiter" für Schritt 6.

---

## Schritt 6: Neuen Skill bauen (Aufgabe b)

### Was ist ein Skill — nochmal kurz

Ein Skill ist eine Markdown-Datei die Claude Code sagt: *"Wenn du diesen Skill ausführst, verhalte dich genau so."* Er besteht aus:

- **Frontmatter** (`---` Block oben): technische Metadaten — welche Tools der Skill nutzen darf (`allowed-tools`), ob Claude ihn direkt ausführen darf oder nur als Vorlage liest (`disable-model-invocation`)
- **Prompt-Inhalt**: Anweisungen, Vorgehen, Output-Format — alles was Claude beim Ausführen des Skills lesen soll

**Warum liegt er unter `.claude/skills/name/SKILL.md`?**
Claude Code lädt automatisch alle Dateien in `.claude/skills/` und macht sie als `/name` aufrufbar. Der Ordnername wird zum Befehl. `slack-importer/SKILL.md` → `/slack-importer`. Das Verzeichnis ist Teil des Projekts — wer das Projekt klont, bekommt die Skills mit. Skills sind geteilt, versioniert, reproduzierbar.

**Ohne diese Struktur:** Claude weiß nicht dass der Skill existiert. Er wird nicht aufrufbar.

---

### Skill bauen mit Claude Code

Du schreibst den Skill nicht selbst — du beschreibst Claude Code was du willst, und Claude Code erstellt die Datei. Das ist die eigentliche Übung: Claude Code als Werkzeug nutzen um Claude Code zu konfigurieren.

**So gehst du vor:**

1. Wähle eine der beiden Varianten unten
2. Kopiere den Prompt-Vorschlag in den Chat
3. Claude Code erstellt die Datei unter dem richtigen Pfad
4. Teste den Skill sofort

---

### Variante A: `slack-importer`

Dieser Skill liest `input/raw_feedback/slack.md` und extrahiert nur Nachrichten mit echtem Signal (Schmerz, Wunsch, konkretes Feedback) — und filtert Rauschen raus (Terminankündigungen, Smalltalk, interne Logistik).

**Prompt für Claude Code:**

> Erstelle einen neuen Skill unter `.claude/skills/slack-importer/SKILL.md`. Der Skill soll `input/raw_feedback/slack.md` lesen, jede Nachricht bewerten ob sie ein echtes Kundensignal enthält (Schmerz, Wunsch, Feedback) oder Rauschen ist (Terminankündigungen, Koordination, Smalltalk), und nur die Signal-Nachrichten mit Quelle und Kategorie nach `output/00_slack_filtered.md` schreiben. Frontmatter: `allowed-tools: Read, Write`. Füge klare Filterkriterien ein.

Warum diese Variante? Sie zeigt wie `allowed-tools` den Skill auf konkrete Tools einschränkt — und wie ein Skill als Vorfilter für andere Skills funktioniert.

---

### Variante B: `devils-advocate`

Dieser Skill nimmt eine Produktempfehlung entgegen und argumentiert dagegen: Welche Annahmen könnten falsch sein? Was wurde übersehen?

**Prompt für Claude Code:**

> Erstelle einen neuen Skill unter `.claude/skills/devils-advocate/SKILL.md`. Der Skill bekommt eine Produktempfehlung als Text übergeben (kein Datei-Zugriff nötig) und gibt genau 3 Gegenargumente zurück. Jedes Gegenargument hat einen Titel, eine Begründung und erklärt warum das für die Entscheidung relevant ist. Kein `allowed-tools` im Frontmatter. Format: nummerierte Liste.

Warum diese Variante? Sie zeigt dass Skills auch ohne Tools sinnvoll sind — als strukturierte Denk-Prompts die Claude in eine bestimmte Rolle zwingen.

---

**Teste deinen Skill nach dem Anlegen:**
- Variante A: Sag `Führe /slack-importer aus`
- Variante B: Sag `/devils-advocate — hier ist die Empfehlung: [füge den Inhalt aus deinem decision_brief ein]`

Sag "weiter" für Schritt 7.

---

## Schritt 7: Reflexion

Du hast in dieser Stunde:
- Die drei Schichten von Claude Code verstanden (CLAUDE.md, Skills, Agents)
- Einen Agenten beobachtet der drei Skills orchestriert
- Einen bestehenden Skill erweitert
- Einen neuen Skill gebaut

**Drei Fragen für deinen Kontext:**

1. **Welchen Prozess in deiner Arbeit** würdest du als Agent bauen — also mehrere Schritte, die immer in derselben Reihenfolge passieren?

2. **Welche Qualitätsprinzipien** gehören in dein `CLAUDE.md` — was soll Claude in deinem Kontext immer wissen?

3. **Welchen Skill** würdest du als erstes bauen — für eine Aufgabe die du regelmäßig machst und bei der du eine konsistente Struktur willst?

Schreib deine Antworten auf — oder diskutiere sie direkt hier im Chat.

---

**Woche 3 abgeschlossen.**

---

## Zusatzmaterial

- `SkillsAgentsDoc.md` — Taxonomie, Entstehungsweg und Entscheidungsbaum: wann wird ein Experiment zum Skill, wann zum Agent?
- `MCPKonnektorenDoc.md` — MCP von einfach zu komplex: Konnektoren, `/mcp add`, Custom Server, Custom Skript
- `ClaudeRemoteDoc.md` — Remote Interfaces: Channels (Telegram/Discord/iMessage), Remote Control, Computer Use

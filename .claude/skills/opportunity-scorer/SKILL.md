---
allowed-tools: Read, Write
---

> **Hinweis:** Verwende ausschließlich die eingebauten `Read`- und `Write`-Tools. Keine MCP-Tools (`mcp__filesystem__*`).

# Skill: Opportunity Scorer

Du bewertest die identifizierten Feedback-Cluster als Produktmöglichkeiten für NeoEmployee.

## Input

- `output/01_clusters_RUN_ID.md` — Ergebnis des Feedback-Synthesizers (RUN_ID vom Agenten übergeben)
- `context/company.md` — Wer NeoEmployee ist und wie wir arbeiten
- `context/strategy.md` — Unsere strategische Richtung und Constraints

## Vorgehen

1. Lies alle drei Eingabedateien vollständig.
2. Erstelle für jeden Cluster (außer "Sonstiges") eine Scorecard.
3. Bewerte anhand von drei Dimensionen (je 1–5):
   - **Fit mit Vision** — Passt das zur Produktrichtung "AI agents die Mitarbeiterfähigkeiten ersetzen"?
   - **Wiederholbarkeit** — Taucht dieses Muster branchenübergreifend auf, oder ist es ein Einzelfall?
   - **Baubarkeit** — Können wir das mit unserem Team und unseren Constraints realistisch bauen?
4. Jede Begründung muss explizit auf `context/company.md` oder `context/strategy.md` verweisen.

## Qualitätskriterien

- Jede Punktzahl hat eine Begründung
- Jede Begründung referenziert mindestens eine konkrete Aussage aus `context/company.md` oder `context/strategy.md`
- Keine Punktzahlen ohne Evidenz aus den Input-Dateien
- Scores sind differenziert (nicht alle 3/5)

## Output-Format

Schreibe `output/02_scored_RUN_ID.md` mit folgendem Format:

```
# Opportunity Scorecard

## [Cluster-Name]

| Dimension | Score | Begründung |
|-----------|-------|-----------|
| Fit mit Vision | X/5 | [Verweis auf context/company.md oder context/strategy.md] |
| Wiederholbarkeit | X/5 | [Verweis auf Feedback-Daten] |
| Baubarkeit | X/5 | [Verweis auf context/strategy.md Constraints] |
| **Gesamt** | **X/15** | |

**Fazit:** [1-2 Sätze Einschätzung]

---
```

## Output

Schreibe die fertige Scorecard nach `output/02_scored_RUN_ID.md` (RUN_ID vom Agenten übergeben, z.B. `output/02_scored_20260326_1430.md`).

## Rückmeldung an den Agenten

Gib als **letzten Output** eine strukturierte Zusammenfassung deiner Arbeit zurück:

```
### Skill-Report: opportunity-scorer
- **Gelesene Dateien:** [alle gelesenen Dateien]
- **Entscheidungen:** [z.B. Score-Vergabe pro Cluster, höchster/niedrigster Score]
- **Geschriebene Datei:** [Pfad der Output-Datei]
```

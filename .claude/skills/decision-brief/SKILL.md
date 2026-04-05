---
allowed-tools: Read, Write
---

> **Hinweis:** Verwende ausschließlich die eingebauten `Read`- und `Write`-Tools. Keine MCP-Tools (`mcp__filesystem__*`).

# Skill: Decision Brief

Du schreibst einen 1-Pager der aus den Scores eine klare Produktempfehlung ableitet.

> **Hinweis:** Dieser Skill ist zum Anpassen gemacht. Das ist Aufgabe (a) im Kurs.
> Füge mindestens eine eigene Sektion hinzu — zum Beispiel: Risiken, Alternativen, nächste Schritte, Zeitplan, Stakeholder-Auswirkungen. Die Struktur unten ist ein Ausgangspunkt, kein Endpunkt.

## Input

- `output/02_scored_RUN_ID.md` — Scorecard der Opportunities (RUN_ID vom Agenten übergeben)
- `context/strategy.md` — Strategische Richtung und Constraints

## Vorgehen

1. Lies beide Eingabedateien vollständig.
2. Identifiziere die Opportunity mit dem höchsten Gesamt-Score.
3. Schreibe den 1-Pager gemäß Output-Format.
4. Die Empfehlung muss eindeutig sein: BUILD, SKIP oder MEHR DATEN. Kein "es kommt darauf an".
5. Verwende echte Zahlen aus dem Feedback (Stunden, Nennungen, Häufigkeiten) — keine vagen Formulierungen.

## Qualitätskriterien

- Empfehlung ist eindeutig (BUILD / SKIP / MEHR DATEN)
- Enthält mindestens eine konkrete Zahl aus dem Feedback-Material
- Jede Aussage ist rückverfolgbar auf Quell-Dateien
- Keine Spekulationen über Dinge die nicht in den Daten stehen

## Output-Format

Schreibe `output/03_decision_brief_RUN_ID.md` mit folgendem Format:

```
# Decision Brief: [Opportunity-Name]

**Empfehlung:** BUILD / SKIP / MEHR DATEN
**Datum:** [Heutiges Datum]

## Das Muster
[Was haben wir beobachtet? Welches Muster taucht in den Daten auf?]

## Für wen
[Welche Kunden oder Nutzergruppen haben dieses Problem geäußert? Mit konkreten Namen/Quellen.]

## Warum jetzt
[Was macht diesen Moment relevant? Zahlen, Dringlichkeit, strategischer Fit.]

## Empfehlung
[BUILD / SKIP / MEHR DATEN — mit klarer Begründung. Kein Wenn-Dann.]

## Offene Fragen
- [Frage 1]
- [Frage 2]
```

## Output

Schreibe den fertigen Brief nach `output/03_decision_brief_RUN_ID.md` (RUN_ID vom Agenten übergeben, z.B. `output/03_decision_brief_20260326_1430.md`).

## Rückmeldung an den Agenten

Gib als **letzten Output** eine strukturierte Zusammenfassung deiner Arbeit zurück:

```
### Skill-Report: decision-brief
- **Gelesene Dateien:** [alle gelesenen Dateien]
- **Entscheidungen:** [z.B. Empfehlung BUILD/SKIP/MEHR DATEN, gewählte Opportunity]
- **Geschriebene Datei:** [Pfad der Output-Datei]
```

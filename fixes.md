# Fixes & Learnings

## Ausgangslage

Zwei Agenten — `pattern-agent.md` (defekt) und `pattern-decision.md` (gefixt) — lösen dieselbe Aufgabe.
Der Vergleich zeigt, was schiefging und warum die Fixes funktionieren.

---

## Fix 1: Skills können in Subagenten nicht per Slash-Command aufgerufen werden

**Problem:** `pattern-agent` versuchte `/feedback-synthesizer` etc. aufzurufen.
Subagenten haben keinen Zugriff auf das Skill-Tool — Slash-Commands funktionieren nur im Hauptkontext.

**Fix:** Skills im Frontmatter unter `skills:` listen. Der Skill-Inhalt wird beim Start injiziert.
Der Agent hat die Instruktionen dann als Kontext und führt sie selbst aus.

**Vorher (`pattern-agent`):**
```yaml
tools: [Read, Write]
```
```
Rufe den Skill `/feedback-synthesizer` auf.
```

**Nachher (`pattern-decision`):**
```yaml
skills:
  - feedback-synthesizer
  - opportunity-scorer
  - decision-brief
```
Kein Slash-Command im Prompt — der Agent orchestriert anhand der injizierten Skill-Inhalte.

---

## Fix 2: Eingeschränkte Tool-Liste als Wurzel mehrerer Folgefehler

**Problem:** `pattern-agent` hat explizit `tools: [Read, Write]` gesetzt. Das blockierte alle anderen Tools — insbesondere `Bash` und `Write` mit Verzeichniserstellung. Daraus entstanden zwei Folgefehler:

**Folgefehler a — Timestamp-Generierung:** Ohne `Bash` kann `date +%Y%m%d_%H%M` nicht ausgeführt werden. Das Modell musste raten → inkonsistente Dateinamen → fehlende Output-Dateien.

**Folgefehler b — Output-Verzeichnis nicht erstellbar:** Ohne `Bash` (kein `mkdir`) schlugen alle Schreibversuche nach `output/` fehl, wenn das Verzeichnis nicht existierte. Die Fehlermeldungen führten zu weiteren Fix-Versuchen, die neue Fehler produzierten.

**Fix:** Keine `tools:`-Einschränkung im Frontmatter → Agent bekommt Standardtools inklusive `Bash`.
Alternativ hätte auch ein einfaches "Claude, schreibe die Dateien" im Chat gereicht — das wäre aber weniger robust.

**Einfachste Lösung:** `tools:`-Zeile weglassen. `pattern-decision` macht genau das.

---

## Fix 3: Zielbasierte vs. prozedurale Agent-Prompts

**Learning:** Statt "Rufe Skill A auf, dann Skill B, dann Skill C" reicht eine Zielkette:
"Synthetisiere → Score → Empfehlung". Die Skills liefern Formate und Qualitätsstandards,
der Agent orchestriert selbst. Getestet mit `pattern-decision` Agent.

**Vorher (`pattern-agent`):** Jeder Schritt beschreibt explizit den Skill-Aufruf per Command.

**Nachher (`pattern-decision`):** Drei nummerierte Ziele mit Input/Output-Pfaden, kein Befehlssatz.

---

## Fix 4: Umlaut-Problem trotz deutscher Sprachanweisung

**Problem:** `pattern-decision` schrieb in seinen Outputs "ue", "oe", "ae" statt ä, ö, ü — obwohl die Sprachanweisung im Agent-Prompt korrekte Umlaute forderte. Ein lokaler Fix im Agenten allein reichte nicht zuverlässig.

**Fix:** Globale Umlaut-Anweisung in `CLAUDE.md` ergänzt:

```
## Sprache
Alle Outputs immer mit korrekten deutschen Umlauten: ä, ö, ü, Ä, Ö, Ü, ß. Keine ASCII-Ersetzungen (ae, oe, ue).
```

**Warum das funktioniert:** CLAUDE.md-Instruktionen werden für alle Kontexte (Haupt-Agent, Subagenten) geladen und haben höhere Priorität als lokale Prompt-Anweisungen.

---

## Fix 5: Skill-Reports für Sichtbarkeit der Skill-Ausführung

**Problem:** Es war nicht erkennbar, ob und wann Skills vom Agenten aufgerufen wurden — Skills wurden als Teil des injizierten Kontexts ausgeführt, ohne sichtbares Signal.

**Fix:** Jeder Skill gibt am Ende einen `### Skill-Report` zurück (strukturierte Zusammenfassung: gelesene Dateien, Entscheidungen, geschriebene Datei). `pattern-decision` ist angewiesen, diesen Report nach jedem Skill in ein Debug-Log zu schreiben.

**Ergebnis:** Drei nachvollziehbare Spuren in `output/debug_RUN_ID.md` — eine pro Skill — mit Zeitstempel. Man sieht explizit, dass die Skills ausgeführt wurden, obwohl sie nicht per Slash-Command aufgerufen wurden.

---

## Fix 6: Debug-Log für Run-Nachvollziehbarkeit

**Neu in `pattern-decision`:** Der Agent schreibt zu Beginn `output/debug_RUN_ID.md` und ergänzt es nach jedem Skill-Lauf mit dem jeweiligen Skill-Report + Zeitstempel. Abschluss mit einer Gesamtzusammenfassung.

**Zweck:** Reproduzierbarkeit über mehrere Runs, Fehlerdiagnose ohne Output-Dateien öffnen zu müssen.

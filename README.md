# Woche 3: Skills & Agents

In dieser Woche lernst du wie Claude Code aus einfachen Markdown-Dateien ein strukturiertes System wird — mit Prinzipien, wiederverwendbaren Skills und autonomen Agenten.

## Voraussetzungen

- Claude Code installiert (`npm install -g @anthropic-ai/claude-code`)
- Woche 1 und Woche 2 abgeschlossen (hilfreich, aber nicht zwingend)
- Claude über Anthropic login authentifiziert

## Installation

1. Kopiere wieder dieses Reopitory (z. Bsp. zip herunterladen)
2. Starte Claude Code in dem entstehenden Verzichnis: `claude`

## Kurse starten

**Kurs 1: Skills & Agents**
```
/kurs
```
Oder einfach sagen: "starte den Kurs"

**Kurs 2: MCP & externe Dienste**
```
/kurs-mcp
```
Oder sagen: "starte den MCP-Kurs"

## Dateistruktur

| Pfad | Rolle |
|------|-------|
| `CLAUDE.md` | Projekt-Prinzipien, immer aktiv |
| `.claude/skills/kurs/` | Interaktiver Kurspfad (`/kurs`) |
| `.claude/skills/feedback-synthesizer/` | Analysiert Roh-Feedback, clustert Muster |
| `.claude/skills/opportunity-scorer/` | Bewertet Cluster als Produktchancen |
| `.claude/skills/decision-brief/` | Schreibt eine klare Produktempfehlung |
| `.claude/agents/pattern-agent.md` | Orchestriert alle drei Skills in Sequenz |
| `.claude/skills/kurs-mcp/` | Interaktiver Kurspfad MCP (`/kurs-mcp`) |
| `context/` | NeoEmployee-Kontext (company.md, strategy.md) |
| `input/` | Rohes Kundenfeedback |
| `output/` | Entsteht beim Ausführen des Agenten |
| `SkillsAgentsDoc.md` | Zusatzmaterial: Skills vs. Agents Taxonomie |
| `MCPKonnektorenDoc.md` | Zusatzmaterial: MCP von einfach zu komplex |
| `ClaudeRemoteDoc.md` | Zusatzmaterial: Remote Interfaces & Computer Use |

## Modell-Empfehlung

Verwende **Claude Sonnet** oder **Claude Opus** für beste Ergebnisse. Der pattern-agent führt drei Skills sequenziell aus — das kostet Token. Mit einem schwächeren Modell können Cluster unscharf oder Scores undifferenziert werden.

## Token-Hinweis

Der pattern-agent liest alle Feedback-Dateien + Kontext-Dateien in einem Durchlauf. Bei sehr großen Feedback-Mengen (>50 Einträge) kann das Kontextlimit eng werden. Für diesen Kurs ist die Datenmenge bewusst begrenzt gehalten.

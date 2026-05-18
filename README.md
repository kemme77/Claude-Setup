# Claude-Setup

Persönliches Template-Repo für [Claude Code](https://docs.claude.com/en/docs/claude-code) Setup. Dient als Startpunkt für neue Projekte und als wachsende Sammlung nützlicher Skills, Subagents, Hooks und MCP-Konfigurationen.

## Was drinsteckt

| Pfad | Zweck |
|------|-------|
| `CLAUDE.md` | Projekt-Kontext + Tech-Stack-Auto-Detect-Instruktion für Claude |
| `CLAUDE.local.md.example` | Vorlage für persönliche, gitignorete Prefs |
| `.mcp.json` | MCP-Server-Konfiguration (Beispiele auskommentiert) |
| `.claude/settings.json` | Permissions + Hooks |
| `.claude/skills/` | Custom Skills (siehe `example-skill/`) |
| `.claude/agents/` | Subagent-Definitionen (`code-reviewer`, `test-writer`) |
| `.claude/commands/` | Custom Slash Commands |

## Quick Start

```bash
# 1. Repo klonen oder als Template verwenden
git clone <repo-url> mein-neues-projekt
cd mein-neues-projekt

# 2. Persönliche Prefs anlegen (wird via .gitignore ignoriert)
cp CLAUDE.local.md.example CLAUDE.local.md

# 3. Claude Code starten
claude
```

Beim ersten Start scannt Claude das Repo nach Tech-Stack-Indikatoren (`package.json`, `pyproject.toml`, `go.mod`, etc.), fragt zur Verifikation und trägt den bestätigten Stack samt offiziellen Dokumentations-URLs in `CLAUDE.md` ein. Danach sind diese URLs die kanonische Wissensquelle für Library-Verhalten.

## Anpassen

- **Tech Stack:** wird beim ersten Start interaktiv ausgefüllt
- **Konventionen / Testing / Git Workflow:** Platzhalter in `CLAUDE.md` ersetzen
- **MCP-Server:** Beispiele in `.mcp.json` einkommentieren oder eigene ergänzen
- **Hooks/Permissions:** in `.claude/settings.json` anpassen
- **Neue Skills/Agents:** Verzeichnisse `.claude/skills/` und `.claude/agents/` erweitern

## Referenzen

- [Claude Code Dokumentation](https://docs.claude.com/en/docs/claude-code)
- [Skills](https://docs.claude.com/en/docs/claude-code/skills)
- [Subagents](https://docs.claude.com/en/docs/claude-code/sub-agents)
- [Hooks](https://docs.claude.com/en/docs/claude-code/hooks)
- [MCP](https://docs.claude.com/en/docs/claude-code/mcp)
- [Slash Commands](https://docs.claude.com/en/docs/claude-code/slash-commands)

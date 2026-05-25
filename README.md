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
| `.claude/agents/` | Subagent-Definitionen (siehe [Subagents](#subagents)) |
| `.claude/commands/` | Custom Slash Commands |

## Quick Start

Empfohlener Weg: **`/init-from-template`** Slash-Command (siehe unten). In einem leeren Projektverzeichnis Claude Code starten, `/init-from-template` aufrufen — Template wird kopiert, Stack interaktiv abgefragt, `CLAUDE.md` gefüllt.

Manueller Weg:

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

## Slash-Commands für Automatisierung

Vier Slash-Commands automatisieren den Template-Lifecycle. Im Template unter `.claude/commands/` als Doku, **funktional** sind sie als Mirror in `~/.claude/commands/` (user-global) installiert — damit überall verfügbar, auch in leeren Verzeichnissen.

| Command | Zweck |
|---------|-------|
| `/init-from-template` | Init neues Projekt — kopiert Template, fragt Stack ab, füllt `CLAUDE.md`, ersetzt `{{PROJECT_NAME}}`/`{{PROJECT_PURPOSE}}` |
| `/sync-to-template` | Backport — kopiert neue/geänderte Agents/Skills/Commands zurück ins Template |
| `/audit-claude-setup` | Checkliste — prüft aktuelles Projekt gegen Template-Konventionen |
| `/upgrade-template` | Drift-Check — übernimmt Template-Verbesserungen selektiv ins aktuelle Projekt |

### Installation auf einer neuen Maschine

Template-Pfad ist in den Command-Bodies **hardcoded** als `/home/oogway/GitHub/Claude-Setup`. Auf anderer Maschine:

```bash
# Template lokal klonen
git clone <repo-url> ~/GitHub/Claude-Setup

# User-globale Commands installieren
mkdir -p ~/.claude/commands
cp ~/GitHub/Claude-Setup/.claude/commands/{init-from-template,sync-to-template,audit-claude-setup,upgrade-template}.md ~/.claude/commands/

# Wenn Template-Pfad woanders liegt: ersetzen
sed -i 's|/home/oogway/GitHub/Claude-Setup|/dein/pfad|g' ~/.claude/commands/{init-from-template,sync-to-template,audit-claude-setup,upgrade-template}.md
```

### Sicherheits-Garantien

- **Kein Auto-Commit** — alle Commands enden mit `git status`, du commitest manuell.
- **Kein Blind-Overwrite** — bei Konflikten (Datei existiert mit anderem Inhalt) wird Diff gezeigt und nachgefragt.
- **`CLAUDE.md` beim Upgrade** wird nie überschrieben — nur Patch-Vorschlag.

## Subagents

Acht universelle Subagents unter `.claude/agents/`. Werden bei `/init-from-template` ins Zielprojekt kopiert **und** sind zusätzlich user-global in `~/.claude/agents/` gespiegelt — also auch in Projekten ohne Template-Setup verfügbar.

Aufruf: `Use the <name> subagent to ...` oder `@agent-<name>` im Prompt. Trigger-Phrasen in den `description:`-Feldern sorgen für automatische Delegation in passenden Situationen.

| Agent | Typ | Tools | Modell | Zweck |
|-------|-----|-------|--------|-------|
| `code-reviewer` | Review (read-only) | Read, Grep, Glob, Bash | inherit | Bugs, Security, Lesbarkeit, CLAUDE.md-Konventionen — pre-commit/PR |
| `architecture-reviewer` | Review (read-only) | Read, Grep, Glob, Bash | opus | Modulgrenzen, Kopplung, Layering, Skalierungsannahmen |
| `ui-ux-reviewer` | Review (read-only) | Read, Grep, Glob, Bash | sonnet | a11y (WCAG), Responsive, Semantic HTML, Konsistenz |
| `security-auditor` | Review (read-only) | Read, Grep, Glob, Bash | opus | OWASP-Top-10-Fokus: Injection, Auth, Crypto, Secrets, SSRF, IDOR |
| `performance-analyzer` | Review (read-only) | Read, Grep, Glob, Bash | sonnet | N+1, algorithmische Komplexität, Re-Renders, Bundle-Bloat |
| `test-writer` | Action | Read, Edit, Write, Grep, Glob, Bash | sonnet | Tests für Edge Cases — Framework-Detect aus Stack |
| `debugger` | Action | Read, Edit, Bash, Grep, Glob | sonnet | Root-Cause-Analyse: reproduce → isolate → minimal fix → verify |
| `docs-writer` | Action | Read, Edit, Write, Grep, Glob, Bash | sonnet | README, API-Docs, WHY-Kommentare aus Code |

### Installation des User-Global Mirrors

Wie bei den Slash-Commands: auf neuer Maschine nach Template-Clone einmalig spiegeln.

```bash
mkdir -p ~/.claude/agents
cp ~/GitHub/Claude-Setup/.claude/agents/*.md ~/.claude/agents/
```

Template bleibt Source of Truth — Änderungen dort, dann via `/sync-to-template` oder manuelles `cp` propagieren.

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

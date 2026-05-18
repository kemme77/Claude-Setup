---
description: Prüft das Claude-Code-Setup im aktuellen Projekt gegen die Template-Konventionen aus Claude-Setup und gibt einen strukturierten Checklisten-Report aus. Read-only.
---

# /audit-claude-setup

Read-only Audit. Keine Änderungen am Projekt. Ergebnis: was passt, was fehlt, was sollte verbessert werden.

## Checks

### Basis-Dateien

| Check | Wie prüfen |
|-------|-----------|
| `CLAUDE.md` vorhanden | `Read` |
| `CLAUDE.md` keine offenen `{{...}}` Platzhalter | `Grep '{{[A-Z_]+}}' CLAUDE.md` |
| `CLAUDE.md` keine offenen `<PLATZHALTER` Marker | `Grep '<PLATZHALTER' CLAUDE.md` |
| Tech-Stack-Sektion gefüllt | Inhalt nicht "_Noch nicht verifiziert_" |
| `.mcp.json` in Root | `Read` |
| `CLAUDE.local.md.example` vorhanden | `Read` |
| `.gitignore` enthält `CLAUDE.local.md` | `Grep 'CLAUDE.local.md' .gitignore` |
| `.claude/settings.json` vorhanden | `Read` |

### Inhalts-Bestand

| Check | Wie prüfen |
|-------|-----------|
| Mindestens 1 Skill | `Glob .claude/skills/*/SKILL.md` |
| Mindestens 1 Subagent | `Glob .claude/agents/*.md` |
| Mindestens 1 Custom Command | `Glob .claude/commands/*.md` |

### Hygiene

| Check | Wie prüfen |
|-------|-----------|
| `CLAUDE.local.md` nicht im Index | `git ls-files | grep -x CLAUDE.local.md` → leer |
| Keine `.env*` im Index | `git ls-files | grep -E '^\.env'` → leer |
| `node_modules/` nicht im Index (falls JS/TS) | `git ls-files | grep -E '^node_modules/'` → leer |

## Output-Format

```
## Claude Setup Audit — <projektname>

### OK
- CLAUDE.md vorhanden + Stack verifiziert
- .mcp.json + settings.json present
- 2 Skills, 3 Agents, 1 Command

### Fehlt / unvollständig
- .gitignore ohne CLAUDE.local.md (Vorschlag: Zeile ergänzen)
- Tech-Stack-Sektion enthält noch "_Noch nicht verifiziert_"

### Hygiene-Warnung
- node_modules/foo im Index (Vorschlag: `git rm --cached -r node_modules/`)

### Vorschlag
1. .gitignore patchen
2. /init-from-template erneut für Stack-Verifikation
3. (optional) /upgrade-template für aktuelle Template-Improvements
```

## Regeln

- **Read-only** — niemals editieren oder committen.
- Wenn `pwd` == Template selbst: leichtere Variante (nicht alle Checks sinnvoll).
- Bei nicht-Git-Repos: Hygiene-Checks skippen, vermerken.
- Pro Check kurz, präzise. Keine Romane.

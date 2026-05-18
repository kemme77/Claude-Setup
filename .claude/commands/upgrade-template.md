---
description: Vergleicht das Claude-Setup im aktuellen Projekt mit der neuesten Version im Claude-Setup Template-Repo und übernimmt Verbesserungen selektiv ins Projekt. CLAUDE.md wird nie blind überschrieben — nur Patch-Vorschlag.
---

# /upgrade-template

Nimmt Drift zwischen lokalem Projekt und Template auf, schlägt selektive Updates vor.

**Source:** `/home/oogway/GitHub/Claude-Setup`
**Target:** aktuelles Projekt-Verzeichnis (`pwd`)

## Vorgehen

### 1. Sanity-Check

- Wenn `pwd` == Source: abbrechen.
- Wenn Source nicht existiert: Fehler + README-Hinweis.
- Wenn Target kein `.claude/` hat: Hinweis → `/init-from-template` wäre besser.

### 2. Files-of-interest sammeln

**Strukturell (sicher upgradebar):**

- `.gitignore`
- `.mcp.json`
- `.claude/settings.json`
- Alle `.claude/skills/`, `.claude/agents/`, `.claude/commands/` Files **die im Target fehlen**

**Riskant (User-Inhalt drin, nur Diff zeigen):**

- `CLAUDE.md` — nie überschreiben, nur Patch-Vorschlag
- `CLAUDE.local.md.example`
- `README.md` (falls User Template-README behalten hat)
- Files in `.claude/...` die im Target **modifiziert** wurden gegenüber Template

### 3. Pro Datei klassifizieren

| Zustand                        | Aktion                                     |
| ------------------------------ | ------------------------------------------ |
| im Template, nicht im Target   | **NEU** — sicher zu kopieren               |
| in beiden, identisch           | ignorieren                                 |
| in beiden, Target modifiziert  | **DRIFT** — Diff zeigen, User entscheidet  |
| nur im Target                  | ignorieren (gehört nicht zum Template)     |

### 4. Bericht + AskUserQuestion

Ausgabe:

```text
Neu im Template (sicher übernehmbar):
- .claude/commands/neuer-skill.md

Drift (Diff anschauen):
- .gitignore  (+1 Zeile im Template)
- CLAUDE.md   (+ Sektion "Sicherheit" im Template) — Patch-Vorschlag, kein Overwrite
```

`AskUserQuestion (multiSelect)`: Welche Files updaten?

### 5. Updates anwenden

- **NEU**: `cp` aus Template ins Target.
- **DRIFT bei strukturellen Files** (.gitignore, settings.json, etc.): `diff -u` zeigen, dann `AskUserQuestion`:
  - "Template-Version übernehmen (overwrite)"
  - "Manuell mergen (Diff anzeigen, du editierst)"
  - "Überspringen"
- **DRIFT bei `CLAUDE.md`**: **nie** überschreiben. Stattdessen:
  - Diff zeigen
  - Vorschlag: "Welche Sektionen aus Template übernehmen? Ich kann gezielt patchen."
  - Bei Zustimmung: `Edit` mit gezieltem Patch.

### 6. Abschluss

```bash
git status
```

Bericht:

- Übernommene Files
- Übersprungen
- Manuell zu mergende
- Hinweis: `/audit-claude-setup` für Endprüfung

## Regeln

- **Niemals committen.**
- **`CLAUDE.md` nie blind ersetzen** — User-Inhalt schützen.
- Bei Skill-Ordnern: alle Files im Ordner betrachten, nicht nur SKILL.md.
- Versions-Drift im Tech Stack (z.B. Doku-URL geändert) bewusst ignorieren — User entscheidet bei eigenem Stack.

---
description: Initialisiert .claude/ Setup im aktuellen Verzeichnis aus dem Claude-Setup Template. Kopiert Files, fragt Tech Stack interaktiv ab, füllt CLAUDE.md mit Stack + offiziellen Doku-URLs, ersetzt {{PROJECT_NAME}}/{{PROJECT_PURPOSE}}.
---

# /init-from-template

Initialisiert das aktuelle Verzeichnis aus dem `Claude-Setup` Template.

**Template-Pfad (hardcoded):** `/home/oogway/GitHub/Claude-Setup`

## Vorgehen

### 1. Vorbedingungen prüfen

- Aktuelles Verzeichnis: `pwd`
- Wenn `.claude/` oder `CLAUDE.md` **bereits existiert**: User fragen via `AskUserQuestion`, ob überschreiben.
  - Default: abbrechen, Hinweis auf `/upgrade-template` für selektives Update.
- Wenn Template-Pfad nicht existiert: Fehler ausgeben mit Verweis auf README-Installations-Sektion (`sed`-Command, anderen Pfad setzen).

### 2. Files kopieren

```bash
TEMPLATE=/home/oogway/GitHub/Claude-Setup
cp -i "$TEMPLATE/.gitignore" .
cp -i "$TEMPLATE/.mcp.json" .
cp -i "$TEMPLATE/CLAUDE.md" .
cp -i "$TEMPLATE/CLAUDE.local.md.example" .
cp -ri "$TEMPLATE/.claude" .
```

`README.md` **nicht** automatisch kopieren — separate Frage in Schritt 7.

### 3. Stack auto-detecten

Scan nach Indikatoren im aktuellen Verzeichnis (`Glob` / `Read`):

| Sprache/Framework | Indikator |
|-------------------|-----------|
| JavaScript/TypeScript | `package.json` (Dependencies + Versionen extrahieren) |
| Python | `pyproject.toml`, `requirements.txt`, `Pipfile`, `setup.py` |
| Rust | `Cargo.toml` |
| Go | `go.mod` |
| PHP | `composer.json` |
| Ruby | `Gemfile` |
| Java/Kotlin | `pom.xml`, `build.gradle*` |
| .NET | `*.csproj`, `*.sln` |
| Container/Infra | `Dockerfile`, `docker-compose.*`, `*.tf`, `*.k8s.yaml` |
| Framework-Configs | `next.config.*`, `vite.config.*`, `tailwind.config.*`, `astro.config.*`, `nuxt.config.*`, `svelte.config.*` |

Für `package.json`: alle `dependencies` + `devDependencies` lesen, Versionen mitnehmen.

### 4. Verifikation via AskUserQuestion

Erkannte Stack-Items mit Versionen auflisten. Multi-Select-Question:
"Welche Stack-Komponenten sollen in CLAUDE.md aufgenommen werden? (Default: alle erkannten)"

Bei nichts erkannt: einzelne Frage "Welcher Stack ist im Einsatz?" mit Freitext-Option.

### 5. Doku-URLs ergänzen

Für jede bestätigte Lib offizielle URL ableiten:
- React → https://react.dev
- Next.js → https://nextjs.org/docs
- Vue → https://vuejs.org/guide/
- Svelte → https://svelte.dev/docs
- TypeScript → https://www.typescriptlang.org/docs/
- Tailwind → https://tailwindcss.com/docs
- Vitest → https://vitest.dev/guide/
- Jest → https://jestjs.io/docs/getting-started
- Pytest → https://docs.pytest.org/
- Django → https://docs.djangoproject.com/
- FastAPI → https://fastapi.tiangolo.com/
- Express → https://expressjs.com/
- (etc. — sinnvoll ableiten, bei Unsicherheit User fragen)

### 6. Platzhalter füllen

Per `AskUserQuestion`:
- `{{PROJECT_NAME}}` — Default: Basename des aktuellen Verzeichnisses (`basename $(pwd)`)
- `{{PROJECT_PURPOSE}}` — kurzer Satz

Dann in `CLAUDE.md`:
- `Edit` mit `replace_all: true` für `{{PROJECT_NAME}}`
- `Edit` mit `replace_all: true` für `{{PROJECT_PURPOSE}}`

### 7. CLAUDE.md Tech-Stack-Sektion ausfüllen

Den Block:
```
_Noch nicht verifiziert — Claude führt Auto-Detect beim nächsten Start aus._
```
ersetzen durch:
```
- **Framework:** <Name> <Version> — Doku: <URL>
- **Sprache:** ...
- **Styling:** ...
- **Testing:** ...
```

Zusätzlich auskommentierte Beispiele oberhalb können stehen bleiben oder entfernt werden — User-Wahl.

### 8. README-Behandlung

`AskUserQuestion`:
- "Template-README behalten als Referenz?" → nichts tun
- "Durch Minimal-README für `{{PROJECT_NAME}}` ersetzen?" → Write minimaler README mit Projekt-Name + Quick-Start + Hinweis auf CLAUDE.md

### 9. Optional `git init`

Wenn kein `.git/` vorhanden: `AskUserQuestion` "`git init` ausführen?" → bei Ja: `git init && git add -A` (kein Commit).

### 10. Abschluss

- `git status` (oder bei keinem git: `ls -la`) ausgeben
- Kompakter Bericht:
  - Kopierte Files
  - Gefüllte Platzhalter
  - Stack-Items in CLAUDE.md
- **Nächste Schritte** anzeigen:
  1. `cp CLAUDE.local.md.example CLAUDE.local.md` für persönliche Prefs
  2. `.mcp.json` Beispiele prüfen, MCP-Server bei Bedarf einkommentieren
  3. `.claude/settings.json` Permissions an Stack anpassen
  4. Optional `/audit-claude-setup` für Endprüfung

## Regeln

- **Niemals committen** — User-Hoheit.
- **`cp -i`** verwenden, damit bei vorhandenen Files nachgefragt wird.
- Bei Fehlern in einzelnen Schritten: stoppen, melden, nicht weitermachen.
- Wenn User Stack-Frage skippt: CLAUDE.md Tech-Stack-Block unangetastet lassen (bleibt "_Noch nicht verifiziert_") — wird beim nächsten Session-Start nochmal abgefragt.

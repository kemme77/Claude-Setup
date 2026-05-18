---
description: Kopiert neue oder geänderte Skills, Subagents und Slash-Commands aus dem aktuellen Projekt zurück ins Claude-Setup Template-Repo. Zeigt Diffs, fragt vor Überschreiben nach, committet nie automatisch.
---

# /sync-to-template

Backport von projektspezifischen Verbesserungen ins Template.

**Source:** aktuelles Projekt-Verzeichnis (`pwd`)
**Target:** `/home/oogway/GitHub/Claude-Setup`

## Vorgehen

### 1. Sanity-Check

- Wenn `pwd` == Target-Pfad: abbrechen ("Du bist bereits im Template-Repo — nothing to sync.").
- Wenn Target nicht existiert: Fehler + Hinweis auf README-Installation.
- Wenn aktuelles Projekt kein `.claude/` hat: abbrechen ("Kein Claude-Setup im aktuellen Projekt.").

### 2. Diff aufbauen

Für jeden der drei Pfade:

- `.claude/agents/*.md`
- `.claude/skills/*/SKILL.md` (plus weitere Files im selben Skill-Ordner)
- `.claude/commands/*.md`

Pro Datei drei mögliche Zustände:

- **neu** — existiert nur lokal
- **geändert** — existiert in beiden, Inhalt unterscheidet sich
- **identisch** — Inhalt gleich → ignorieren

Vergleich via Bash:

```bash
diff -q "$LOCAL/datei" "$TEMPLATE/datei"
```

### 3. Kandidaten-Liste

Tabelle ausgeben:

```text
Neu:
- .claude/agents/foo.md

Geändert:
- .claude/skills/bar/SKILL.md
```

Wenn nichts zu syncen: Abbruch mit "Keine Änderungen gegenüber Template."

### 4. AskUserQuestion (multiSelect)

Alle Kandidaten als Optionen. User wählt Teilmenge. Default-Empfehlung: alle neuen, geänderte einzeln entscheiden.

### 5. Pro Auswahl Diff zeigen + Confirm

Bei **neu**: kurz Inhalt zeigen (head), kopieren ohne weitere Frage.

Bei **geändert**: `diff -u` ausgeben, dann separate `AskUserQuestion` "Diese Änderung ins Template übernehmen?" — bei Ja: kopieren.

### 6. Kopieren

```bash
cp "$LOCAL/datei" "$TEMPLATE/datei"
```

Bei Skills mit Multi-File-Ordner: ganzen Ordner rekursiv (`cp -r`).

### 7. Abschluss

```bash
git -C /home/oogway/GitHub/Claude-Setup status
```

Bericht:

- Kopierte Files (Liste)
- Übersprungene Files (Liste)
- Hinweis: "Im Template committen: `cd /home/oogway/GitHub/Claude-Setup && git add -A && git commit`"

## Regeln

- **Niemals committen.**
- Bei `geändert`: niemals ohne explizite Bestätigung überschreiben.
- Bei sensiblen Inhalten (API-Keys, Secrets im Skill-Body): User warnen, nicht kopieren.
- Wenn User abbricht zwischendurch: bereits kopierte Files bleiben, Status klar berichten.

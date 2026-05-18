---
description: Beispiel-Slash-Command. Zeigt Aufbau einer Custom Command Datei. Mit /example aufrufbar.
---

# /example

Dies ist ein Beispiel-Slash-Command. Aufruf via `/example` in einer Claude Code Session.

## Was passiert

Wenn der User `/example` ausführt, befolgst du folgende Schritte:

1. Lies `CLAUDE.md` und fasse den aktuellen Tech Stack in maximal 3 Bullet Points zusammen.
2. Liste die im Repo verfügbaren Subagents auf (alle Dateien in `.claude/agents/`).
3. Liste die im Repo verfügbaren Skills auf (alle `SKILL.md` in `.claude/skills/*/`).
4. Gib eine kurze Übersicht aus.

## Eigenen Slash Command erstellen

```bash
$EDITOR .claude/commands/mein-command.md
```

Mindest-Frontmatter:

```yaml
---
description: Was dieser Command tut — wird im /help Menü angezeigt.
---
```

Danach folgt der eigentliche Prompt/Instruktionssatz für Claude.

## Referenz

- [Slash Commands Doku](https://docs.claude.com/en/docs/claude-code/slash-commands)

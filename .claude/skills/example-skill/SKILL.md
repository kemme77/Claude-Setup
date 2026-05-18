---
name: example-skill
description: Beispiel-Skill für dieses Template. Zeigt Aufbau, Frontmatter und Trigger-Beschreibung. Aktiviere ihn als Vorlage für eigene Skills — Beschreibung anpassen, damit Claude weiß, wann der Skill relevant ist.
---

# Example Skill

Dieser Skill demonstriert die Struktur eines Custom Skills. Claude lädt ihn, wenn die im `description`-Feld beschriebene Aufgabe in einer Anfrage auftaucht.

## Wann nutzen

- Wenn der User explizit `/example-skill` aufruft.
- Wenn die Anfrage zur Beschreibung im Frontmatter passt.

## Was dieser Skill tut

Im echten Skill stehen hier konkrete Schritte/Anweisungen, die Claude befolgt:

1. **Schritt 1** — Was zuerst geprüft wird.
2. **Schritt 2** — Welche Tools verwendet werden.
3. **Schritt 3** — Wie das Ergebnis präsentiert wird.

## Beispiel-Vorlage für eigene Skills

Eigenen Skill anlegen:

```bash
mkdir -p .claude/skills/mein-skill
$EDITOR .claude/skills/mein-skill/SKILL.md
```

Mindest-Frontmatter:

```yaml
---
name: mein-skill
description: Klare, spezifische Beschreibung wann dieser Skill greift. Je präziser, desto besser triggert Claude ihn.
---
```

## Referenz

- [Skills Doku](https://docs.claude.com/en/docs/claude-code/skills)

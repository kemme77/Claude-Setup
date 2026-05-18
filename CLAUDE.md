# Projekt-Kontext

> Diese Datei wird beim Start jeder Claude Code Session automatisch geladen. Sie ist die kanonische Quelle für Projekt-Kontext, Tech Stack und Konventionen. Persönliche Overrides gehören in `CLAUDE.local.md` (gitignored).

## Projekt

- **Name:** {{PROJECT_NAME}}
- **Zweck:** {{PROJECT_PURPOSE}}
- **Status:** <PLATZHALTER — z.B. early development / production / maintenance>

## Tech Stack

> **Auto-Detect Workflow für Claude (verbindlich):**
>
> 1. **Wenn diese Sektion leer oder unverifiziert ist**, scannt Claude beim Session-Start das Repo nach folgenden Indikatoren:
>    - JavaScript/TypeScript: `package.json`, `tsconfig.json`, `next.config.*`, `vite.config.*`, `tailwind.config.*`, `astro.config.*`, `nuxt.config.*`, `svelte.config.*`
>    - Python: `pyproject.toml`, `requirements.txt`, `setup.py`, `Pipfile`, `poetry.lock`
>    - Rust: `Cargo.toml`
>    - Go: `go.mod`
>    - PHP: `composer.json`
>    - Ruby: `Gemfile`
>    - Java/Kotlin: `pom.xml`, `build.gradle*`
>    - .NET: `*.csproj`, `*.sln`
>    - Container/Infra: `Dockerfile`, `docker-compose.*`, `terraform/*.tf`, `*.k8s.yaml`
> 2. Claude listet erkannte Stack-Komponenten **inklusive Versionen** und fragt den User per `AskUserQuestion` zur Verifikation.
> 3. Nach Bestätigung trägt Claude den Stack unten ein **und fügt für jede Library/jedes Framework die offizielle Dokumentations-URL hinzu**.
> 4. Diese URLs sind ab sofort die **kanonische Wissensquelle**. Bei Unsicherheit über API/Verhalten **nicht aus Memory raten** — stattdessen `WebFetch` auf die hinterlegte Doku-URL.
> 5. Bei späterer Stack-Änderung: URLs ergänzen/entfernen, dann neu verifizieren lassen.

### Verifizierter Stack

<!-- Wird beim ersten Start automatisch ausgefüllt. Format: -->
<!--
- **Framework:** Next.js 15.0.3 — Doku: https://nextjs.org/docs
- **Sprache:** TypeScript 5.6.3 — Doku: https://www.typescriptlang.org/docs/
- **Styling:** Tailwind CSS 4.0 — Doku: https://tailwindcss.com/docs
- **Testing:** Vitest 2.1 — Doku: https://vitest.dev/guide/
-->

_Noch nicht verifiziert — Claude führt Auto-Detect beim nächsten Start aus._

## Konventionen

<PLATZHALTER — z.B.:>
- Code-Stil: Prettier / Black / gofmt
- Naming: <Konvention>
- Imports: <Reihenfolge / absolute vs. relative>
- Kommentar-Policy: nur WHY, nicht WHAT

## Testing

<PLATZHALTER — z.B.:>
- Test-Runner: <vitest / pytest / jest / go test>
- Test-Befehl: `npm test` / `pytest` / etc.
- Coverage-Ziel: <X%>
- Wann Tests Pflicht: <neue Features / Bugfixes / Refactor>

## Git Workflow

<PLATZHALTER — z.B.:>
- Branch-Namen: `feature/<kebab-case>`, `fix/<ticket>`
- Commit-Stil: Conventional Commits / freier Text
- PRs gegen: `main`
- Niemals `--force` auf `main`/`master`

## Wichtige Pfade

<PLATZHALTER — z.B.:>
- `src/` — Anwendungscode
- `tests/` — Tests
- `docs/` — Dokumentation
- `scripts/` — Build-/CI-Helper

## Persönliche Overrides

Individuelle Präferenzen (Sprache, Antwortstil, IDE-Hinweise) gehören in `CLAUDE.local.md`. Die Datei ist gitignored und wird zusätzlich zu dieser geladen.

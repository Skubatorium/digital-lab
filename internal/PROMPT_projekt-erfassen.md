# Prompt — Projekt für das „Agentic Projects"-Showcase erfassen

> **So benutzt du das:** Öffne in Cowork einen neuen Chat **im Repo des Projekts**, das
> du erfassen willst (oder im `digital-lab`-Repo mit Zugriff auf den ganzen `Repos`-Ordner),
> und kopiere den Block unten (ab „--- PROMPT START ---") als erste Nachricht hinein.
> Der Prompt erzeugt **eine standardisierte Projektdatei** im exakt gleichen Schema wie die
> bestehenden Dateien in `digital-lab/projects/` — damit das Showcase sie ohne Anpassung lesen kann.

---

--- PROMPT START ---

Du erfasst dieses Repository als **Projekt-Datenblatt** für mein „Agentic Projects"-Showcase.
Ziel ist eine einzige, faktentreue Markdown-Datei im unten definierten Schema. **Erfinde nichts** —
jede Angabe muss aus den Dateien oder der Git-Historie belegbar sein. Wenn etwas unklar ist,
lass das Feld leer oder schreib „unbekannt", statt zu raten.

## 1. Exploriere diese Quellen (in dieser Reihenfolge)

1. `README.md` — Projektbeschreibung, Stack, Features
2. `CLAUDE.md` und alle `*.md` im Wurzelverzeichnis — Rolle, Regeln, Plan
3. `docs/` rekursiv — vor allem `plans/`, `specs/`, `features/`, `architecture*`, `decisions*`, `design/`
4. `.claude/agents/` — Namen und Rollen der Agenten (für den „Agentic Flow")
5. `.claude/commands/`, `.claude/hooks/` — Slash-Commands, Pipeline-Hooks
6. `package.json` / `requirements.txt` / `Makefile` — Tech-Stack, Scripts, Deployment
7. Git (falls vorhanden):
   - erster Commit: `git log --reverse --format=%ad --date=short | head -1`
   - letzter Commit + HEAD-Hash: `git log -1 --format='%ad %h' --date=short`
   - Anzahl Commits: `git rev-list --count HEAD`
8. Test-Umfang zählen: Unit-/Spec-Dateien (`*.test.*`, `*.spec.*`, `tests/`) und E2E (`*e2e*`, Playwright)

## 2. Erzeuge GENAU dieses Schema

```markdown
---
slug: <repo-ordnername in kebab-case, oder besserer Projekt-Slug>
name: <Anzeigename des Projekts>
tagline: <ein Satz, was es ist>
type: <agentic-assistant | web-app | game | tool | other>
status: <live | active | draft | poc>
accent: "<Hex-Farbe, passend zum Projekt>"
timeframe_start: <YYYY-MM-DD, erster Commit oder Plandatum>
timeframe_end: <YYYY-MM-DD oder ongoing>
planned: <YYYY-MM-DD, Datum des Projektplans falls erkennbar>
repo: <repo-ordnername>
git_tracked: <true | false>
metrics:           # 3–6 aussagekräftige Kennzahlen
  - label: <z.B. Commits>
    value: "<Wert>"
tech:              # konkrete Technologien mit Versionen wenn bekannt
  - <Technologie>
last_scanned: <heutiges Datum YYYY-MM-DD>
source_commit: "<HEAD-Hash @ Datum, oder 'n/a (kein Git)'>"
---

## Fachlich

**Idee.** <Worum geht es, aus fachlicher/nicht-technischer Sicht.>

**Was es tut.** <Der Ablauf / die Nutzung in 2–4 Sätzen.>

**Kernfeatures.**
- <kurz, je 1 Zeile, die Highlights>

**Spezialfeature — <Name>.** <Falls es ein besonders spannendes Feature gibt, eigener Absatz.>

**Agentic Flow.** <Welche Agenten, welcher Flow/Pipeline, wer macht was wann, welche Modelle.>

## Technisch

- **Architektur:** <Monorepo? Frontend/Backend/Worker? Module?>
- **Stack:** <Frameworks, Sprachen, DB, Auth>
- **Qualität:** <Testanzahl Unit/E2E, Linting, CI, Coverage>
- **Deployment:** <Hosting, Backend, Mail, Ports, Datenresidenz>

## Impressionen

<!-- Screenshots in assets/screenshots/<slug>/ ablegen, hier mit Caption referenzieren -->
- file: <dateiname.png>
  caption: <was man sieht>
```

## 3. Wohin schreiben

- **Wenn du Zugriff auf den `digital-lab`-Ordner hast** (ganzer `Repos`-Ordner gemountet):
  schreibe/aktualisiere die Datei direkt unter `digital-lab/projects/<slug>.md`,
  lege den Screenshot-Ordner `digital-lab/assets/screenshots/<slug>/` an (falls nicht vorhanden),
  und trage das Projekt in `digital-lab/MEMORY.md` ein bzw. aktualisiere dort Stand/Datum.
- **Wenn du nur dieses eine Repo siehst:** schreibe die Datei als `showcase-data.md` ins
  Wurzelverzeichnis dieses Repos **und** gib den vollständigen Inhalt im Chat aus, damit ich
  ihn nach `digital-lab/projects/<slug>.md` kopieren kann.

## 4. Zum Schluss

Gib mir eine kurze Zusammenfassung: Projektname, Zeitraum, 3 wichtigste Kennzahlen, und welche
Screenshot-Slots noch leer sind (welche Bilder ich also noch beisteuern sollte).

--- PROMPT END ---

---

## Hinweis für Updates (gelesen vom digital-lab-Agenten)

Wenn ein Projekt bereits in `projects/` existiert und sich nur etwas geändert hat, muss **nicht**
das ganze Repo neu gelesen werden. Vergleiche zuerst `source_commit` / `last_scanned` in der
bestehenden Datei mit dem aktuellen `git log -1`. Lies dann gezielt nur die **neuen** Commits
(`git log <alter-hash>..HEAD`) und die geänderten Dateien, und aktualisiere Metriken,
Kernfeatures und `source_commit`/`last_scanned`. Siehe `MEMORY.md`.

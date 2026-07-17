# Digital Lab — Agentic Projects Showcase

Zentrale Sammelstelle, um meine **Agentic Projects** (KI-gestützt entwickelte Projekte) als
modernes, teilbares Showcase darzustellen — eine Übersichtsseite mit Kacheln, die zu Detailseiten
mit Tabs (Fachlich / Technisch / Impressionen) führen.

## Schnellstart

- **Projekt erfassen:** `internal/PROMPT_projekt-erfassen.md` öffnen, Anleitung oben befolgen.
- **Screenshots beisteuern:** siehe `internal/SCREENSHOTS.md`.
- **Screenshots automatisiert erzeugen lassen** (für ein beliebiges Projekt, nicht manuell
  fotografiert): siehe `internal/AUTOMATED_SCREENSHOTS.md`.
- **Showcase bauen:** in Cowork sagen „Bau das Showcase neu" (liest `projects/*.md` + Screenshots)
  — Ergebnis wird **immer** direkt in `showcase/index.html` in diesem Repo geschrieben, nicht in
  ein Cowork Live-Artifact. Das ist die einzige kanonische Datei, siehe Warnhinweis in `internal/MEMORY.md`.
- **Stand & Index:** in `internal/MEMORY.md`.

## Deployment (GitHub Pages)

`.github/workflows/pages.yml` baut bei jedem Push auf `main` automatisch einen `_site`-Ordner
(nur `showcase/index.html` + `assets/`, plus CNAME) und veröffentlicht ihn über GitHub Pages auf
**lab.osgame.de**. Kein manueller Schritt nötig, dauert meist ein bis zwei Minuten. Alles unter
`internal/` und `projects/` bleibt Repo-intern, wird nie live gestellt.

`.gitlab-ci.yml` ist ein Altlast-Rest aus einer früheren GitLab-Phase des Repos und wird nicht
mehr ausgeführt (Repo liegt seit längerem auf GitHub) - kann bei Gelegenheit entfernt werden.

## Dateien

| Datei / Ordner | Zweck |
|---|---|
| `internal/MEMORY.md` | Index + State-Tracker (zuerst lesen) |
| `internal/PROMPT_projekt-erfassen.md` | Wiederverwendbarer Prompt, um ein Repo standardisiert zu erfassen |
| `internal/SCREENSHOTS.md` | Welche Screenshots gebraucht werden + Ablage |
| `internal/AUTOMATED_SCREENSHOTS.md` | Playbook: Screenshots per Sandbox/Playwright selbst erzeugen (statt manuell) |
| `internal/docs/` | Feedback/QA-Notizen |
| `projects/*.md` | Eine Datei pro Projekt — Quelle der Wahrheit für den Inhalt |
| `assets/screenshots/<slug>/` | Screenshots je Projekt |
| `showcase/` | Gebautes Showcase (HTML), kanonische Live-Datei |
| `.github/workflows/pages.yml` | GitHub-Pages-Build (nur `showcase/index.html` + `assets/` → `_site/`), läuft bei jedem Push auf `main` |
| `.gitlab-ci.yml` | Unbenutzter Rest aus früherer GitLab-Phase, nicht mehr aktiv |

## Erfasste Projekte

MAILO · Maschinenverleih Skubatz · OOZCOD — Document Zoo · Drachenspur · NetObs · MindLoop.
(Bewusst ausgelassen: n26-finance-dashboard, partypilot.)

# Digital Lab — Agentic Projects Showcase

Zentrale Sammelstelle, um meine **Agentic Projects** (KI-gestützt entwickelte Projekte) als
modernes, teilbares Showcase darzustellen — eine Übersichtsseite mit Kacheln, die zu Detailseiten
mit Tabs (Fachlich / Technisch / Impressionen) führen.

## Schnellstart

- **Projekt erfassen:** `internal/PROMPT_projekt-erfassen.md` öffnen, Anleitung oben befolgen.
- **Screenshots beisteuern:** siehe `internal/SCREENSHOTS.md`.
- **Showcase bauen:** in Cowork sagen „Bau das Showcase neu" (liest `projects/*.md` + Screenshots)
  — Ergebnis wird **immer** direkt in `showcase/index.html` in diesem Repo geschrieben, nicht in
  ein Cowork Live-Artifact. Das ist die einzige kanonische Datei, siehe Warnhinweis in `internal/MEMORY.md`.
- **Stand & Index:** in `internal/MEMORY.md`.

## Deployment (GitLab Pages)

`.gitlab-ci.yml` baut bei jedem Push auf den Default-Branch den `public/`-Ordner (nur
`showcase/index.html` + `assets/`) und veröffentlicht ihn über GitLab Pages. Alles unter
`internal/` und `projects/` bleibt Repo-intern, wird nie live gestellt.

## Dateien

| Datei / Ordner | Zweck |
|---|---|
| `internal/MEMORY.md` | Index + State-Tracker (zuerst lesen) |
| `internal/PROMPT_projekt-erfassen.md` | Wiederverwendbarer Prompt, um ein Repo standardisiert zu erfassen |
| `internal/SCREENSHOTS.md` | Welche Screenshots gebraucht werden + Ablage |
| `internal/docs/` | Feedback/QA-Notizen |
| `projects/*.md` | Eine Datei pro Projekt — Quelle der Wahrheit für den Inhalt |
| `assets/screenshots/<slug>/` | Screenshots je Projekt |
| `showcase/` | Gebautes Showcase (HTML), kanonische Live-Datei |
| `.gitlab-ci.yml` | Pages-Build (nur `showcase/index.html` + `assets/` → `public/`) |

## Erfasste Projekte

MAILO · Maschinenverleih Skubatz · OOZCOD — Document Zoo · Drachenspur · NetObs · MindLoop.
(Bewusst ausgelassen: n26-finance-dashboard, partypilot.)

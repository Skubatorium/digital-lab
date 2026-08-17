---
slug: frame-forge
name: FrameForge
tagline: Orchestrierte, reproduzierbare Videoschnitt-Pipeline für Reise- und Projektfilme — Claude Code delegiert an neun spezialisierte Sub-Agenten, die eigentliche Verarbeitung übernehmen Python, FFmpeg und lokale CV-/Audio-Bibliotheken
type: tool
status: active
accent: "#C1443C"
timeframe_start: 2026-07-27
timeframe_end: 2026-08-10
planned: 2026-07-27
repo: frame-forge
git_tracked: true
metrics:
  - label: spezialisierte Sub-Agenten
    value: "9"
  - label: Phasen in der State-Machine
    value: "11"
  - label: Schnitt-Stil-Presets
    value: "12"
  - label: reproduzierbarer Final-Render
    value: "4K"
  - label: automatisierte Tests
    value: "540"
tech:
  - Python 3.12 (uv)
  - FFmpeg / ffprobe — nie direkt, nur über frameforge.render
  - Claude Code (Orchestrator + 9 Sub-Agenten)
  - OpenCV-CV-Metriken (Schärfe, Stabilität, Szenen)
  - Cairo (SVG-Text-Overlays)
  - exiftool
  - pytest + ruff
ais:
  - Claude Code — Orchestrator + 9 Sub-Agenten (Opus für story-architect & qc-reviewer, Sonnet für den Rest)
last_scanned: 2026-08-17
source_commit: "HEAD @ 2026-08-10 (8f2190a)"
---

## Fachlich

**Idee.** FrameForge ist eine orchestrierte Videoschnitt-Pipeline für Reise- und Projektfilme.
Claude Code arbeitet hier als **Orchestrator, nicht als Editor**: er delegiert an neun
spezialisierte Sub-Agenten, die eigentliche Verarbeitung übernehmen Python, FFmpeg und lokale
CV-/Audio-Bibliotheken. Grundidee ist die Trennung von Projekt und Export: Ein *Projekt* ist das
Rohmaterial samt Index und Designsystem (z. B. „norwegen-2026"), ein *Export* ist ein konkretes
Produkt daraus — Teaser, Hauptfilm, Themenfassung. Ein Projekt trägt beliebig viele Exporte, ohne
das Material je erneut zu sichten. Erste vollständige Anwendung war die Reise Norwegen 2026: aus
rund 1.180 gesichteten Video- und Fotoaufnahmen entstanden zwei fertige Filme (Roadtrip Edition,
Drone Edition) sowie die private Website, auf der beide veröffentlicht sind.

**Was es tut.** Der Ablauf ist eine erzwungene Reihenfolge — eine State-Machine mit 11 Phasen von
INIT bis RENDERED, die kein Schritt überspringen kann (ein Hook blockt das auch bei direkten
Bash-Aufrufen). Nach dem Ingest (Scan + Proxy-Erzeugung) sichtet der `media-indexer`-Agent
Keyframes jedes Clips und schreibt Beschreibung, Tags und Rating. Der `design-system`-Agent legt
Farbe, Typo und Motion als Designsystem fest, aus dem automatisch alle Text-Overlays entstehen
(Titel, Bauchbinde, Kapitelkarte, Credits). Für einen Export beschreibt ein Brief Stil-Preset,
Ziellänge sowie Pflicht- und verbotene Shots; `story-architect` baut daraus ein dramaturgisches
Beat-Sheet, `timeline-builder` übersetzt es in eine `timeline.json` — die Single Source of Truth
für Render und NLE-Export. `audio-designer` wählt Musik und Ducking-Kurven, `map-animator` baut
Kartenclips aus GPX-Tracks. Ein `qc-reviewer` prüft die Timeline gegen den Brief, bevor ein
Proxy-Preview entsteht; erst nach expliziter Freigabe rendert `render-engineer` den finalen,
farbkorrigierten 4K-Export.

**Kernfeatures.**
- Automatische <b>Material-Analyse</b> — Schärfe, Stabilität, Belichtung und Szenenerkennung per CV, dazu eine KI-Beschreibung inklusive Tags und Rating je Clip
- <b>Schnitt auf die Musik</b>: der audio-designer synchronisiert Beats und Energiekurve mit der Timeline und regelt O-Ton per Ducking automatisch ab
- <b>Automatische Bauchbinden & Text-Overlays</b> (Titel, Kapitelkarten, Credits) — generiert aus dem Designsystem, nicht von Hand gestaltet
- <b>Color-Grading & 4K-Render</b>: reproduzierbar aus timeline.json, EBU-R128-Loudness-Normalisierung, versionierte Ausgabe, die nie überschrieben wird
- 12 Schnitt-Stil-Presets (u. a. Nordic Cinematic, Punch Teaser, Diary/Handheld) und 9 Design-Themes als Startpunkt
- Kartenanimationen aus GPX-Tracks, optionale Gesichtserkennung (Opt-in), FCPXML-/OTIO-Export für DaVinci Resolve/Final Cut

**Spezialfeature — Token-Disziplin: einmal analysieren, nie vergessen.** Das Rohmaterial umfasst
rund 100 GB — ohne Disziplin würde jeder neue Schnittversuch die komplette Analyse wiederholen.
Jede Datei wird per Hash (SHA-256 aus erstem MB, Größe, Änderungsdatum) genau einmal an ein
Vision-Modell geschickt; ist der Eintrag einmal in `assets.json`, bleibt er es. Vision läuft nur
auf Keyframes (3 pro Clip bei 10/50/85 %, 768 px, JPEG q80, Fotos nur 1 Frame), nie auf ganzen
Verzeichnissen — Abfragen laufen über `frameforge query` mit Filtern, statt hunderte Dateien ins
Kontextfenster zu laden. Previews rendern auf 1080p-Proxies, erst der Final-Render mappt
automatisch auf die 4K-Originale.

**Agentic Flow.** Claude Code ist reiner Orchestrator, nicht Editor: er entscheidet, delegiert an
die passenden Sub-Agenten und prüft Ergebnisse — FFmpeg-Aufrufe von Hand sind per Hook blockiert.
Kreative Schritte (Material beschreiben, Designsystem, Story → Timeline) laufen über Claude und
die Sub-Agenten, deterministische Schritte (Ingest, Preview, Render, NLE-Export) direkt über die
`frameforge`-CLI. Ein `/ff-wizard` führt Schritt für Schritt durch die komplette Pipeline und
merkt sich jederzeit unterbrechbar den Stand.

## Technisch

- **Architektur:** CLI-Werkzeugschicht (`frameforge/`) + Claude-Code-Sub-Agenten (`.claude/agents/`) + Slash-Commands (`/ff-*`); ein Gate-Hook (`.claude/hooks/gate.py`) erzwingt die 11-Phasen-State-Machine auch bei direkten Bash-Aufrufen.
- **Projekt vs. Export:** Rohmaterial liegt extern (`media_root` in `project.yaml`, Repo bleibt klein); ein Projekt (Material + Index + Designsystem) trägt beliebig viele Exporte, ohne erneuten Ingest/Index/Design.
- **Reproduzierbarkeit:** `timeline.json` ist Single Source of Truth für Render und FCPXML-/OTIO-Export; die Freigabe bindet an den exakten Timeline-Stand — jede Änderung danach verlangt erneutes Preview + Approve.
- **Caching:** Hash-basierte Analyse (SHA-256 aus erstem MB + Größe + mtime), Keyframe-only Vision-Calls, Musik-Analyse (BPM/Energie) gecacht — kein Asset wird zweimal an ein Modell geschickt.
- **Qualität:** 540 Testfunktionen über 30 Testdateien (pytest), Ruff-Linting, `frameforge doctor` prüft ffmpeg/ffprobe/exiftool/libcairo/Python-Version vor jedem Pipeline-Schritt.
- **Betrieb:** läuft vollständig lokal (macOS/Homebrew: ffmpeg, exiftool, cairo), kein Cloud-Dienst; entstanden zwischen dem ersten Prototyp Ende Juli und dem aktuellen Stand Mitte August 2026.

## Impressionen

<!-- Screenshots in assets/screenshots/frame-forge/ ablegen. FrameForge selbst hat keine
     grafische Oberfläche (läuft über Claude Code + CLI) — gezeigt wird stattdessen ein
     tatsächliches Produkt aus der Pipeline: die private Reise-Website "Norwegen 2026". -->
- file: website-startseite.png
  caption: Ein Produkt aus der Pipeline — die private Reise-Website „Norwegen 2026" (FrameForge selbst hat keine grafische Oberfläche, läuft über Claude Code und die CLI)
- file: website-roadtrip-edition.png
  caption: Roadtrip Edition — 18 Tage chronologisch geschnitten, 167 von 1.181 gesichteten Aufnahmen
- file: website-drone-edition.png
  caption: Drone Edition — reiner Drohnenfilm ohne Text und Kommentar, nur die Landschaft
- file: poster-roadtrip-edition.jpg
  caption: Poster-Frame der Roadtrip Edition (18:00 min)
- file: poster-drone-edition.jpg
  caption: Poster-Frame der Drone Edition (9:28 min)

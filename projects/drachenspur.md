---
slug: drachenspur
name: Drachenspur
tagline: Browserbasierter Dungeon-Crawler mit echten 3D-Würfeln, basierend auf dem Brettspiel Karak — gebaut für meinen Sohn zu seinem sechsten Geburtstag
type: game
status: active
accent: "#6E57B0"
timeframe_start: 2026-04-21
timeframe_end: 2026-06-10
planned: 2026-04-21
repo: osgame
git_tracked: true
metrics:
  - label: Unit-Tests
    value: "345"
  - label: E2E-Tests
    value: "43"
  - label: Agenten
    value: "7"
  - label: Spieler
    value: "1–3"
tech:
  - PixiJS v8 (2D)
  - three.js + cannon-es (3D-Würfel-Physik)
  - TypeScript (strict)
  - Vite 8
  - Howler.js (Audio)
  - Zod
  - Vitest + Playwright
  - Supabase + Vercel
ais:
  - Claude Code — Programmierung
  - ChatGPT — Grafiken aus Asset-Manager-Prompts
  - ElevenLabs — Soundeffekte & Hintergrundmusik
last_scanned: 2026-06-16
source_commit: "HEAD @ 2026-06-10"
---

## Fachlich

**Idee.** Drachenspur ist die digitale Version unseres Lieblings-Brettspiels **Karak**. Mein Sohn
und ich spielen Karak oft zusammen — ich wollte ihm eine eigene Welt schaffen, in der er der Held
ist. Spieler ziehen reihum Dungeon-Plättchen, erkunden gemeinsam, besiegen Monster im Würfelkampf
und sammeln Schätze, bevor am Ende der Drache erscheint.

**Was es tut.** Ein vollständiges Spiel für **1–3 Spieler** mit Charakterauswahl, Dungeon-
Erkundung, 3D-Würfel-Kämpfen, persistenten Spielständen, Statistiken und einem umfangreichen
Kompendium. Jede Partie wird aufgezeichnet und ist sehr crash-sicher sowie jederzeit pausierbar
(Autosave beim Start und nach jedem Zug).

**Kernfeatures.**
- Eigene Helden & Welt mit Charakterauswahl, basierend auf Karak
- Physikalisch simulierte 3D-Würfel im Kampf — jeder Wurf eine echte Physik-Simulation
- Kompendium mit 6 Tabs: Helden, Monster, Waffen, Schätze, Regeln, Welt
- Statistiken pro Held und Monster, EndScreen mit Scoreboard
- Speichern & Fortsetzen mit Archiv (inkl. Savegames & Statistiken)
- Themebar: über den Asset-Manager lässt sich eine komplett neue Welt gestalten

**Spezialfeature — Themebar dank Asset-Manager.** Über ein Admin-Panel lassen sich alle Assets
(136 Grafiken sowie 82 eigene Soundeffekte, Musik- und Sprachdateien) austauschen — man könnte
damit eine völlig neue Welt bauen. Die Spiellogik ist als eigene, UI-freie Engine strikt von der
Darstellung getrennt.

**Agentic Flow.** Zuerst ein Plan, dann Umsetzung durch 7 Agenten in fester Reihenfolge:
Plan & Regelwerk (game-designer) → Anforderungen (requirements-engineer) → Design & Grafik-Check
(designer) → Asset-Prompts → Grafik (asset-coordinator) → Entwicklung (senior-dev) → QA & Autoplay-Bot
(quality-engineer) → Code-Review (code-reviewer). Eingesetzte KIs: **Claude Code** (Programmierung),
**ChatGPT** (Grafiken aus Asset-Manager-Prompts), **ElevenLabs** (Soundeffekte & Musik).

## Technisch

- **Architektur:** UI-freie Engine (Spielzustand, Kampf, Aktionen, Inventar, Zufall) sauber von der Darstellung getrennt; Spielinhalte als JSON mit Zod-Schemas.
- **Rendering:** PixiJS v8 für die 2D-Welt; three.js + cannon-es für die echte 3D-Würfel-Physik im Kampf.
- **Autoplay-Bot:** ein selbstspielender Bot lief dauerhaft durch den Dungeon, um automatisch Bugs zu finden (Stuck-Detection + Logging). Kein Kernfeature, sondern QA-Werkzeug.
- **Sprache/Build:** TypeScript strict, Vite 8.
- **Persistenz:** Spielstände im Browser (localStorage, laufend + Archiv); Assets in IndexedDB; Supabase für Accounts.
- **Qualität:** 345 Unit-Tests (Vitest) + 43 E2E-Tests (Playwright), Code-Analyse, Content-Validierung.

## Impressionen

<!-- Screenshots in assets/screenshots/drachenspur/ -->
- file: start.png
  caption: Startbildschirm mit Hauptmenü
- file: heldenportraet.png
  caption: Charakterauswahl — jeder Held hat zwei Spezialfähigkeiten
- file: abenteuer-beginnt.png
  caption: Das Abenteuer beginnt — hinab in den Dungeon
- file: game.png
  caption: Dungeon-Erkundung in der Draufsicht
- file: sieg.png
  caption: Würfelkampf gewonnen
- file: compendium-monster.png
  caption: Kompendium — Monster-Übersicht
- file: compendium-held.png
  caption: Kompendium — Heldenprofil
- file: statistics.png
  caption: Statistiken — Gesamt- und Heldenwerte
- file: admin-grafik.png
  caption: Asset-Manager — Grafiken (Admin-Panel)

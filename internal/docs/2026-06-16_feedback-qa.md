# QA- & Feedback-Plan — Showcase Überarbeitung (16.06.2026)

Christians Feedback zur ersten Showcase-Version, übersetzt in konkrete Maßnahmen.
Status: ✅ umgesetzt · ⏳ offen/nachgelagert.

## Global / Hero
- ✅ Claim „Software, gebaut mit KI-Agenten" raus → neuer Claim rund um **private Projekte & Lernen** („Ideen selbst umsetzen").
- ✅ Hero-Fakten: **keine Commits mehr** (sagen nichts über Qualität/Umfang) → 4 Projekte · 4 Domänen · 16 spezialisierte Agenten.
- ✅ Hero-Text knapper, Hobby-/Freizeit-Charakter betont.
- ⏳ **Hero-Grafik**: Slideshow-Bühne rechts angelegt (faded links ins Helle). Aktuell abstrakte Platzhalter, später 3er-Screenshot-Slideshow (5 s Wechsel), Vorbild maschinenverleih-skubatz.de.
- ✅ Wortmarke: „Christian Skubatz" (Punkt entfernt).

## Navigation
- ✅ „Über mich" aus der Navigation entfernt (laut Feedback unwichtig).
- ✅ Nav reduziert auf **Projekte · Kontakt**.

## Projekt-Übersicht
- ✅ Hover-Effekt: **kein Verrutschen nach rechts** mehr → sanfte Einfärbung + Akzentbalken + Pfeil.
- ✅ Doppellinie unter „Projekte" behoben (erste Zeile ohne zusätzliche Oberlinie).
- ✅ Besserer Section-Claim + kurzer Einleitungstext.
- ✅ Monat / Typ / Status bleiben.

## Inhaltliche Korrekturen je Projekt
### OOZCOD
- ✅ Tagline erweitert (inkl. eingebautem Chatbot „Oozy").
- ✅ Metriken: „~70 Dokumente" → **100+ verarbeitet**; schwache OCR-Sprachen-Metrik raus.
- ✅ „Steuer-Sphären" → verständlich **Steuer-Kategorien**.
- ✅ Features schärfer: Zuweisungs-/Freigabeprozess, automatischer Steuer-Export, Konto-Checker, Oozy-Bot.
- ✅ Technik: „Makefile-Frisch-Start auf Port 3000" entfernt.
- ✅ Betrieb: „Privacy-by-Design"-Framing entschärft → „lokal via Docker; der KI-Aufruf ließe sich auf ein lokales Modell umstellen".

### Drachenspur
- ✅ Tagline: **kein „Coop"**; basiert auf **Brettspiel Karak**, **für jüngere Helden / meinen Sohn**.
- ✅ Spielerzahl **1–3** (war fälschlich 1–4).
- ✅ Testzahlen korrigiert: **345 Unit- / 43 E2E-Tests** (war 27). LOC- und Commit-Metrik raus.
- ✅ Idee: Karak-Bezug + Geschichte (mit Sohn spielen, eigene Welt schaffen).
- ✅ Autoplay-Bot ist **kein Kernfeature** → in Technik (lief dauerhaft zum Bug-Finden).
- ✅ „Themebar" als Stärke betont (neue Welt über Asset-Manager möglich).
- ✅ Flow als Reihenfolge dargestellt; **eingesetzte KIs** ergänzt: Claude Code (Code), ChatGPT (Grafik aus Asset-Manager-Prompts), ElevenLabs (SFX + Musik).
- ✅ Agenten als saubere Liste.

### MAILO
- ✅ Idee ausgebaut: erst gesamtes Postfach via Cowork aufgeräumt/kategorisiert/gelabelt, dann täglicher Assistent, der morgens Mails prüft und briefed.
- ✅ Metriken: irrelevante „500er/Batches"-Badge raus → seit 2009 / tägliches Morgen-Briefing.

### Maschinenverleih Skubatz
- ✅ Tagline: **Nebengewerbe**, nicht „kommerzielle Plattform für einen Inhaber".
- ✅ Metriken: Commits raus; **86 Unit- / 83 E2E-Tests**, 101 Feature-Specs, 9 Agenten.
- ✅ „Was es tut" prozessorientierter: Buchungssystem, **mobil-optimiert**, digitaler Ausleih-/Rückgabeprozess (am iPad mitführbar). Steuer-Export als **nachgelagert** markiert.
- ✅ Technik: „fester Dev-Port 3100" raus; Jargon (CSP-Hardening etc.) entschärft/erklärt.
- ✅ Agenten-Flow visuell dargestellt.

## Detailseiten allgemein
- ✅ „Fachlich / Technik / Impressionen"-Tabs bleiben (gefallen sehr gut).
- ✅ **Agentic Flow** in jedem Projekt als visuelle Schritt-Kette + Agenten-/KI-Liste.
- ✅ Detailseiten: mehr Text bei Idee/„Was es tut".

## Design
- ✅ Etwas **mehr (weiche) Farbe**, ohne knallig zu werden — Akzentfarben breiter eingesetzt.
- ✅ Helles, luftiges Layout bleibt.

## Footer / Kontakt
- ✅ Große „Lust, mehr zu sehen?"-CTA raus (niemand soll aktiv kontaktieren).
- ✅ Stattdessen **dezenter Footer**: Name · Agentic Projects · 4 Projekte · Stand Juni 2026 (Mail nur klein).

## Datenquellen-Hinweis
Testzahlen aus den Repos gezählt (it/test-Fälle bzw. Pytest); Drachenspur nutzt die im Spiel
angezeigten 345/43. Dokumentenzahl OOZCOD laut Christian (~130, konservativ „100+").

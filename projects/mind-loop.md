---
slug: mind-loop
name: MindLoop
tagline: Internes Lerntool zur Prüfungsvorbereitung auf ISTQB-Zertifizierungen — Karteikarten-Quizzes zum entspannten Lernen, aktiven Üben und einer realistischen, zeitgetakteten Prüfungssimulation
type: web-app
status: active
accent: "#1F6FCC"
timeframe_start: 2026-07-04
timeframe_end: ongoing
planned: 2026-07-04
repo: mind-loop
git_tracked: true
metrics:
  - label: Prüfungsfragen
    value: "150"
  - label: Lern-/Übungs-/Prüfungsmodus
    value: "3"
  - label: zweisprachig
    value: "DE / EN"
  - label: Prüfungssimulation mit Timer
    value: "60 Min"
tech:
  - Next.js 15 (App Router) + TypeScript
  - Prisma 7 + better-sqlite3-Adapter
  - next-intl (DE/EN)
  - Tailwind + shadcn/ui
  - argon2id (Passwort-Hashing)
  - Vitest + Playwright (Smoke)
ais:
  - Claude Code — Programmierung über Agenten-Team
last_scanned: 2026-07-05
source_commit: "HEAD @ 2026-07-04 (109b6fa)"
---

## Fachlich

**Idee.** MindLoop ist ein internes, userbasiertes Lerntool zur Prüfungsvorbereitung auf
ISTQB-Zertifizierungen. Nutzer schreiben sich in einen Kurs ein und üben mit Karteikarten-Quizzes
in drei Modi: entspanntes Durchlesen mit Lösungen, aktives Üben mit Sofort-Feedback und eine
realistische Prüfungssimulation mit Timer. Fortschritts-Statistiken zeigen Stärken und
Wissenslücken je Kapitel. Erster Kurs: **CT-GenAI** ("Testen mit generativer KI") — die Plattform
ist kurs-agnostisch, weitere Kurse lassen sich über einen Content-Ordner einspeisen.

**Was es tut.** Ein Invite-Link erzeugt den Account, danach Login per Passwort. Im Kurskatalog
meldet man sich für einen Kurs an und sieht die Kapitelübersicht mit Fragenzahl und K-Level.
Im Lernmodus liest man Karten mit sofort sichtbarer Lösung, im Übungsmodus (Standard oder
Mastery — falsch beantwortete Fragen kommen häufiger) gibt es sofortiges Feedback, im
Prüfungsmodus zieht das System proportional Fragen über alle Kapitel, bewertet sie serverseitig
und rechnet nach dem echten ISTQB-Punkteschema ab (K1/K2 = 1 Punkt, K3 = 2 Punkte). Am Ende zeigt
eine Statistik-Übersicht, wie stark oder schwach man je Kapitel steht.

**Kernfeatures.**
- Drei Modi: entspanntes <b>Lernen</b>, aktives <b>Üben</b> (Standard/Mastery) mit Sofort-Feedback, realistische <b>Prüfungssimulation</b> mit Timer
- <b>150 geprüfte Fragen</b> über 5 Kapitel für den ersten Kurs (CT-GenAI), K-Level-Mix wie im echten Examen
- Fortschritts-Statistik je Kapitel (stark/mittel/schwach/offen)
- Serverseitige Bewertung im Prüfungsmodus — Antworten werden nie vorab an den Client geleakt
- Zweisprachige Oberfläche (DE/EN), inklusive aller Fragen und Antworten
- Invite-basierte Registrierung, Passwort-Login mit argon2id, kein offener Self-Signup

**Spezialfeature — Content-Pipeline mit Prüf-Nachweis.** Jede Kurs-Grundlage (ein offizieller
Syllabus als PDF) wird seitenweise in ein Wiki verwandelt, mit einem deterministischen
Seiten-Coverage-Beweis (100 % in Deutsch und Englisch). Erst danach entstehen daraus Fragen,
gewichtet nach Unterrichtsminuten und an Lernzielen verankert — und jede Frage durchläuft eine
eigene fachliche Prüfung, bevor sie live geht. Die Kalibrierung gegen ein echtes, offizielles
ISTQB-Probeexamen deckte dabei einen Punkte-Fehler auf (K3-Fragen zählen 2 statt 1 Punkt), der
so ins Bewertungsschema übernommen wurde.

**Agentic Flow.** Ein Orchestrator ruft für Features und für Kursinhalte je eine eigene,
feste Agenten-Kette auf: Features laufen über Requirements-Engineer → Full-Stack-Dev →
Code-Reviewer → QA-Engineer, Kursinhalte über Content-Ingestor → Quizmaster →
Question-Validator.

## Technisch

- **Architektur:** Next.js 15 (App Router, TypeScript) + Prisma 7 mit better-sqlite3-Adapter — kein Rust-Query-Engine-Binary zur Laufzeit nötig, SQLite als Datenbank.
- **Auth:** Invite-Flow + Passwort-Login (argon2id), Sessions (gehashte Tokens), Rollen, CSRF-Schutz (Double-Submit), Rate-Limiting.
- **Sicherheit im Prüfungsmodus:** server-autoritative Bewertung und Timer — Prüfungsantworten verlassen den Server nie vorab.
- **Content:** 150 validierte Fragen über 5 Kapitel, 100 % Seiten-Coverage in DE und EN (71/74 Seiten), Kalibrierung gegen ein offizielles ISTQB-Probeexamen.
- **i18n:** next-intl (DE/EN), Standard-Locale bewusst Englisch, unabhängig vom Browser-Header.
- **Qualität:** rund 49 Vitest-Unit-Tests plus ein schlanker Playwright-Smoke-Test über den kompletten Nutzerfluss (Invite → Login → Enroll → Practice → Exam → Ergebnis → Sprachumschalter).
- **Betrieb:** fester Port 3300 (andere Projekte belegen 3000/3100/3200), automatisches DB-Backup vor destruktiven Kommandos.

## Impressionen

<!-- Screenshots in assets/screenshots/mind-loop/ ablegen -->
- file: dashboard.png
  caption: Dashboard — meine Kurse und Kurskatalog
- file: kurs-uebersicht.png
  caption: Kursübersicht — drei Lernmodi und Kapitelliste
- file: lernmodus.png
  caption: Lernmodus — Frage mit sofort sichtbarer Lösung
- file: uebungsmodus-start.png
  caption: Übungsmodus — Standard oder Mastery wählen
- file: uebungsmodus-frage.png
  caption: Übungsmodus — Frage beantworten
- file: pruefung-start.png
  caption: Prüfungssimulation — Regeln und Start
- file: pruefung-frage.png
  caption: Prüfungssimulation mit Timer und Fragen-Navigator
- file: statistik.png
  caption: Statistik je Kapitel (stark/mittel/schwach/offen)

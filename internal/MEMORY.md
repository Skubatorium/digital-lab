# MEMORY — Digital Lab / Agentic Projects Showcase

> Dies ist das Gedächtnis dieses Ordners. Es hält fest, **was hier passiert**, **welche Projekte
> erfasst sind** und **auf welchem Stand** sie sind — damit Updates inkrementell laufen, ohne dass
> jedes Repo komplett neu gelesen werden muss. Lies diese Datei zu Beginn jeder Sitzung zuerst.

## ⚠️ KRITISCH: Kanonische Datei — hier und nur hier arbeiten

**Die einzige gültige, aktuelle Version des Showcases ist `showcase/index.html` in genau diesem
Repo-Ordner** (`~/Workspace/Repos/digital-lab/showcase/index.html`), geöffnet direkt als lokale
Datei (`file://...`) im Browser. Relative Bildpfade (`../assets/screenshots/<slug>/...`)
funktionieren nur hier.

**Nicht verwechseln mit einem Cowork Live-Artifact** (z. B. unter
`~/Documents/Claude/Artifacts/.../index.html`). Live-Artifacts sind ein separater, isolierter
Ablageort mit eigenem Inhalt — sie können keine relativen Dateipfade auflösen, brauchen daher
eingebettete Base64-Bilder, und sind **nicht** automatisch mit dieser Datei synchron. Am
05.07.2026 kam es dazu, dass ein ganzer Feedback-Durchlauf versehentlich nur in einem solchen
Live-Artifact landete, während `showcase/index.html` unverändert (zeitweise sogar mit Platzhalter-
Inhalt überschrieben) blieb — das darf nicht wieder passieren.

**Regel für jede künftige Sitzung:** Änderungen am Showcase werden ausschließlich in
`showcase/index.html` in diesem Repo vorgenommen. Falls irgendwo ein Live-Artifact zu diesem
Projekt existiert oder erwähnt wird, gilt es als optionale/veraltete Kopie, nicht als
Arbeitsgrundlage — es sei denn, Christian sagt ausdrücklich etwas anderes.

## Was dieser Ordner ist

`digital-lab` (im `Repos`-Ordner) ist die zentrale Sammelstelle für ein **Showcase meiner
Agentic Projects**. Pro Projekt gibt es eine standardisierte Datei in `projects/`. Daraus wird
ein modernes, teilbares **Live-Artifact** gebaut: eine Landing-Page mit Projekt-Kacheln, die zu
Detailseiten mit Tabs (Fachlich / Technisch / Impressionen) führen.

## Ordnerstruktur

```
digital-lab/
  MEMORY.md                     ← diese Datei (Index + State-Tracker)
  README.md                     ← Anleitung / Workflow
  PROMPT_projekt-erfassen.md    ← wiederverwendbarer Prompt zum Erfassen eines Projekts
  SCREENSHOTS.md                ← welche Screenshots gebraucht werden + wie ablegen
  projects/                     ← eine .md pro Projekt (Quelle der Wahrheit für den Inhalt)
  assets/screenshots/<slug>/    ← Screenshots je Projekt (von Christian beigesteuert)
  showcase/                     ← gebautes Showcase (HTML-Build des Live-Artifacts)
```

## Workflow

1. **Neues Projekt erfassen:** `PROMPT_projekt-erfassen.md` in einen Chat im jeweiligen Repo
   kopieren → erzeugt `projects/<slug>.md`.
2. **Screenshots beisteuern:** in `assets/screenshots/<slug>/` ablegen (siehe `SCREENSHOTS.md`).
3. **Showcase bauen/aktualisieren:** „Bau das Showcase neu" — liest `projects/*.md` + Screenshots
   und regeneriert das Live-Artifact.
4. **Update eines bestehenden Projekts:** nur Delta lesen (siehe „Update-Regel" unten).

## Update-Regel (inkrementell)

Bevor ein bestehendes Projekt neu gelesen wird: `source_commit` in `projects/<slug>.md` mit
aktuellem `git log -1 --format=%h` vergleichen. Bei Gleichstand → nichts zu tun. Sonst nur
`git log <alter-hash>..HEAD` + geänderte Dateien lesen, Metriken/Features/`source_commit`/
`last_scanned` aktualisieren. Bei Repos ohne Git: nach Datum / sichtbaren Änderungen entscheiden.

## Showcase-Anzeigereihenfolge

Projekte werden **chronologisch** angezeigt (frühestes zuerst): OOZCOD → Drachenspur → MAILO →
Maschinenverleih → NetObs. Reihenfolge ist die Sortierung des `PROJECTS`-Arrays in
`showcase/index.html`.

## Erfasste Projekte (Stand 2026-07-05, nach MindLoop-Erfassung)

| # | Slug | Name | Typ | Status | Zeitraum | Quelle-Stand | Screenshots |
|---|---|---|---|---|---|---|---|
| 01 | `oozcod` | OOZCOD — Document Zoo | web-app | active | 2026-04-05 → 2026-06-04 | HEAD @ 2026-06-04 | ✅ 9 |
| 02 | `drachenspur` | Drachenspur | game | active | 2026-04-21 → 2026-06-10 | HEAD @ 2026-06-10 | ✅ 9 |
| 03 | `mailo` | MAILO | agentic-assistant | live | 2026-05-16 → laufend | kein Git | ✅ 5 |
| 04 | `maschinenverleih-skubatz` | Maschinenverleih Skubatz | web-app | active | 2026-06-01 → laufend | HEAD @ 2026-06-16 | ✅ 10 |
| 05 | `net-obs` | NetObs | tool | active | 2026-06-23 → 2026-06-26 | kein Git | ✅ 6 |
| 06 | `mind-loop` | MindLoop | web-app | active | 2026-07-04 → laufend | HEAD @ 2026-07-13 (0984eb0) | ✅ 10 |

### MindLoop erfasst + Screenshots automatisiert erzeugt (05.07.2026)

Sechstes Projekt: internes Lerntool zur ISTQB-Prüfungsvorbereitung (Next.js 15 + Prisma 7 mit
better-sqlite3-Adapter, next-intl DE/EN, 150 KI-generierte und geprüfte Fragen). Repo erkundet
(README, Masterplan-Fortschritt, `.claude/agents/`, Prisma-Schema), `projects/mind-loop.md` neu
angelegt, als 6. Karte in `showcase/index.html` ergänzt (Hero-Cycle-Logik war bereits generisch
genug, kein Sonderfall nötig). Hero-Cycle zeigt jetzt Drachenspur zuerst (Christians Wunsch),
danach alle übrigen Nicht-Mailo-Projekte inkl. MindLoop in Array-Reihenfolge. Sektions-Untertext
"Fünf Projekte" → "Sechs Projekte" korrigiert.

**Screenshots wurden diesmal selbst erzeugt, nicht von Christian beigesteuert:** Repo in eine
Sandbox-Kopie geklont, `npm install` mit `--nodedir=/usr` (System-Node-Headers, da nodejs.org für
den Header-Download blockiert war) zum Kompilieren von `better-sqlite3`/`argon2` gebracht,
`prisma generate` übersprungen (bereits vorhandener generierter Adapter-Client reicht, da
Prisma 7 mit Driver-Adapter keine Rust-Query-Engine-Binary braucht — nur `schema-engine` wäre
nötig gewesen und dessen Download war blockiert). Playwright-Chromium ließ sich zunächst nicht
starten (`libXdamage.so.1` fehlte, kein root/apt-Zugriff, arm64-Ubuntu-Mirror geblockt) — behoben
mit einer minimalen Stub-Shared-Library (4 No-op-Funktionen: `XDamageQueryExtension/Create/
Destroy/Subtract`), kompiliert mit `gcc -shared` und per `LD_LIBRARY_PATH` eingebunden. Ein
Node-Skript startete den Dev-Server (`scripts/run-server.mjs`), loggte sich mit einem eigens
angelegten Nutzer ein (`create-admin.ts`, berührt keine bestehenden Nutzer) und lief einmal durch
Login → Dashboard → Kursübersicht → Lernen → Üben → Prüfung → Statistik, mit Full-Page-Screenshots
je Station (Viewport 1440×900). Alles lief in einem einzigen Bash-Aufruf, weil Hintergrundprozesse
diese Sandbox zwischen Tool-Aufrufen nicht überleben. 9 der 10 Rohbilder wurden übernommen (Lern-
und Übungsmodus-Startformular sahen fast identisch aus, nur eines behalten).

### Screenshot-Runde (04.07.2026)
Christian hat für alle fünf Projekte rohe Bildschirmfotos (macOS-Screenshots, unterschiedliche
Ausschnittsgrößen) in `assets/screenshots/<slug>/` abgelegt. Diese wurden gesichtet (Contact-Sheets),
kuratiert, auf max. 1600px Breite herunterskaliert (kein Upscaling, kein Verzerren, Seitenverhältnis
erhalten) und unter sprechenden Dateinamen abgelegt; die Original-Rohdateien liegen unangetastet in
`assets/screenshots/<slug>/_raw/`. In OOZCOD wurden die vier Konto-Checker-Screens (Euro-Beträge,
IBAN, Transaktionsliste) mit echtem Pixelate+Gaussian-Blur unkenntlich gemacht (kein CSS-Filter,
also nicht per Inspector umgehbar) — Dateien: `kontochecker-uebersicht.png`, `kontochecker-monat.png`,
`kontochecker-transaktionen.png`, `kontochecker-tooltip.png`.

Die Detailseiten-Hero-Sektion (`.heroshots` in `showcase/index.html`) wurde neu gebaut: großes
Bild oben mit sanftem Fade-Slideshow (Autoplay alle 4.8s), Auswahl-Thumbnail-Leiste rechts daneben
(anklickbar), Caption-Overlay unten im Bild. Gilt einheitlich für alle fünf Projekte, gespeist aus
`impressionen[]` je Projekt.

### Korrigierte Kennzahlen (Feedback 16.06.2026)
- Drachenspur: **345 Unit- / 43 E2E-Tests** (aus dem Spiel), **1–3 Spieler**, basiert auf **Karak**.
- Maschinenverleih: **86 Unit- / 83 E2E-Tests**, 101 Feature-Specs, 9 Agenten (Commits raus).
- OOZCOD: **100+ Dokumente** verarbeitet (nicht ~70), „Steuer-Sphären" → „Steuer-Kategorien".
- MAILO: „500er/Batches"-Metrik raus → seit 2009 / tägliches Briefing.
- Commits werden nirgends mehr als Kennzahl gezeigt (keine Aussagekraft).
- Feedback-Details + Maßnahmen: siehe `docs/2026-06-16_feedback-qa.md`.

**Bewusst (noch) nicht erfasst:** `n26-finance-dashboard` (raus — würde Christian heute nicht
mehr nutzen), `partypilot` (Draft/POC). Bei Bedarf später als Status `draft`/`poc` aufnehmen.

### Feedback-Runde 2 (04.07.2026) - Design & Content

- **Font:** Fraunces raus (Christian störte sich am "kaputten" f), ersetzt durch Source Serif 4.
- **Keine Gedankenstriche:** — und – siteweit durch normales Minus "-" ersetzt (Christians
  ausdrücklicher Wunsch, gilt für alle künftigen Texte auf der Seite).
- **Homepage-Hero-Banner:** zeigt jetzt reale Screenshots im Cycle statt Farbverlauf; Mailo ist
  aus dem Cycle raus (kein gutes Hero-Motiv); Bildausschnitt ist `object-position:left top` für
  alle außer Drachenspur (dort `center top` - Christian fand die Bildmitte dort am besten).
  Die großen Stat-Zahlen (Projekte/Domänen/Agenten) sind raus, stattdessen ein kleiner Text,
  der mit dem Slide durchcycelt (Projektname + erster Satz der Tagline).
- **Detail-Hero:** kein Slider mehr, nur noch ein statisches, breites Beispielbild (erstes
  Impressionen-Bild), da der Slider sich mit dem Screenshots-Tab doppelte.
- **Tab-Umbenennung:** "Impressionen" heißt jetzt überall "Screenshots".
- **Metrics-Pillen (die großen Zahlen unter der Tagline auf der Detailseite) sind jetzt
  produktorientiert**, nicht mehr dev-lastig: keine Unit-/E2E-Test-Zahlen oder Agenten-Counts
  mehr oben (die stehen weiterhin im Technik-Tab bei "Qualität"). Ausnahme: Mailo - dessen
  Metrics (Mails verarbeitet, Kategorien, Verlauf, Briefing) waren schon produktorientiert und
  blieben unverändert.
- **Screenshot-Grid:** feste Bildhöhe (215px, `object-position:center top`) für ruhiges Layout,
  dünner 1px-Rand um jede Kachel, leichter Verlaufs-Fade unten im Bild.
- **OOZCOD Kontochecker:** von 4 auf 2 Bilder reduziert (kontochecker-monat.png und
  kontochecker-tooltip.png waren Dopplungen, raus); geblieben: kontochecker-uebersicht.png und
  kontochecker-transaktionen.png.
- **Taglines überarbeitet:** OOZCOD ohne Chatbot-Erwähnung im Kurztext, Drachenspur ohne
  "echte 3D-Würfel"-Hype-Ton, NetOps ohne Fachbegriff "intermittierend" - stattdessen die
  Glasfaser-Geschichte (WLAN vs. Leitung vs. Internet) erzählt.
- **"Aktiv"/"Live"-Status-Wort** ist aus den Meta-Zeilen (Homepage-Kacheln + Detail-Hero) raus.
- **"Ansehen"-Pfeil** auf den Homepage-Kacheln ist jetzt unten rechts statt oben rechts positioniert.
- **Lightbox-Viewer bleibt** wie in Runde 1 gebaut (Klick öffnet Vollbild, Pfeiltasten-Navigation).

### Feedback-Runde 3 (04.07.2026) - Detailschliff

- **Hero:** rechte Spalte (Bild + Caption) vergrößert (Grid `.82fr/1.18fr`, Stage-Höhe 340→400px);
  Caption unter dem Bild nutzt jetzt ein kurzes eigenes `heroline`-Feld (max. 2 Zeilen) statt der
  vollen Tagline. Lead-Text: Beispiel "ein Spiel, ein Geburtstagsgeschenk" (Dopplung, das Spiel WAR
  das Geschenk) ersetzt durch "einen Mail-Assistenten, ein Spiel, eine Firmen-Website".
- **Sektions-Headline** "Vom Dokumenten-Manager bis zum Browserspiel." ist jetzt einzeilig
  (kein `max-width`/`text-wrap:balance` mehr auf `.secintro`).
- **"Ansehen"-Pfeil** ist jetzt ein Kind-Element von `.thumb` (bottom-right im Bild selbst,
  dunkles Pill-Badge) statt row-relativ positioniert — dadurch unabhängig von unterschiedlichen
  Zeilenhöhen konsistent platziert.
- **Screenshot-Karten:** Caption-Leiste (`.cap`) hat jetzt dunklen Hintergrund (`var(--ink)`) mit
  hellem Text statt weiß — bessere Abgrenzung bei Screenshots mit viel Weißanteil (OOZCOD, Mailo).
  Trennlinie zwischen Bild und Caption; Bild-Fade unten verstärkt (.14 → .28 Opacity).
- **Neue Navigation:** am Ende jeder Detailseite zusätzlich zu "← Alle Projekte" Quick-Jump-Links
  zu "Technik" und "Screenshots" (`jumpTab()`), damit man nach dem Durchscrollen nicht wieder
  hochscrollen muss.
- **OOZCOD:** Tagline/Idee-Text betonen jetzt allgemeines Dokumentenmanagement statt reinem
  Steuerfokus; Metrik "Steuer-Kategorien" → "Dokumenten-Kategorien"; "does"-Text ohne
  Scan-Annahme (auch heruntergeladene PDFs) und ohne die fachlich unklare "PDF → Bild"-Behauptung;
  Spezialfeature von "Kostenkontrolle" zu "Datenschutz bei der KI-Analyse" (API nicht fürs
  Training, vertrauliche Dokumente markierbar) umgedreht; Flow-Schritt "PDF → Bild" raus,
  "Speichern + Budget" → "Speichern"; Technik-Zeile "Audit-/Hardening-Durchlauf" raus; mehrere
  Screenshot-Captions präzisiert (Filterung statt "Baum", "Belege je Person/Immobilie" statt
  "je Kategorie", Kategorisierung bei Transaktionsliste erwähnt).
- **Drachenspur:** Bilder-Liste von 11 auf 9 reduziert — `character-select.png` und
  `combat-3d-dice.png` entfernt (nach `_raw/` verschoben); `abspann.png` war fachlich falsch
  benannt (zeigt tatsächlich den Einstieg in den Dungeon, kein Abspann) → umbenannt in
  `abenteuer-beginnt.png` und als neues 2. Bild einsortiert; `heldenportraet.png` neu als
  Charakterauswahl-Bild bestätigt; "isometrischer Look" bei `game.png` korrigiert zu "Draufsicht";
  Asset-Zahlen im Spezialfeature korrigiert: **139 Grafiken / 132 Audio-Dateien** (nicht "37
  Sprites" — verifiziert direkt im osgame-Repo, `AdminPanel.ts`); RGB→RGBA-/PixiJS-Render-Detail
  gestrichen (nicht relevant); Metrik "Inkl. Kompendium" → "Inkl. Savegames & Statistiken";
  Tagline ergänzt um "gebaut für meinen Sohn zu seinem sechsten Geburtstag".
- **Mailo:** Uhrzeit 7:30 Uhr jetzt explizit in Tagline/Metrik/Idee-Text; zwei schwache
  Kernfeatures ("Newsletter-Liste statt Auto-Abmeldung", "Datenschutz-Wächter") ersetzt durch
  "Direktes Feedback im Briefing" und "läuft vollautomatisch als Cloud-Routine"; Sicherheits-Text
  präzisiert (Gmail-MCP kann technisch ohnehin nur Entwürfe erzeugen, kein Senden); Agentic-Flow-
  Text ergänzt um "mehrere Sessions und Tage" für die Aufräumphase.
- **Maschinenverleih:** Tagline ausführlicher (Online-Kalender, digitale Übergabe/Rückgabe,
  KI-Rechercheassistent); "Steuer-Export ist nachgelagert" (klang nach unfertig) korrigiert zu
  "kann exportiert werden" (Feature existiert bereits); neue Technik-Zeile "Status: Beta-Tests
  und laufende Geräteeinpflege, noch nicht vollständig live"; Deployment-Zeile ergänzt um
  Lerneffekt-Hinweis zur Supabase/Vercel/Resend-Pipeline.
- **NetOps:** Metrik "4 Messpunkte parallel verglichen" ersetzt durch "FRITZ!Box API-Integration
  (TR-064)"; Spezialfeature "Klassifikations-Matrix" ersetzt durch die reale Anekdote (PlayStation
  Remote Play mit Echtzeit-Priorisierung hat den Netzwerkausfall verursacht, per Tool identifiziert
  und durch WLAN-Routing-Anpassung gelöst); Technik-Zeile "28 von 30 Aufgaben abgeschlossen"
  entfernt (keine Aussagekraft nach außen).
- **About-Sektion:** komplett neu geschrieben — Headline "...die in der agentischen
  Produktlösung aufgeht"; Laufbahn jetzt mit Mediengestalter/Instructional-Designer-Stationen und
  der Anekdote zum eigenen kleinen Softwareunternehmen mit 18 (drei Jahre, "viel Wissen, wenig
  Geld"); Phrase "schnelle Start-ups und Scale-ups" gestrichen; "gutes Gespür" (zu selbstlobend)
  umformuliert zu "gutes Auge für Problemlösungen, Produktentwicklung, Technik, Qualität,
  Usability und Gestaltung"; Schluss ergänzt um "als POC oder als wirklich im Alltag eingesetztes
  Produkt" und "Feilen, Ausprobieren, Bauen, Lernen, Problemlösung".
- Projekt-`.md`-Dateien (`projects/*.md`) entsprechend nachgezogen, damit sie mit dem Showcase
  synchron bleiben.

### Feedback-Runde 4 (06.07.2026) - Abgleich nach Wiederherstellung

Nach dem versehentlichen Überschreiben von `showcase/index.html` (siehe kritischer Hinweis oben)
wurde die Datei über die noch offene Session "NetOps showcase page" wiederhergestellt. Diese
Wiederherstellung war nicht vollständig deckungsgleich mit allen Punkten aus der ursprünglichen
Feedback-Sitzung. Ein vollständiger Abgleich gegen `digital-lab-feedback-plan.md` (alle Punkte der
ursprünglichen Sitzung) plus die inzwischen bestätigten Zahlen ergab folgende Nachträge:

- **OOZCOD:** Kategorien-Zahl 7 → 15 (Seed-Datei hat 13 Kategorien, plus mind. 2 organisch
  hinzugekommene Ordner `family`/`vermietung` ohne Seed-Eintrag, direkt im Repo verifiziert);
  Kernfeature generischer formuliert (keine Einzelaufzählung Typ/Datum); Oozy-Bullet um
  Kontoauszüge ergänzt; Archiv-Filterung um Person/Fahrzeug ergänzt; Special-Block von
  "Budget-Tracking" auf echte Kosten-/Datenschutz-Story umgestellt (~1-2 Cent/Seite, lokales LLM
  als Alternative); Monatslabel Kachel "Apr 2026" → "Apr-Jun 2026".
- **Drachenspur:** Familienbezug nachgetragen (Tagline "Helden aus der eigenen Familie", Idee-Text
  mit Ehefrau); "crash-sicher"-Framing durch Logger-Beschreibung ersetzt; "basierend auf Karak"
  korrekt der Spielmechanik zugeordnet (nicht Helden/Welt); Asset-Zahlen 139/132 → **136 Grafiken /
  83 Audiodateien** (Christians eigene Nachzählung im Repo, verbindlich); Deployment-Zeile in
  Technisch-Tab ergänzt; Monatslabel "Apr 2026" → "Apr-Jun 2026".
- **MAILO:** "über rund 20 Jahre" (Widerspruch) → "rund zwei Jahrzehnte"; "läuft auch bei
  zugeklapptem Laptop" überall entfernt (bestätigt falsch, siehe Recherche); Label-Zahlen überall
  auf **56 Labels / 12 Oberkategorien** aktualisiert (vorher veraltete 53/11), Hinweis auf ~44
  Alt-Labels ergänzt; "um Kontingent zu schonen" gestrichen; Aufräumphase-Dauer "Tage" → "rund drei
  Wochen"; "does"-Text in zwei Absätze (Aufräumen/Tagesbetrieb) geteilt.
- **Maschinenverleih:** **Wichtigster nachgetragener Punkt** - überall ergänzt, dass es das
  gemeinsame Nebengewerbe von Christian und seiner Frau ist (Tagline, Idee-Text, "does"-Text "Wir"
  statt "Ich"); Special-Block korrigiert (kein Kaufbeleg-Upload laut Spec 099, "nichts wird
  gespeichert"-Satz gestrichen); Deployment-Zeile verweist jetzt auf Drachenspur statt "großer
  Lerneffekt"; redundanter Steuer-Export-Zusatz beim Kundenbereich-Bullet entfernt.
- **NetObs:** Zeitraum auf die tatsächlichen **23.-26. Juni 2026** präzisiert; Test-Zahl 122 → 123
  (Christians eigene Nachprüfung); 4. Kennzahl "4 Messpunkte" ergänzt (vorher nur 3 Kennzahlen,
  Inkonsistenz zu allen anderen Projekten).
- **MindLoop:** "Internes" aus der Tagline entfernt; K-Level-Begriff aus nutzerseitigem Text
  entfernt; Kalibrierungs-Satz (ISTQB-Punkte-"Fehler") komplett aus Special-Block, Technisch-Tabelle
  und Flow-Steps gestrichen (war laut Recherche kein Bug, sondern korrekte CT-GenAI-Kalibrierung -
  zu viel Detail für die Showcase-Copy); "60 Min Prüfungssimulation"-Metrik durch "5 Kapitel"
  ersetzt; neues Feature "Frage als fehlerhaft melden" ergänzt; verwaiste "Betrieb"-Zeile
  (Port/Backup-Detail) aus Technisch-Tab entfernt.
- **Global/Feature:** Hero-Bild ist jetzt klickbar (springt zur Projekt-Detailseite), die
  Fortschritts-Punkte liegen in einem eigenen beigen Streifen (`.stagebar`) zwischen Bild und
  Caption und sind unabhängig anklickbar (Slide manuell wechseln, `event.stopPropagation()`).
  Lightbox hat jetzt echtes Zoom/Pan (+/− Buttons, "Original"-Reset, Mausrad-Zoom, Ziehen zum
  Schwenken bei Zoom > 1) zusätzlich zur bestehenden Bild-für-Bild-Navigation (Pfeiltasten/Buttons).
- Nach jeder Änderung: JS-Syntax geprüft (`node --check`), alle 47 Bildreferenzen über alle 6
  Projekte aufgelöst (kein 404-Risiko).

### MindLoop-Screenshots erneuert (09.07.2026)

Alle 8 MindLoop-Screenshots waren vom 05.07.2026 (Commit 109b6fa) - seither lief u. a. Commit
`1b58830` ("fix(design-system): fix serif fallback and undersized headings"): Font-Variable war
nur auf `<body>` statt `<html>` im Scope (Serif-Fallback-Risiko) und `--text-xs` bis `--text-3xl`
waren nie im `@theme`-Block registriert, wodurch Tailwind für alle Überschriften seine eigene, bis
zu ~35 % kleinere Standardskala nutzte statt der Design-System-Werte. Dazu mehrere UI-Änderungen
(4-Schritte-Dashboard, nummerierte Formulare, Kapitel-Fragenzahlen). Dev-Server lokal auf Port 3300
gestartet, dedizierter Screenshot-Nutzer `showcase`/„Show Case" (Rolle Admin) per
`create-admin.ts` angelegt (bestehende Nutzer unangetastet), per Playwright-Skript exakt dieselben
8 Stationen wie beim Original-Durchlauf abgefahren (Login → Dashboard → Kursübersicht → Lernen →
Üben-Start → Üben-Frage → Prüfung-Start → Prüfung-Frage → Statistik, DE-Locale, Viewport 1440×900,
Full-Page) und die 8 PNGs unter identischem Dateinamen in `assets/screenshots/mind-loop/`
überschrieben - keine Datei umbenannt, hinzugefügt oder entfernt. Übungs-/Prüfungssessions bewusst
nicht abgeschlossen (nur erste Frage angezeigt), damit die Statistik-Seite wie im Original bei
0 Stark/Mittel/Schwach bleibt. Der `showcase`-Testnutzer bleibt in der lokalen Dev-DB von MindLoop
stehen (ähnlich den dort bereits vorhandenen `testuser`/`verify-sum-*`-Alt-Accounts) - bei Bedarf
manuell aufräumen.

**Nachtrag selber Tag:** erster Durchlauf nutzte `fullPage: true` (Playwright) - dadurch variable
Bildhöhen je nach Seiteninhalt (z. B. Kursübersicht deutlich höher als Dashboard). Christian wollte
stattdessen durchgehend nur den sichtbaren Viewport (kein Scroll-Stitching), fixe Auflösung
1440×900 für alle 8 Bilder, Pivot oben-links (Logo bleibt oben links stehen, kein mittiger
Beschnitt). Zweiter Durchlauf mit `fullPage: false` über denselben `showcase`-Nutzer/dieselben 8
Stationen wiederholt, Dateien erneut unter identischem Namen überschrieben.

### MindLoop-Update: neue Features nachgezogen, Screenshots erneuert (18.07.2026)

MindLoop war seit der letzten Erfassung (HEAD 109b6fa, 04.07.2026) bis HEAD 0984eb0 (13.07.2026)
weitergebaut worden. Nur dieses eine Projekt wurde in dieser Runde angefasst, auf Christians
ausdrücklichen Wunsch - alle anderen Projekte bleiben unverändert.

Neu in der Showcase-Copy (`projects/mind-loop.md` + `showcase/index.html`, `fachlich.does`/
`features`/`impressionen`): redesignter 3-Schritte-Einstieg auf dem Dashboard (`0a8f51c`); neuer
Info-Lernmodus mit Kapitel-Zusammenfassungen als Wiki-Seite, die am Ende in ein Cheat Sheet und
Merkhilfen (Akronyme, Eselsbrücken) mündet (`8cfa6fe` + Wiki-Content-Erweiterung Spec 026); Statistik-
Seite mit Umschalter "alle Fragen des Kapitels" / "nur beantwortete Fragen" (`0958b65`). Dabei auch
zwei kleine Alt-Inkonsistenzen zwischen `projects/mind-loop.md` und dem bereits weiter fortgeschrittenen
`showcase/index.html` nachgezogen (die Runde vom 06.07. hatte nur die HTML-Datei aktualisiert, nicht
die .md-Quelle): "Internes" aus der Tagline raus, Metrik "60 Min Prüfungssimulation" → "5 Kapitel".

**Screenshots erneuert (Sandbox-Klon, nicht im echten MindLoop-Repo gearbeitet):** Repo in eine
Linux-Sandbox-Kopie geklont (nicht im echten `~/Workspace/Repos/mind-loop`, um dessen node_modules/
dev.db nicht anzufassen), `npm install` mit `--nodedir=/usr`, DB per direkt ausgeführten
`migration.sql`-Dateien aufgebaut (Schema-Engine-Download war durch das Sandbox-Netzwerk-Allowlist
blockiert, `prisma generate` daher übersprungen und der bereits im echten Repo vorhandene generierte
Adapter-Client nach `src/generated/prisma` kopiert - reines TypeScript, keine native Binary, plattform-
unabhängig). `better-sqlite3` mangels Prebuild lokal per `node-gyp --debug` kompiliert (Release-Build
brach am großen `sqlite3.c` regelmäßig am 45-Sekunden-Zeitlimit ab, Debug kompiliert ohne Optimierung
schnell genug). Chromium für Playwright per resumable `curl -C -` besorgt (Playwrights eigener
Downloader unterstützt kein Resume und schaffte die 187 MB nie vor dem Zeitlimit); dabei fiel auf,
dass ein früherer `npm install`-Versuch die `@next/swc-linux-arm64-gnu`-Binary abgeschnitten
heruntergeladen hatte (Bus-Error beim Laden) - Re-Install mit geleertem npm-Cache behoben. Fehlendes
`libXdamage.so.1` wie schon am 05.07. dokumentiert per Vier-Funktionen-Stub-Library gelöst. Login,
Einschreibung und alle 10 Stationen (Dashboard → Kursübersicht → Lernen → Kapitel-Zusammenfassung →
Cheat-Sheet/Merkhilfen-Scroll → Üben-Start → Üben-Frage → Prüfung-Start → Prüfung-Frage → Statistik)
mit dediziertem `showcase`-Testnutzer (Rolle Admin) durchlaufen, Viewport 1440×900, kein
`fullPage`-Scroll-Stitching, wie am 09.07. als Christians Vorgabe festgehalten. Google Fonts
(Figtree/IBM Plex Mono) waren im Sandbox-Netzwerk blockiert - Screenshots zeigen daher System-
Fallback-Schrift statt der echten Design-System-Fonts, Layout/Inhalt sind aber unverändert korrekt.
Ein bestehender App-Bug fiel dabei auf: die Kapitel-Wiki-Seite rendert den Quellen-HTML-Kommentar
(`<!-- Quelle: ... -->`) als sichtbaren Text statt ihn zu verstecken - für den Kapitelzusammenfassung-
Screenshot per DOM-Manipulation vor der Aufnahme ausgeblendet (nur fürs Bild, keine Code-Änderung im
MindLoop-Repo), Christian aber separat gemeldet, da es live so auch für echte Nutzer sichtbar ist.

## Offene Punkte

- [ ] Finaler Showcase-Name steht noch aus (Arbeitstitel: „Agentic Projects").
- [ ] Showcase als Live-Artifact veröffentlichen.
- [ ] Optional: weitere/bessere Rohbilder für Projekte mit dünnerer Screenshot-Auswahl (mailo,
      net-obs) nachreichen, falls gewünscht.

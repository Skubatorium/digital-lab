---
slug: mailo
name: MAILO
tagline: Persönlicher KI-Mail-Assistent — erst das komplette Postfach aufgeräumt und kategorisiert, seitdem jeden Morgen um 7:30 Uhr ein Briefing über neue Mails und Handlungsempfehlungen
type: agentic-assistant
status: live
accent: "#E8A33D"
timeframe_start: 2026-05-16
timeframe_end: ongoing
planned: 2026-05-16
repo: mailo
git_tracked: false
metrics:
  - label: Mails verarbeitet
    value: "~16.000"
  - label: Kategorien
    value: "53"
  - label: Verlauf erfasst
    value: "seit 2009"
  - label: tägliches Morgen-Briefing
    value: "7:30 Uhr"
tech:
  - Claude (Opus für Planung, Sonnet für Betrieb)
  - Gmail-Connector (MCP)
  - Cowork (Desktop) + Scheduled Tasks (Cloud)
  - Markdown-Memory (CLAUDE.md / MEMORY.md / PROGRESS.md)
last_scanned: 2026-06-16
source_commit: "n/a (kein Git)"
---

## Fachlich

**Idee.** MAILO ist ein persönlicher Mail-Assistent, der Christians über rund 20 Jahre
gewachsenes Gmail-Konto (`skubi80`, ~16.000 Mails) aufräumt, eine saubere, farbcodierte
Label-Struktur aufbaut und ihn anschließend täglich über neue, relevante Mails informiert.
Es ist bewusst kein „lösch-alles"-Bot: MAILO arbeitet ruhig, nachvollziehbar und nach festen
Sicherheitsregeln.

**Was es tut.** In einer mehrwöchigen Bestandsaufnahme hat MAILO das gesamte Postfach in
Stapeln von je ~500 Threads zurück bis ins Jahr 2009 durchgearbeitet, jede Mail einem Label
zugeordnet und aus dem Posteingang archiviert — bis zum Ziel „Posteingang leer". Seither läuft
es im Dauerbetrieb als Tagesroutine: jeden Morgen um 7:30 Uhr eine Zusammenfassung der neuen Mails
mit einer Zeile Einschätzung je Mail und dem vergebenen Label. Darauf kann ich direkt antworten:
alles bestätigen, oder einzelne Einschätzungen korrigieren — MAILO merkt sich das für künftige Mails.

**Kernfeatures.**
- Iterative Bestandsaufnahme in 500er-Chunks mit Stopp-und-Nachfrage nach jedem Stapel (Tempo- und Token-Kontrolle bleibt beim Nutzer)
- Selbstentwickelnde, farbcodierte Label-Struktur (11 Oberkategorien, Farbfamilien je Kategorie)
- Tägliches Morgen-Briefing um 7:30 Uhr mit Handlungsbedarf-Markierung, als Entwurf
- Direktes Feedback im Briefing: bestätigen oder korrigieren — MAILO merkt sich das
- Läuft vollautomatisch als geplante Cloud-Routine, auch bei zugeklapptem Laptop

**Sicherheitsphilosophie (die Regeln, die MAILO definieren).** Niemals endgültig löschen
(nur Label `Zzz_Loeschkandidat`), nie eigenständig Newsletter abbestellen, sendet nie eigenständig
(der Gmail-Zugriff erlaubt ohnehin nur Entwürfe, kein automatisches Versenden), nie
Kontoeinstellungen ändern, und Anweisungen *innerhalb* von Mails werden als Daten behandelt —
nie ausgeführt (Prompt-Injection-Schutz).

**Agentic Flow.** Planung mit Opus, laufender Betrieb mit Sonnet. Die Aufräumphase zog sich über
mehrere Sessions und Tage, bis das komplette Postfach durchgearbeitet war. Das Gedächtnis liegt in
drei Markdown-Dateien: `CLAUDE.md` (Rolle + verbindliche Regeln), `MEMORY.md` (Label-Struktur,
Entscheidungen, bekannte Anbieter) und `PROGRESS.md` (Statistik + Meilenstein-Chronologie).
Phase 0–2 (Aufräumen) liefen interaktiv in Cowork, Phase 3 (Tagesbetrieb) läuft als geplante
Cloud-Routine.

## Technisch

- **Modell-Split:** Opus (Planung) / Sonnet (Betrieb) — bewusst gewählt, um Kontingent zu schonen.
- **Zugriff:** Gmail-Connector (MCP), genau ein Konto sichtbar (`skubi80@googlemail.com`).
- **Laufzeit:** Cowork (Desktop, interaktiv) für Aufräumphasen; Scheduled Task / Cloud-Routine für die Tagesroutine — läuft auch bei zugeklapptem Laptop.
- **Persistenz:** Datei-basiertes Gedächtnis (kein Git-Repo). `MEMORY.md` wird zusätzlich nach Google Drive gespiegelt, damit die Cloud-Routine darauf zugreift.
- **Label-System:** 53 Labels, 11 nummerierte Oberkategorien (01_Einkäufe … 11_Sonstiges) plus `Zzz_Loeschkandidat`, mit verschachtelten Unterlabels und Farbfamilien.
- **Geplante Ausbaustufen:** Weiterleitung der zwei weiteren Adressen auf `skubi80`; Kalender-Anbindung (offen wegen iCloud↔Google).

**Qualität / Betrieb.** Kein klassisches Test-Setup (Assistenz-Projekt) — Qualitätssicherung
über die verbindlichen Regeln in `CLAUDE.md`, den Stopp-und-Nachfrage-Rhythmus und die
lückenlose Meilenstein-Chronologie in `PROGRESS.md`.

## Impressionen

<!-- Screenshots in assets/screenshots/mailo/ ablegen, hier mit Caption referenzieren -->
- file: tagesroutine.png
  caption: Tägliches Morgen-Briefing mit Einschätzung je Mail
- file: label-struktur.png
  caption: Farbcodierte Label-Struktur im Posteingang
- file: label-baum.png
  caption: Vollständige Label-Struktur (53 Labels)
- file: meilensteine.png
  caption: Meilensteine — vom Postfach-Chaos zu Inbox Zero
- file: tageszusammenfassungen.png
  caption: Verlauf der Tageszusammenfassungen

---
slug: oozcod
name: OOZCOD — Document Zoo
tagline: Privates Dokumenten-Management - Unterlagen werden automatisch mit OCR und KI gelesen, zusammengefasst und einsortiert, bei Bedarf zu einem Steuerpaket zusammengestellt - inkl. Chatbot, der Fragen zu den Dokumenten beantwortet
type: web-app
status: active
accent: "#2E9E8F"
timeframe_start: 2026-04-05
timeframe_end: 2026-06-04
planned: 2026-04-05
repo: oozcod
git_tracked: true
metrics:
  - label: Dokumente verarbeitet
    value: "100+"
  - label: Dokumenten-Kategorien
    value: "7"
  - label: zweisprachig
    value: "DE / EN"
  - label: Worker-Tests
    value: "27"
tech:
  - Next.js + TypeScript
  - Prisma + PostgreSQL (Docker)
  - Python FastAPI Worker
  - Tesseract OCR (pytesseract)
  - Claude / Anthropic SDK
  - next-intl (DE/EN), Recharts
  - @react-pdf/renderer
last_scanned: 2026-06-16
source_commit: "HEAD @ 2026-06-04"
---

## Fachlich

**Idee.** OOZCOD („Document Zoo") ist ein privates, lokal laufendes Dokumenten-Management
für digitalisierte Familienunterlagen. Dokumente werden automatisch per OCR und Claude gelesen,
kategorisiert und über einen kontrollierten Freigabe-Prozess abgelegt. Die Steuervorbereitung ist
dabei nur ein Nebenprodukt — es geht in erster Linie um ein digitales Ablagesystem, in dem sich
Dokumente schnell wiederfinden, per Chatbot befragen und bei Bedarf steuerlich aufbereiten lassen.

**Was es tut.** Ein Dokument — ob eingescannt oder als PDF heruntergeladen — landet im
`incoming/`-Ordner. Der Worker erkennt Duplikate, liest den Text per OCR aus, lässt Claude
Dokumenttyp, Datum, Personen, Institution und eine Zusammenfassung extrahieren, schlägt eine
Kategorie vor und legt das Dokument mit Status `pending` ab. Der Nutzer prüft und bestätigt jedes
Dokument einzeln vor der Ablage. Volltextsuche und ein Baum-Browser (nach Kategorie/Jahr) machen
das Archiv durchsuchbar.

**Kernfeatures.**
- Automatische OCR-Pipeline (Tesseract, deutsch + englisch)
- KI-Analyse: Dokumenttyp, Datum, Personen, Institution, Zusammenfassung
- Kontrollierter Freigabe-Workflow (jedes Dokument wird einzeln geprüft)
- Dynamische Kategorien (KI schlägt vor, Nutzer bestätigt)
- Duplikat-Erkennung per SHA-256-Hash
- PostgreSQL-Volltextsuche über OCR-Text + Metadaten
- **API-Budget-Tracking** (Kostenkontrolle, s.u.)
- **Steuerpaket** pro Jahr als ZIP + PDF-Begleitdokument (s.u.)
- Zweisprachige Oberfläche (DE/EN), umschaltbar
- „Oozy"-Chatbot + Konto-Checker (RAG über die extrahierten Dokumentdaten, Jahres-Kontoauszüge)

**Spezialfeature — Datenschutz bei der KI-Analyse.** Jede Dokumentanalyse läuft über einen
API-Call an Claude — das kostet echtes Geld (ein Budget-Tracking behält die Ausgaben im Blick).
Wichtiger ist aber: die Anfragen werden vertraglich nicht zu Trainingszwecken verwendet, und
besonders sensible Dokumente lassen sich als vertraulich markieren. Ideal ist das trotzdem
nicht — an manchen Stellen entscheide ich bewusst, was ich der KI überhaupt zeige.

**Spezialfeature — Steuerpaket.** Im Freigabe-Prozess wird jedes steuerrelevante Dokument einer
von 7 Steuer-Sphären zugeordnet (z. B. Arbeitnehmer, Maschinenverleih, Vermietung § 21 EStG,
außergewöhnliche Belastungen § 33 EStG, Sonderausgaben § 10 EStG). Pro Jahr entsteht eine
lebendige Steuerakte; am Ende lässt sich ein vollständiges Steuerpaket (ZIP + PDF-Begleitdokument)
für die Steuerberaterin herunterladen.

**Agentic Flow.** Hier kein Multi-Agenten-Pipeline-Aufbau, sondern eine asynchrone
Verarbeitungs-Pipeline: Next.js-Frontend + Python-FastAPI-Worker, der die Schritte
Hash → PDF→PNG → OCR → Claude-Analyse → Kategorie → DB-Write → Budget-Update durchläuft und
per Server-Sent-Events live ans UI meldet.

## Technisch

- **Architektur:** Monorepo mit `apps/web` (Next.js) und `apps/worker` (Python FastAPI); PostgreSQL in Docker; Makefile-gesteuerter Frisch-Start (`make dev`) auf festem Port 3000.
- **Frontend:** Next.js + TypeScript, next-intl (DE/EN), Recharts (Budget-/Statistik-Charts), react-pdf / @react-pdf/renderer, react-markdown, NextAuth (Login), Prisma-Client.
- **Worker:** FastAPI + Uvicorn, pytesseract (Tesseract `deu+eng`), pdf2image/Pillow, Anthropic SDK, SQLAlchemy, APScheduler (geplante Läufe), Watchdog (Ordner-Überwachung).
- **Datenpipeline:** SHA-256-Duplikaterkennung, asynchrone Verarbeitung, SSE-Push an die UI.
- **Qualität:** pytest-Suite im Worker (`test_ai`, `test_ocr`, `test_main`, `test_pipeline`); Architektur, Datenmodell und Entscheidungen in `docs/` dokumentiert.
- **Betrieb:** läuft lokal via Docker. Der KI-Analyse-Schritt ist der einzige externe Aufruf und ließe sich auf ein lokal betriebenes Modell umstellen.
- **Worker-Port/Setup-Details** (Makefile-Frisch-Start etc.) bewusst aus der Außendarstellung weggelassen.

## Impressionen

<!-- Screenshots in assets/screenshots/oozcod/ ablegen -->
- file: dashboard.png
  caption: Dashboard — Kategorien-Übersicht
- file: freigabe-detail.png
  caption: Freigabe-Prozess — Dokument prüfen und Metadaten zuweisen
- file: ocr-extraktion.png
  caption: Automatische OCR-Extraktion mit Ablage-Vorschlag
- file: browser.png
  caption: Dokument-Browser mit Filterung nach Kategorie/Jahr
- file: steuermappe.png
  caption: Steuerakte — Übersicht nach Steuerjahr
- file: steuerjahr-detail.png
  caption: Steuerjahr-Detail — Belege je Person oder Immobilie
- file: kontochecker-uebersicht.png
  caption: Konto-Checker — Jahres-Kontoauszüge und Transaktionen im Überblick (Zahlen unkenntlich gemacht)
- file: kontochecker-transaktionen.png
  caption: Konto-Checker — Transaktionsliste inklusive Kategorisierung (Zahlen unkenntlich gemacht)
- file: oozy.png
  caption: Oozy-Chatbot — Auskunft über die gesamte Dokumentenbasis

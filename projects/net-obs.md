---
slug: net-obs
name: NetObs
tagline: Lokales Netzwerk-Monitoring, das intermittierende ISP-Probleme misst, automatisch klassifiziert und report-fähig macht — gebaut, um dem Provider datenbasiert statt mit Bauchgefühl zu kommen.
type: tool
status: active
accent: "#2D6CDF"
timeframe_start: 2026-06-23
timeframe_end: 2026-06-26
planned: 2026-06-23
repo: net-obs
git_tracked: false
metrics:
  - label: Unit-Tests
    value: "122"
  - label: API-Integration (TR-064)
    value: "FRITZ!Box"
  - label: Agenten
    value: "6"
tech:
  - Python 3.11+ (uv)
  - typer (CLI)
  - FastAPI + HTMX (Dashboard)
  - Plotly.js (Charts)
  - SQLite (WAL-Modus)
  - fritzconnection (TR-064)
  - icmplib + dnspython + httpx (Probes)
  - WeasyPrint (PDF-Reports)
  - launchd (macOS-Autostart)
  - Pytest + Ruff (Qualität)
ais:
  - Claude Code — Programmierung über 6-Agenten-Team
last_scanned: 2026-07-04
source_commit: "n/a (kein Git)"
---

## Fachlich

**Idee.** Nach der Umstellung auf einen 1&1-Glasfaseranschluss traten immer wieder kurze,
undurchsichtige Netzwerkaussetzer auf — Webseiten hingen Sekunden, Teams-Calls ruckelten, und
der Support verwies auf „mögliche Probleme im Telekom-Netz". Statt auf Zuruf zu vertrauen, sollte
ein Tool dauerhaft mitschneiden, **wo genau** das Problem liegt: im eigenen WLAN, an der Leitung,
beim Peering oder bei der DNS-Auflösung.

**Was es tut.** NetObs läuft als Hintergrund-Daemon auf dem Mac, misst permanent vier parallele
Messpunkte (Gateway, erster ISP-Hop, mehrere öffentliche Ziele, mehrere DNS-Resolver), erkennt
Auffälligkeiten über ein Sliding Window und ordnet jedem Vorfall automatisch eine Ursache zu. Ein
Web-Dashboard zeigt alles live, und ein Report-Befehl erzeugt ein druckbares HTML/PDF-Dokument für
die Eskalation beim Provider.

**Kernfeatures.**
- Vier parallele Messpunkte (Gateway / ISP-Hop / Public-Targets / DNS) als Kern der Lokalisierung
- Automatische <b>Incident-Erkennung & Klassifikation</b> (LOCAL, ISP_LINE, ISP_BACKBONE, PEERING, DNS, IPV6_STACK, LOCAL_ROAMING …)
- Live-Web-Dashboard (FastAPI + HTMX + Plotly.js) mit Latenz-Charts, DNS-Vergleich und Incident-Liste
- FRITZ!Box-Integration (TR-064): WAN-Status, Durchsatz, Mesh-Events, Roaming — korreliert mit den Probes
- HTML-/PDF-Reports mit Executive Summary, gedacht für die Eskalation an den ISP
- macOS-natives Autostart (launchd) + Mail-Alarm bei Incidents

**Spezialfeature — vom Datensatz zur Lösung.** Über den Live-Downloadstatus im Dashboard fiel auf:
Es gab immer wieder Lastspitzen im Netz, die andere Geräte ausbremsten — lange unklar, woher und
warum. Des Rätsels Lösung war eine alte FRITZ!Box-Einstellung: Die PlayStation war für geschmeidiges
Remote Play unterwegs mit Echtzeit-Priorität eingestuft — sobald sie im Ruhemodus im Hintergrund
etwas herunterlud, wurde dadurch das ganze Netz spürbar unrund. Jetzt ist es umgedreht: die
MacBooks haben die Echtzeit-Priorität, seitdem laufen Meetings und Arbeitskontext ohne Probleme.

**Agentic Flow.** Ein Orchestrator-Agent koordiniert den Fortschritt anhand eines datei-basierten
Task-Trackers (`docs/progress/`) und delegiert an fünf spezialisierte Agenten — reine Konzeption
und Review, keine eigene Implementierung durch den Orchestrator selbst.

## Technisch

- **Architektur:** Drei unabhängige Komponenten auf einer gemeinsamen SQLite-Datenbank (WAL-Modus): Hintergrund-Daemon (Probes, Detector, Classifier), read-only Web-Dashboard (FastAPI/HTMX, nur `127.0.0.1`), und ein einmaliger Report-Befehl.
- **Stack:** Python 3.11+ verwaltet über uv, typer-CLI, FastAPI + HTMX + Plotly.js fürs Dashboard, SQLite für Speicherung, fritzconnection für die FRITZ!Box-TR-064-Schnittstelle, WeasyPrint für PDF-Export.
- **Qualität:** 122 Unit-Tests (Pytest), Ruff-Linting, pip-audit-Abhängigkeitsprüfung als Teil des Quality-Gates (`make qa`).
- **Deployment:** Läuft vollständig lokal auf macOS, kein Cloud-Dienst, keine Telemetrie. Optionaler Autostart über launchd. FRITZ!Box-Zugriff ausschließlich über einen eigens angelegten Read-only-User (nie den Admin-Account).

## Impressionen

<!-- Screenshots in assets/screenshots/net-obs/ -->
- file: dashboard-latenz.png
  caption: Live-Dashboard — Latenz nach Kategorie (Gateway / ISP-Hop / Public)
- file: dashboard-dns.png
  caption: DNS-Antwortzeiten im Vergleich + Incidents der letzten 24 Stunden
- file: incidents.png
  caption: Incident-Liste mit automatischer Klassifikation
- file: fritzbox.png
  caption: FRITZ!Box-Tab — WAN-Status, Durchsatz, Ereignisse
- file: report.png
  caption: PDF-Report mit Executive Summary für die ISP-Eskalation
- file: config.png
  caption: Konfiguration der Probe- und Erkennungs-Schwellwerte

---
slug: maschinenverleih-skubatz
name: Maschinenverleih Skubatz
tagline: Buchungs- und Verwaltungsplattform für mein Nebengewerbe, den Werkzeug- & Maschinenverleih Skubatz — Online-Kalender, digitale Übergabe/Rückgabe und ein KI-Rechercheassistent fürs Geräte-Anlegen
type: web-app
status: active
accent: "#F25C2A"
timeframe_start: 2026-06-01
timeframe_end: ongoing
planned: 2026-06-01
repo: maschinenverleih-skubatz
git_tracked: true
metrics:
  - label: Feature-Specs
    value: "101"
  - label: Unit-Tests
    value: "86"
  - label: E2E-Tests
    value: "83"
  - label: Agenten
    value: "9"
tech:
  - Next.js 15 (App Router) + React 19
  - TypeScript (strict)
  - Tailwind v4 + shadcn/ui
  - Supabase (Postgres/Auth/Storage/RLS, EU-Frankfurt)
  - Vercel (Frankfurt)
  - Resend (EU, transaktionale Mails)
  - Vitest + Playwright
  - pnpm Monorepo
last_scanned: 2026-06-16
source_commit: "HEAD @ 2026-06-16"
---

## Fachlich

**Idee.** Eine vollwertige, kommerzielle Website für einen inhabergeführten deutschen
Werkzeug- und Maschinenverleih (`maschinenverleih-skubatz.de`). Öffentlicher Katalog mit
Verfügbarkeit und Buchungsanfrage, ein Kundenbereich (Login, Buchungen, DSGVO-Self-Service)
und ein Admin-Bereich für den gesamten Geräte-Lebenszyklus — vom Anlegen über Übergabe und
Rückgabe bis zum Steuer-Export. Deutschsprachige Oberfläche, EU-Datenresidenz, testgetrieben.

**Was es tut.** Kunden durchsuchen den Gerätekatalog, prüfen die Verfügbarkeit im Kalender,
legen Geräte in den Warenkorb (Gruppen-Buchung) und stellen eine Buchungsanfrage. Der Inhaber
verwaltet im Admin-Bereich Geräte, bestätigt oder lehnt Buchungen ab, dokumentiert Übergabe
und Rückgabe (inkl. Foto- und Unterschrift-Erfassung) und kann die steuerrelevanten Daten exportieren.

**Kernfeatures.**
- Öffentlicher Gerätekatalog mit Verfügbarkeits-Kalender und Buchungs-Overlay
- Warenkorb-/Gruppenbuchung, Buchungs-Verlängerung, Verspätungs-Logik
- Kompletter Admin-Lebenszyklus: Anlegen, Freigabe, Übergabe/Rückgabe, Wartung
- **KI-Recherche-Assistent beim Geräte-Anlegen** (Spezialfeature, s.u.)
- Vertrags-PDF + digitale Unterschrift, transaktionale Lifecycle-Mails
- DSGVO-Self-Service (Datenauskunft/-export, Löschung), AGB-Akzeptanz
- SEO inkl. OG-Images, A11y-Sweeps, CSP-Hardening, Vercel Analytics

**Spezialfeature — KI-Recherche-Assistent (Spec 099).** Beim Anlegen eines neuen Geräts kann
der Inhaber mit einem eigenen, serverseitig hinterlegten Anthropic-API-Key eine Claude-(Opus)-
Recherche auslösen: aus Hersteller-URL + Produktname (optional Kaufbeleg) werden Name, Kategorie,
Hersteller, Beschreibungen, Such-Tags und ein Preisvorschlag (orientiert an marktüblichen
Mietpreisen) vorbefüllt. Alles ist ein **Vorschlag**, den der Inhaber prüft und bestätigt —
nichts wird automatisch gespeichert, die Seriennummer bleibt immer manuell, jeder Lauf ist eine
explizite, token-abgerechnete Aktion. Der DSGVO-/Kosten-Trade-off (Daten verlassen die EU-Default-
Residenz) ist dokumentiert und freigegeben.

**Agentic Flow.** Features durchlaufen eine deterministische **9-Schritt-Pipeline**:
Requirements → Spec-Review → TDD-Red → Implement → Code-Review → DSGVO/Legal → SEO →
Final-QA → Commit. Ein Orchestrator-Agent kontrolliert die Übergänge, Hooks routen den
nächsten Agenten über `.claude/state/`. Die neun Agenten: orchestrator, requirements-engineer,
researcher, senior-fullstack-dev, code-reviewer, dsgvo-agent, legal-reviewer, seo-agent,
qa-engineer.

## Technisch

- **Architektur:** pnpm-Monorepo — `apps/web` (Next.js), `packages/design-system` (Brand-Tokens, Components), `packages/shared` (Zod-Schemas), `packages/db` (Supabase-Migrationen + generierte Typen).
- **Spec-Driven:** 101 frontmatter-gesteuerte Feature-Specs in `docs/specs/` mit Akzeptanzkriterien, Abhängigkeiten, PII-Flags und e2e-Journeys; Architecture Decision Records in `docs/adr/`.
- **Stack:** Next.js 15 / React 19 / TypeScript strict, Tailwind v4 + shadcn/ui, Supabase (Postgres, Auth/OTP, Storage, Row-Level-Security; EU Frankfurt), Vercel (Frankfurt), Resend (EU).
- **Qualität:** 67 Unit-/Integrationstest-Dateien (Vitest) + 57 E2E-Test-Dateien (Playwright), inkl. A11y-Tests (`@a11y`), Lighthouse-CI, ESLint mit 0 Warnungen, Prettier, Husky-Pre-Commit-Hooks, Gitleaks-Secret-Scan.
- **Deployment:** Vercel (Frankfurt) + Supabase + Resend (EU) — der Aufbau dieser kompletten Deployment-Pipeline war selbst ein großer Lerneffekt; fester Dev-Port 3100.
- **Datenschutz/Recht:** dedizierte DSGVO- und Legal-Reviewer-Agenten in der Pipeline; PII-Tracking auf Spec-Ebene; EU-Datenresidenz als Default.
- **Status:** seit Juni 2026 in Beta-Tests und laufender Geräteeinpflege, noch nicht vollständig live.

## Impressionen

<!-- Screenshots in assets/screenshots/maschinenverleih-skubatz/ ablegen -->
- file: startseite.png
  caption: Startseite — Werkzeuge & Maschinen fürs Wochenendprojekt
- file: geraet-detail.png
  caption: Gerätedetail mit Verfügbarkeit
- file: verfuegbarkeit.png
  caption: Verfügbarkeits-Kalender — Zeitraum wählen
- file: vier-schritte.png
  caption: In vier Schritten zum Gerät
- file: mein-konto.png
  caption: Kundenbereich — eigene Buchungen im Überblick
- file: admin-dashboard.png
  caption: Admin-Dashboard — Anfragen, Auslastung, Buchungen
- file: admin-buchungen.png
  caption: Admin — Buchungsübersicht
- file: kalender-schliessfaecher.png
  caption: Kalender & Schließzeiten verwalten
- file: admin-geraet-anlegen.png
  caption: Neues Gerät anlegen mit KI-Recherche-Assistent
- file: einstellungen.png
  caption: Einstellungen

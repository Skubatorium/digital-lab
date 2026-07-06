# Screenshots — was da ist und wie abgelegt wird

Screenshots liegen unter `assets/screenshots/<slug>/` mit sprechenden Dateinamen (PNG, auf max.
1600px Breite herunterskaliert, Seitenverhältnis erhalten — kein Verzerren, kein Upscaling). Das
**erste** Bild der `impressionen`-Liste einer Projektdatei wird als Kachel-Teaser auf der
Startseite und als erstes Bild im Detail-Hero-Slider verwendet.

Rohe, unbearbeitete Screenshots (macOS-Bildschirmfotos, Original-Dateinamen) liegen weiterhin in
`assets/screenshots/<slug>/_raw/` — falls mal ein anderer Ausschnitt gebraucht wird.

Wenn du neue/zusätzliche Rohbilder ablegst: einfach in `assets/screenshots/<slug>/` (oder direkt
in `_raw/`) legen und Bescheid sagen — ich sichte, kuratiere und passe die `Impressionen`-Liste
in der Projektdatei an.

## mailo → `assets/screenshots/mailo/`
- `tagesroutine.png` — Tägliches Morgen-Briefing mit Einschätzung je Mail
- `label-struktur.png` — Farbcodierte Label-Struktur im Posteingang
- `label-baum.png` — Vollständige Label-Struktur (53 Labels)
- `meilensteine.png` — Meilensteine: vom Postfach-Chaos zu Inbox Zero
- `tageszusammenfassungen.png` — Verlauf der Tageszusammenfassungen
- `automatisierte-aufgabe.png` — Automatisierte Tagesroutine (Ausführungs-Log)
- `ausfuehrungen.png` — Tägliche Ausführung der geplanten Routine

## maschinenverleih-skubatz → `assets/screenshots/maschinenverleih-skubatz/`
- `startseite.png` — Startseite
- `geraet-detail.png` — Gerätedetail mit Verfügbarkeit
- `verfuegbarkeit.png` — Verfügbarkeits-Kalender
- `vier-schritte.png` — In vier Schritten zum Gerät
- `mein-konto.png` — Kundenbereich: eigene Buchungen
- `admin-dashboard.png` — Admin-Dashboard
- `admin-buchungen.png` — Admin: Buchungsübersicht
- `kalender-schliessfaecher.png` — Kalender & Schließfächer
- `admin-geraet-anlegen.png` — Neues Gerät anlegen mit KI-Recherche-Assistent
- `einstellungen.png` — Einstellungen: Verleihpreise

## oozcod → `assets/screenshots/oozcod/`
- `dashboard.png` — Dashboard: Kategorien-Übersicht
- `freigabe-warteschlange.png` — Freigabe-Warteschlange
- `freigabe-detail.png` — Freigabe-Prozess im Detail
- `ocr-extraktion.png` — Automatische OCR-Extraktion
- `browser.png` — Dokument-Browser (Baum nach Kategorie/Jahr)
- `suche.png` — Volltextsuche
- `steuermappe.png` — Steuerakte: Übersicht nach Steuerjahr
- `steuerjahr-detail.png` — Steuerjahr-Detail
- `kontochecker-uebersicht.png` — Konto-Checker Übersicht (Euro-Beträge & IBAN geblurrt)
- `kontochecker-monat.png` — Konto-Checker Monatsansicht (Beträge geblurrt)
- `kontochecker-transaktionen.png` — Konto-Checker Transaktionsliste (Beträge geblurrt)
- `kontochecker-tooltip.png` — Konto-Checker Jahresverlauf (Beträge geblurrt)
- `oozy.png` — Oozy-Chatbot

## drachenspur → `assets/screenshots/drachenspur/`
- `start.png` — Startbildschirm mit Hauptmenü
- `character-select.png` — Charakterauswahl
- `heldenportraet.png` — Heldenporträt
- `game.png` — Dungeon-Erkundung
- `dungeon-plaettchen.png` — Dungeon-Plättchen aufdecken
- `combat-3d-dice.png` — Würfelkampf
- `sieg.png` — Sieg-Bildschirm
- `compendium-monster.png` — Kompendium: Monster
- `compendium-held.png` — Kompendium: Heldenprofil
- `statistics.png` — Statistiken pro Held und Monster
- `admin-grafik.png` — Asset-Manager: Grafiken
- `admin-audio.png` — Asset-Manager: Audio
- `abspann.png` — Abspann

## net-obs → `assets/screenshots/net-obs/`
- `dashboard-latenz.png` — Live-Dashboard: Latenz nach Kategorie
- `dashboard-dns.png` — DNS-Antwortzeiten + Incidents
- `incidents.png` — Incident-Liste mit Klassifikation
- `fritzbox.png` — FRITZ!Box-Tab
- `report.png` — PDF-Report
- `config.png` — Konfiguration

## Sensible Daten / Blur-Regel

Zahlen wie Dokumenten- oder Kategorie-Anzahlen sind unkritisch und bleiben sichtbar. Echte
Euro-Beträge aus persönlichen Finanz-/Kontodaten (aktuell: OOZCODs Konto-Checker) werden vor der
Veröffentlichung mit einem echten Pixelate+Gaussian-Blur unkenntlich gemacht (kein reiner
CSS-Filter — die Information ist im Bild selbst zerstört, nicht nur optisch überdeckt).

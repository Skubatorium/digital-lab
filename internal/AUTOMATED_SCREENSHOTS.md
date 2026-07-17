# Automatisierte Screenshots — Playbook für beliebige Projekte

Unterschied zu `SCREENSHOTS.md`: Dort geht es um Screenshots, die **Christian selbst** beisteuert
(macOS-Bildschirmfotos, manuell abgelegt). Dieses Dokument ist für den Fall, dass **ich** (Claude,
in Cowork) Screenshots eines laufenden Projekts selbst erzeuge — z. B. weil Christian keine Lust
hat, manuell durchzuklicken, oder weil sich ein Projekt geändert hat und die bestehenden Bilder
veraltet sind. Gilt für **jedes** Projekt mit einer lokal lauffähigen Web-App, nicht nur für ein
bestimmtes — MindLoop war 2026-07-05/07-18 nur der erste Anwendungsfall.

Digital Lab ist dabei immer der Ausgangspunkt: Christian sagt "geh zu `<projekt>`, mach neue
Screenshots", ich lese hier nach, hole mir dann Zugriff auf das Ziel-Repo und lege die fertigen
Bilder wieder in `assets/screenshots/<slug>/` in **diesem** Repo ab.

## Warum ein Sandbox-Klon, nie das echte Repo

Verbundene Projekt-Ordner (`request_cowork_directory`) sind in Cowork ein Bind-Mount vom echten
Rechner. Ihr `node_modules` wurde auf macOS installiert — native Module (better-sqlite3, argon2,
sharp, @next/swc, …) laufen im Linux-Sandbox nicht. Nie den Dev-Server oder Playwright direkt
gegen den echten Mount laufen lassen. Stattdessen:

```
git clone <bash-pfad-des-mounts> /tmp/<projekt>/repo
```

und dort komplett neu `npm install` (o. ä.) — das schützt außerdem jede echte lokale DB vor
versehentlichem Anfassen. Nur fertige Screenshots wandern zurück in **dieses** Digital-Lab-Repo.

## Wiederkehrende Sandbox-Stolpersteine (nicht projektspezifisch)

Diese Probleme sind Eigenschaften der Cowork-Sandbox selbst, nicht des jeweiligen Projekts —
gelten also wortwörtlich für das nächste Projekt genauso:

- **`mcp__workspace__bash` killt nach 45 s hart**, und jeder Aufruf läuft in einem eigenen
  Pid-Namespace — Hintergrundprozesse (`nohup … & disown`) überleben das Ende eines Aufrufs
  **nicht**. Ein Dev-Server + das komplette Playwright-Skript müssen daher in einem einzigen
  Bash-Aufruf laufen. Das Dateisystem (inkl. `.next/cache`, `node_modules`, npm-Cache) übersteht
  Aufrufe dagegen problemlos — lange Installationen/Downloads einfach über mehrere Aufrufe
  wiederholen, sie setzen dort fort, wo sie unterbrochen wurden (Ausnahme: siehe
  Chromium-Download unten). Trick für zügige erste echte Screenshot-Runde: ein günstiger
  "Aufwärm"-Aufruf zuerst (Server starten, einloggen, jede Ziel-Route einmal per `curl` mit
  Session-Cookie aufrufen, damit Next.js sie kompiliert/cached), danach ein finaler Aufruf mit
  neu gestartetem (jetzt schnellem) Server + vollständigem Playwright-Durchlauf.
- **`npx playwright install chromium` kann keinen abgebrochenen Download fortsetzen** und fängt
  bei jedem Versuch wieder bei 0 % an — bei etwa 187 MB (arm64) schafft es das nie vor dem
  45-Sekunden-Limit. Fix: die Zip-Datei selbst per `curl -L -C - -o chromium.zip <cdn-url>` laden
  (das unterstützt echtes Resume über mehrere Aufrufe), manuell entpacken, und beim
  `chromium.launch()` explizit `executablePath` auf die entpackte Binary setzen — Playwrights
  eigene Browser-Verwaltung dabei komplett umgehen.
- **Chromium startet danach oft nicht** wegen fehlender Shared Library, typischerweise
  `libXdamage.so.1` (kein root/apt in der Sandbox). Fix: Vier-Funktionen-No-op-C-Stub
  (`XDamageQueryExtension/Create/Destroy/Subtract`), `gcc -shared -fPIC -o libXdamage.so.1
  stub.c`, beim Start `LD_LIBRARY_PATH=<stub-ordner>` setzen. Mit `ldd <chrome-binary> | grep
  "not found"` prüfen, ob noch weitere Libraries fehlen — bisher war es immer nur diese eine.
- **Native Module ohne Prebuild für linux-arm64** (z. B. better-sqlite3) brauchen einen lokalen
  Kompilierlauf. `node-gyp` ist oft nicht vorhanden (`npm install --no-save node-gyp`). Große
  C-Amalgamationen (SQLite-Quelltext o. ä.) bei `-O2`/Release sprengen fast immer die
  45-Sekunden-Grenze — `node-gyp rebuild --debug --nodedir=/usr` (unoptimiert) kompiliert
  schnell genug und lädt genauso.
- **Manche Downloads landen abgeschnitten im npm-Cache** (z. B. `@next/swc-linux-*`), wenn ein
  vorheriger `npm install` mitten im Download durch das 45-Sekunden-Limit gekillt wurde — das
  führt zu kryptischen Abstürzen (Bus-Error) statt einer klaren Fehlermeldung. Prüfen mit
  `file <datei>.node` (verdächtig: "missing section headers" bei einer Größe, die viel kleiner
  wirkt als bei anderen Paketen dieser Größenordnung) — Fix: `npm cache clean --force`, dann
  das Paket gezielt neu installieren.
- **Manche Domains sind über das Netzwerk-Allowlist blockiert** (z. B. `binaries.prisma.sh` für
  Prisma-Engine-Downloads, `fonts.googleapis.com` für Google Fonts, `github.com`-Releases für
  manche `prebuild-install`-Downloads). Erkennbar am Response-Header `X-Proxy-Error:
  blocked-by-allowlist` bzw. HTTP 403. Kein Workaround über andere Tools versuchen (nicht
  umgehen) — stattdessen einen Ersatzweg finden, der ohne den blockierten Download auskommt
  (Beispiel Prisma: die bereits generierte Client-Datei aus dem echten Repo kopieren statt neu
  zu generieren) oder mit Fallback leben (Beispiel Google Fonts: Screenshots zeigen dann eine
  System-Fallback-Schrift statt der echten Design-System-Schrift — Layout/Inhalt bleiben korrekt,
  aber das dazusagen).
- **`next dev --turbopack` kann mit Bus-Error abstürzen** in dieser Sandbox — falls das passiert,
  einfach ohne `--turbopack`-Flag starten.

## Genereller Ablauf

1. Ziel-Repo per `request_cowork_directory` verbinden (falls noch nicht geschehen), Struktur
   grob erkunden (README, `package.json`-Scripts, Prisma-Schema o. ä., je nach Stack).
2. Sandbox-Klon anlegen (`git clone <mount-pfad> /tmp/<slug>/repo`), dort `npm install`
   (oder Äquivalent) plus die oben gelisteten Fixes nach Bedarf.
3. Testdaten/Login-Weg klären: bevorzugt ein dedizierter, klar benannter Test-/Demo-Account
   (z. B. `showcase`/Rolle Admin bei MindLoop), der bestehende Nutzer/Daten nicht anfasst.
   Login im Playwright-Skript nach Möglichkeit direkt per API-Request im Browser-Context setzen
   (schneller als das Formular auszufüllen), UI-Interaktionen nur dort, wo eine API einen
   CSRF-Header o. ä. erwartet, den nachzubauen mehr Aufwand wäre als ein echter Klick.
4. Konvention für die Aufnahmen — sofern Christian nichts anderes sagt, gilt der bei MindLoop
   etablierte Standard: fester Viewport **1440×900**, **kein** `fullPage`-Scroll-Stitching
   (Playwright `fullPage:false`), Bildausschnitt beginnt oben links (kein mittiger Crop).
5. Screenshots nach `assets/screenshots/<slug>/` in **diesem** Repo kopieren (Dateinamen
   sprechend, wie in `SCREENSHOTS.md` üblich).
6. `projects/<slug>.md` (Impressionen-Liste + ggf. Kernfeatures/Tagline bei neuen Features) und
   `showcase/index.html` (gleiche Felder im `PROJECTS`-Array) aktualisieren — **nur** das
   angefragte Projekt anfassen, nichts an den anderen ändern, auch wenn deren Doku ähnlich
   veraltet aussieht.
7. Validieren: `node --check` auf das extrahierte Inline-`<script>` in `showcase/index.html`,
   und dass jede referenzierte Bilddatei tatsächlich existiert (kein 404-Risiko).

## Git-Besonderheiten dieses (Digital-Lab-)Repos

- Der verbundene Ordner erlaubt standardmäßig kein Löschen/Unlink irgendeiner Datei (auch nicht
  frisch erzeugter, auch nicht Gits eigenem `index.lock`) — `git commit` bricht sonst mit
  "unable to unlink"/"unable to create index.lock" ab. Einmal `allow_cowork_file_delete` für
  einen Pfad in diesem Ordner aufrufen, danach `rm -f .git/index.lock` und erneut versuchen.
- Keine SSH-Keys in der Sandbox — `git push` schlägt immer mit "Permission denied (publickey)"
  fehl. Lokal committen, Christian bittet danach selbst `git push` auf seinem Rechner aus (ist
  derselbe Git-Ordner, da Bind-Mount).
- `git config user.name`/`user.email` ist in der Sandbox nicht gesetzt — vor dem ersten Commit
  einer Sitzung lokal setzen (Werte aus `git log` der bestehenden Commits übernehmen).
- Deploy läuft über GitHub Actions (`.github/workflows/pages.yml`) nach **lab.osgame.de**,
  automatisch bei jedem Push auf `main`, kein manueller Schritt nötig (~1-2 Min.).

## Bereits dokumentierte Projekt-Durchläufe

- **MindLoop** (2026-07-05, 2026-07-18): siehe `internal/MEMORY.md`, Abschnitte "MindLoop erfasst
  + Screenshots automatisiert erzeugt" und "MindLoop-Update ... Screenshots erneuert" — dort auch
  MindLoop-spezifische Dinge wie Test-User-Anlage per `create-admin.ts`, Prisma-Migrations-Workaround
  und die konkreten Playwright-Selektoren.

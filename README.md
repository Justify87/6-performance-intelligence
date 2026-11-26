# GTA VI Performance Intelligence

**Zweck:** Evidenzbasierter Katalog bereits in den jeweiligen Standardbranch integrierter Verbesserungen an GTA-VI-Portierungen und Xbox-Series-Kompatibilitätsschichten. Die Ergebnisse sollen eine **spätere, separate Integration** guter Optimierungen ermöglichen. Dieses Repository speichert nur Messungen, Beurteilungen und dauerhafte Referenzen auf Fremdcode – **keinen Fremdcode, keine Spieldateien und keine Installer**.

## Betrieb und Grenzen

| Eigenschaft | Verbindlicher Stand |
| --- | --- |
| Analyse | ChatGPT liest neue GitHub-Commits, prüft Diffs und Messnachweise und urteilt selbst |
| Takt | Geplant: eine separate ChatGPT-Aufgabe stündlich; **derzeit nicht aktiviert** |
| Quell-Branch | Je Repository **ausschließlich dessen aktuell gemeldeter `default_branch`** |
| Scan-Grenze | Nur `(last_scanned_sha, captured_default_branch_head]`; SHA, **nicht Uhrzeit** |
| Funde | Nur messbare und hinreichend vergleichbare **Vorher-/Nachher-Verbesserungen** |
| Persistenz | Bewertungen, Übersicht und Scan-Cursor **atomar in einem Git-Commit** |
| Laufzeitsoftware | **Keine** Python-Skripte, GitHub Actions, Server, Compiler oder Benchmark-Automation |
| Geplante Schreibfähigkeit | **Noch durch echten unbeaufsichtigten Stundenlauf zu verifizieren** |

## Dateiverträge

| Datei | Verantwortlichkeit | Schreibregel |
| --- | --- | --- |
| [`AGENTS.md`](AGENTS.md) | Ausführbares Harness: Gates, Evidenz, Recovery, Freigaben | Nur bewusste Harness-Änderungen |
| [`WATCHLIST.md`](WATCHLIST.md) | Quell-Repositories, Priorität und Monitoring-Status | Nur begründete Aufnahme/Statusänderung |
| [`BASELINE.md`](BASELINE.md) | Feste technische Ausgangsbasis für spätere Integration | Nur explizite, evidenzbasierte Neuauswahl |
| [`FINDINGS.md`](FINDINGS.md) | **Kanonisches Register** bestätigter Verbesserungen | Nur vollständige, quellenbelegte Befunde |
| [`PERFORMANCE.md`](PERFORMANCE.md) | **Abgeleitete**, vergleichsorientierte Ranglisten | Nur auf Grundlage der Finding-IDs |
| [`state/scan-state.json`](state/scan-state.json) | Persistente SHA-Cursor, Default-Branch-Beobachtung, Fehlerstatus | Mit neuen Findings atomar publizieren |
| `README.md` | Ziel, Architektur, Bedien- und Betriebsstatus | Keine konkurrierenden Detailregeln |

## Technische Integrationsbasis

[BASELINE.md](BASELINE.md) verankert einen konkreten Quellcode-Stand unabhängig von den Scan-Cursorn. Neue belegte Verbesserungen werden gegen diesen Stand auf Vorhandensein und Übertragbarkeit geprüft; eine Umsetzung erfolgt nicht im Monitor.

## Start-Anker und Qualitätsgrenzen

Die existierenden `last_scanned_sha` wurden bei der Einrichtung vom damaligen Standardbranch erfasst. **Sie sind Start-Anker, keine Behauptung über rückwirkend geprüfte Commits.** Regulär geprüft wird nur, was **danach** im Standardbranch hinzukommt. Nachgereichte Benchmarks in neuen Commits dürfen für eine gezielte Verifikation frühere Code-Commits referenzieren; dies ist kein allgemeiner historischer Rescan.

Ein Vergleich ist nur aussagekräftig, wenn Messbedingungen hinreichend vergleichbar sind. Menu-FPS ≠ Spielwelt-FPS, Frame-Generation ≠ native FPS, reservierter ≠ tatsächlich genutzter Speicher. Entwickler-Messungen und unabhängige Reproduktionen bleiben unterscheidbar.

**Nächster Betriebs-Meilenstein:** manueller Scan-/Speichertest; danach explizit geplanter stündlicher KI-Lauf inklusive gesondertem Nachweis des unbeaufsichtigten GitHub-Schreibens.

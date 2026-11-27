# GTA VI Performance Intelligence

**Zweck:** Evidenzbasierter Katalog technisch und lizenzrechtlich anschlussfähiger, bereits in den jeweiligen Standardbranch integrierter Verbesserungen an GTA-VI-Portierungen und Xbox-Series-Kompatibilitätsschichten. Die Ergebnisse sollen eine **spätere, separate Integration** guter Optimierungen ermöglichen. Dieses Repository speichert nur Messungen, Beurteilungen und dauerhafte Referenzen auf Fremdcode – **keinen Fremdcode, keine Spieldateien und keine Installer**.

## Betrieb und Grenzen

| Eigenschaft | Verbindlicher Stand |
| --- | --- |
| Analyse | ChatGPT liest neue GitHub-Commits, prüft Diffs und Messnachweise und urteilt selbst |
| Takt | Geplant: eine separate ChatGPT-Aufgabe stündlich; **derzeit nicht aktiviert** |
| Quell-Branch | Je Repository **ausschließlich dessen aktuell gemeldeter `default_branch`** |
| Scan-Grenze | Nur `(last_scanned_sha, captured_default_branch_head]`; SHA, **nicht Uhrzeit** |
| Quellenauswahl | Regulär nur Quellen mit belegtem, integrationsfähigem Subsystemumfang; pausierte Quellen behalten ihre Cursor |
| Funde | Nur zurechenbare, hinreichend vergleichbare **Vorher-/Nachher-Verbesserungen** mit noch nicht vollständig vorhandenem Mechanismus und belegter Integrationsrelevanz |
| Persistenz | Bewertungen, Übersicht und Scan-Cursor **atomar in einem Git-Commit** |
| Laufzeitsoftware | **Keine** Python-Skripte, GitHub Actions, Server, Compiler oder Benchmark-Automation |
| Geplante Schreibfähigkeit | **Noch durch echten unbeaufsichtigten Stundenlauf zu verifizieren** |

## Dateiverträge

| Datei | Verantwortlichkeit | Schreibregel |
| --- | --- | --- |
| [`AGENTS.md`](AGENTS.md) | Ausführbares Harness: Gates, Evidenz, Recovery, Freigaben | Nur bewusste Harness-Änderungen |
| [`WATCHLIST.md`](WATCHLIST.md) | Quell-Repositories, Priorität, Monitoring-Status und erlaubte Subsysteme | Begründete Aufnahme/Evidenzaktualisierung oder explizite Scope-Wartung |
| [`BASELINE.md`](BASELINE.md) | Feste technische Ausgangsbasis und Kompatibilitätsgrenzen | Nur explizite Vertragsrevision oder evidenzbasierte Neuauswahl |
| [`FINDINGS.md`](FINDINGS.md) | **Kanonisches Register** gemessener Verbesserungen und ihrer Integrationsrelevanz | Nur vollständige, qualifizierte neue Befunde; historische Evidenz erhalten |
| [`PERFORMANCE.md`](PERFORMANCE.md) | **Abgeleitete**, vergleichsorientierte Ranglisten | Nur auf Grundlage der Finding-IDs |
| [`state/scan-state.json`](state/scan-state.json) | Persistente SHA-Cursor, Default-Branch-Beobachtung, Fehlerstatus | Mit neuen Findings atomar publizieren |
| `README.md` | Ziel, Architektur, Bedien- und Betriebsstatus | Keine konkurrierenden Detailregeln |

## Technische Integrationsbasis

[BASELINE.md](BASELINE.md) verankert einen konkreten Quellcode-Stand unabhängig von den Scan-Cursorn. Die [WATCHLIST](WATCHLIST.md) begrenzt die reguläre Überwachung auf belegte kompatible Subsysteme; die vollständige Commit-Erfassung aktiver Quellen bleibt bestehen. Neue belegte Verbesserungen werden zusätzlich gegen den Anker auf Vorhandensein, Übertragbarkeit, Herkunft und Lizenz geprüft. Historische Messungen bleiben erhalten, die [Performance-Übersicht](PERFORMANCE.md) trennt aktive Integrationskandidaten von bedingten bzw. reinen Referenzbefunden. Eine Umsetzung erfolgt nicht im Monitor.

## Start-Anker und Qualitätsgrenzen

Die existierenden `last_scanned_sha` wurden bei der Einrichtung vom damaligen Standardbranch erfasst. **Sie sind Start-Anker, keine Behauptung über rückwirkend geprüfte Commits.** Regulär geprüft wird nur, was **danach** im Standardbranch hinzukommt. Nachgereichte Benchmarks in neuen Commits dürfen für eine gezielte Verifikation frühere Code-Commits referenzieren; dies ist kein allgemeiner historischer Rescan.

Ein Vergleich ist nur aussagekräftig, wenn Messbedingungen hinreichend vergleichbar sind. Menu-FPS ≠ Spielwelt-FPS, Frame-Generation ≠ native FPS, reservierter ≠ tatsächlich genutzter Speicher. Entwickler-Messungen und unabhängige Reproduktionen bleiben unterscheidbar.

**Nächster Betriebs-Meilenstein:** manueller Scan-/Speichertest; danach explizit geplanter stündlicher KI-Lauf inklusive gesondertem Nachweis des unbeaufsichtigten GitHub-Schreibens.

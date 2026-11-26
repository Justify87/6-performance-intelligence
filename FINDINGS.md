# FINDINGS.md — Kanonisches Performance-Register

**Einzige maßgebliche Datenquelle** für gemessene Gewinne. Nur Änderungen, die erstmals in einem **seit dem jeweiligen Cursor neuen Standardbranch-Commitbereich** bewertet werden und den Aufnahmevertrag in [`AGENTS.md`](AGENTS.md) erfüllen, dürfen eingetragen werden. Neue Evidenz in einem neuen Commit darf einen älteren Code-Commit als Ursache referenzieren.

## Registerschema

| Feld | Zulässige Semantik |
| --- | --- |
| ID | Eindeutiges, monotones `PERF-0001` usw.; niemals wiederverwenden |
| Verbesserung | Technische Ursache, nicht Marketingname |
| Metric | `WORLD_FPS`, `FRAME_TIME_MS`, `RAM_GIB`, `VRAM_GIB`, `STABILITY`, `LOAD_TIME_S` (präzise metrische Definition im Detail) |
| Before → After | Vergleichbare **gemessene** Werte mit Einheit |
| Delta | Absoluter und optional korrekter relativer Gewinn |
| Evidence | `AUTHOR_MEASURED` oder `INDEPENDENT_REPRODUCED` |
| Source | Link zur stabilen Finding-ID unterhalb und zu ursprünglichem Quell-Commit |

## Registerübersicht

| ID | Verbesserung | Metric | Before → After | Delta | Evidence |
| --- | --- | --- | --- | --- | --- |
| _Keine_ | Bisher kein qualifizierter Befund seit den Bootstrap-Cursorn | — | — | — | — |

## Detaildatensätze

Für **jeden** Fund nach `### PERF-XXXX — Kurzname` exakt diese Feldreihenfolge verwenden:

| Feld | Pflichtinhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` oder `VERIFIED_INDEPENDENT` (gemessen, aber Herkunft beachten) |
| Origin repository | Kanonisches `owner/name` |
| Code commit(s) | Vollständige GitHub-Commit-URLs; mehrere falls nötig |
| Evidence commit(s) | Vollständige Commit-URLs der gemessenen Vorher-/Nachher-Angaben; ggf. identisch |
| Merged PR | URL falls tatsächlich im Default-Branch enthalten, sonst `n/a` |
| Subsystem / Mechanism | Technische Ursache, relevante Änderung |
| Metric; Before; After; Delta | Exakte Messgröße mit Einheit; Werte und Vergleichsrechnung |
| Test conditions | Hardware/GPU/CPU, Szene, Auflösung, Spielprofil, Laufdauer und Grafikqualität, soweit dokumentiert; fehlende Werte `unknown` |
| Evidence | `AUTHOR_MEASURED` oder `INDEPENDENT_REPRODUCED` und spezifischer Nachweis |
| Trade-offs | Bekannte Regressionen, Qualitäts- und Stabilitätsfolgen oder `not documented` |
| Integration | Abhängigkeiten, Überschneidungen, potenzielle Kompatibilitätsprobleme |
| Provenance / License | Herkunftsquelle, Lizenz nachweisbar oder `not verified` |
| Adoptions | Spätere übernommene Commits anderer Forks oder `none recorded` |

Ein Datensatz wird **nur** mit allen Pflichtfeldern angelegt. Lange Erklärungen dürfen zusätzlich unter `#### Technical notes` erscheinen; Standardfelder nicht umbenennen. Bei Übernahme oder erneuter Messung **bestehenden Datensatz ergänzen**, nicht zweite ID vergeben. Eine neue Messung ist kein Beweis für identische Hardware-/Szenenbedingungen.

## Nicht zulässige Funde

Keine Menü-FPS statt Welt-FPS, keine inhaltslosen „FPS boost“-Behauptungen, keine theoretischen Speicherallokationen statt gemessener Belegung, keine reine Telemetrie, keine ungemergten PR-Branches, keine Bildqualitätsreduktion ohne vergleichbare Bedingungen, keine unverifizierten Vorher-/Nachher-Zahlen. Entwickler-Messungen sind **belegt als Entwicklerangabe**, nicht unabhängig bestätigt.

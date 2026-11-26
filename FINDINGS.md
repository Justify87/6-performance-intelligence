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
| [PERF-0001](#perf-0001--descriptor-hot-page-fallback) | Fallback für übersehene Deskriptor-Schreibzugriffe | `FRAME_TIME_MS` | 82 ms → 72 ms | −10 ms (−12,2 %) | `AUTHOR_MEASURED` |

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

### PERF-0001 — Descriptor hot-page fallback

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` – nur im Quellprojekt berichtete Messung |
| Origin repository | [iLucasRT/VInAMD](https://github.com/iLucasRT/VInAMD) |
| Code commit(s) | [69a99e3 – Hot-page-Fallback](https://github.com/iLucasRT/VInAMD/commit/69a99e313a764370fda3bb850ef8df1c1018fb72); [5101a92 – Aktivierung in der Konfiguration](https://github.com/iLucasRT/VInAMD/commit/5101a92f1fb5fce0e2973299c37044ddbb0e86ab) |
| Evidence commit(s) | [5101a92 – README mit gemessenem Ein-/Aus-Vergleich](https://github.com/iLucasRT/VInAMD/commit/5101a92f1fb5fce0e2973299c37044ddbb0e86ab); [frühere README eb9d08a als Versionsbaseline, kein isolierter A/B-Test](https://github.com/iLucasRT/VInAMD/commit/eb9d08a05be908c1ee12a4716038c78be154fe22) |
| Merged PR | `n/a` – ein zugehöriger gemergter PR wurde hier nicht unabhängig verifiziert |
| Subsystem / Mechanism | GPU-Deskriptorverwaltung: von gepatchten Stores übersehene View-Records führen zu zeitweiligem erneutem Ganzseiten-Scan (`VICTORY_PORT_DESCRIPTORS_HOT`, Standard 240 Frames). Der neue Launcher aktiviert Renderer-Port 2 mit dieser Korrektur; zugleich Renderer-Port 3 und gepufferte Dateizugriffe. |
| Metric; Before; After; Delta | **Mediane Frametime der Spielwelt:** Renderer-Port 2 **aus: 82 ms** → Port 2 **ein: 72 ms**; **−10 ms (−12,2 %)**, vom Entwickler im neuen README als Gegenprobe berichtet. Begleitend: Anteil Sekunden mit einem Frame über 250 ms **23 % (aus) → 1 % (ein)**; dies ist keine unabhängige zweite Optimierung. |
| Test conditions | Vom Projekt dokumentiert: Windows 11; Ryzen 7 5800X3D; Radeon RX 7900 XTX 24 GB; 64 GB System-RAM; Diagnostics-Build; vorübersetzte Shader/gewärmte Pipelines; Open World/Stadt. Exakte Auflösung, Szenenfolge und A/B-Messdauer: `unknown`. |
| Evidence | `AUTHOR_MEASURED` – A/B-Zahlen in versionierter README; von uns keine Benchmarks ausgeführt, keine unabhängig reproduzierten Rohlogs. Der frühere Versionsvergleich 80 ms → 72 ms betrifft mehrere gleichzeitige Änderungen und belegt **nicht** allein den Port-2-Effekt. |
| Trade-offs | Bereits bekannte Probleme: neue Texturen können zeitweise schwarz oder niedrig aufgelöst sein; vollständige Bildgleichheit des A/B-Vergleichs nicht unabhängig geprüft. Der Hot-page-Fallback soll zuvor fehlende indirekte Beleuchtung wiederherstellen. |
| Integration | Abhängigkeit von Renderer-Port 2 und dessen Deskriptor-Tracking/Fallback; nicht als unabhängiger Drop-in-Patch getestet. Spätere Integration erfordert Abgleich der Deskriptorverwaltung, Port-3-Einstellungen sowie A/B-Benchmarks auf Zielhardware. |
| Provenance / License | Attribution zu den verlinkten VInAMD-Commits; mögliche Vorarbeiten anderer Forks vor Übernahme prüfen. Lizenzkompatibilität: `not verified`. |
| Adoptions | `none recorded` |

#### Technical notes

Der **isolierbar dokumentierte** Vergleich betrifft das Ein-/Ausschalten von Renderer-Port 2 innerhalb des aktualisierten VInAMD-Stands (82 ms gegenüber 72 ms). Die separate Änderung des gesamten Projektstands von 80 auf 72 ms und die gesunkene Stutter-Rate sind Begleitbeobachtungen; sie dürfen nicht kausal einer einzelnen zusätzlichen Codeänderung zugeschrieben werden.

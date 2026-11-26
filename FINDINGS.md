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
| [PERF-0002](#perf-0002--shader-reply-polling) | Shader-Antworten schneller erkennen | `LOAD_TIME_S` | 32 s → 26 s | −6 s (−18,75 %) | `AUTHOR_MEASURED` |
| [PERF-0003](#perf-0003--single-record-indirect-clamp) | Spezialisierter Ein-Record-Indirect-Clamp (synthetischer GPU-Benchmark) | `FRAME_TIME_MS` | 26,532 ms → 21,392 ms | −5,140 ms (−19,4 %) | `AUTHOR_MEASURED` |
| [PERF-0004](#perf-0004--three-copy-and-refresh-switches) | Gemeinsam gemessene Kopier-/Refresh-Optimierungen | `WORLD_FPS` | 12,7 FPS → 14,8 FPS | +2,1 FPS (+16,5 %) | `AUTHOR_MEASURED` |
| [PERF-0005](#perf-0005--reduced-descriptor-heap-scan-cadence) | Seltener vollständiger Deskriptor-Heap-Scan | `WORLD_FPS` | 14,2 FPS → 15,2 FPS | +1,0 FPS (+7,0 %) | `AUTHOR_MEASURED` |
| [PERF-0006](#perf-0006--asynchronous-copy-back-relay) | Asynchroner Copy-back-Relay (bereits in Baseline) | `FRAME_TIME_MS` | 274 ms → 126 ms | −148 ms (−54,0 %) | `AUTHOR_MEASURED` |

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
| Baseline integration | Baseline-ID + vollständiger SHA; Status gemäß untenstehendem Vertrag; konkreter Code-/Herkunftsbeleg und Einschränkungen |
| Provenance / License | Herkunftsquelle, Lizenz nachweisbar oder `not verified` |
| Adoptions | Spätere übernommene Commits anderer Forks oder `none recorded` |

Ein Datensatz wird **nur** mit allen Pflichtfeldern angelegt. Lange Erklärungen dürfen zusätzlich unter `#### Technical notes` erscheinen; Standardfelder nicht umbenennen. Bei Übernahme oder erneuter Messung **bestehenden Datensatz ergänzen**, nicht zweite ID vergeben. Eine neue Messung ist kein Beweis für identische Hardware-/Szenenbedingungen.

## Baseline-Integrationsvertrag

| Status | Semantik |
| --- | --- |
| `ALREADY_PRESENT` | Technische Wirkung am fixierten Baseline-SHA nachweisbar enthalten |
| `PORTABLE` | Voraussichtlich direkt übertragbar; passende Schnittstellen und Herkunft nachgewiesen, noch nicht implementiert |
| `ADAPTATION_REQUIRED` | Gemeinsame oder anschlussfähige Architektur, Anpassungs-/Validierungsbedarf |
| `INCOMPATIBLE` | Konkreter technischer oder Lizenzkonflikt verhindert sinnvolle Übertragung |
| `UNKNOWN` | Evidenz zur Kompatibilität fehlt |

Die Einstufung bezieht sich ausschließlich auf [BASELINE.md](BASELINE.md) und den angegebenen SHA. Sie ändert keine gemessenen Quellwerte und ist keine Zusicherung einer lauffähigen Integration.

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
| Baseline integration | `BASE-0001`, [`2de998014dc85ad68cd6422a250400c7084ace57`](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57), `ADAPTATION_REQUIRED`: gemeinsamer Code-Vorfahr c9c932c und [Baseline-Deskriptor-Fallback](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/port_descriptors.rs) geprüft; Hot-page-Schalter fehlt. Port-2-Pfad und weitere VInAMD-Abhängigkeiten/Launcher-Konfiguration bei späterer Übernahme gezielt prüfen. Messgewinn auf Baseline unbekannt. |
| Provenance / License | Attribution zu den verlinkten VInAMD-Commits; mögliche Vorarbeiten anderer Forks vor Übernahme prüfen. Lizenzkompatibilität: `not verified`. |
| Adoptions | `none recorded` |

#### Technical notes

Der **isolierbar dokumentierte** Vergleich betrifft das Ein-/Ausschalten von Renderer-Port 2 innerhalb des aktualisierten VInAMD-Stands (82 ms gegenüber 72 ms). Die separate Änderung des gesamten Projektstands von 80 auf 72 ms und die gesunkene Stutter-Rate sind Begleitbeobachtungen; sie dürfen nicht kausal einer einzelnen zusätzlichen Codeänderung zugeschrieben werden.

### PERF-0002 — Shader reply polling

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` – je ein vom Quellprojekt berichteter Boot pro Zustand |
| Origin repository | [brcly/TuxBox](https://github.com/brcly/TuxBox) |
| Code commit(s) | [f0ffa49 – Warteintervall und Abfrage ausstehender Shader-Anfragen](https://github.com/brcly/TuxBox/commit/f0ffa49c7db563be182f91d4a9242c10b06305a2) |
| Evidence commit(s) | [f0ffa49 – Messwerte in Commit-Nachricht und Code-Diff](https://github.com/brcly/TuxBox/commit/f0ffa49c7db563be182f91d4a9242c10b06305a2); [Zuordnung ProjectScarlett zu GTA VI](https://github.com/brcly/TuxBox/blob/f0ffa49c7db563be182f91d4a9242c10b06305a2/docs/PROJECTSCARLETT.md) |
| Merged PR | `n/a` – keine PR-Zuordnung verifiziert |
| Subsystem / Mechanism | Shader-Compiler-IPC unter Wine: statt exponentiell wachsender Poll-Pause und wiederholter Suche nach nicht existierenden Antwortdateien wird die noch ausstehende Anfrage beobachtet. Das Intervall beträgt ein Achtel der bisherigen Wartezeit, begrenzt auf 1–25 ms; eine zusätzliche Antwortprüfung erfolgt spätestens nach 250 ms. |
| Metric; Before; After; Delta | **Start bis stabile Frames:** 32 s → 26 s, −6 s (−18,75 %). Begleitend Zeit für die ersten 756 Shader: 20 s → 15 s, −5 s (−25 %); keine zweite Finding-ID. |
| Test conditions | GTA VI (ProjectScarlett), RTX 4070 Ti SUPER, je ein Boot ohne gespeicherte Übersetzungen/DXC-Ergebnisse, warme Treiber-Caches. Weitere Hardware, Profil, Auflösung und exakte Bildqualität: `unknown`. Projekt nutzt Wine/Proton; 162 Crate-Tests unter GE-Proton11-14 gemeldet. |
| Evidence | `AUTHOR_MEASURED` – gleicher dokumentierter Cache-Ausgangszustand, ein Vorher-/Nachher-Paar; keine unabhängigen Rohlogs oder Wiederholungsserie. |
| Trade-offs | Keine gemessenen Gameplay-FPS; mögliche zusätzliche Polling-Last, keine vollständige GPU-Prüfung, keine AMD-Messung. Projekt dokumentiert weiterhin eingeschränkte Spielbarkeit. |
| Integration | Benötigt TuxBox-artigen dateibasierten Shader-Request/Reply-Mechanismus; nicht mit beliebiger Shader-Cache-Optimierung gleichzusetzen. |
| Baseline integration | `BASE-0001`, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57), `UNKNOWN`: [Baseline-Cache](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-pipeline/src/cache.rs) verwendet einen inhaltsadressierten Konvertierungscache, [Pipeline-Worker](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/pipeline/make.rs) lokale Condvar-Wartebudgets. Der konkret gemessene Datei-IPC-Pfad ist dort nicht nachgewiesen; Übertragbarkeit des Mechanismus hängt von einem entsprechenden Provider-Pfad ab, Zusatznutzen unbekannt. |
| Provenance / License | TuxBox-Commit und [Apache-2.0-Lizenz](https://github.com/brcly/TuxBox/blob/f0ffa49c7db563be182f91d4a9242c10b06305a2/LICENSE); Drittkomponenten vor Übernahme separat prüfen. |
| Adoptions | `none recorded` |

### PERF-0003 — Single-record indirect clamp

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` – synthetischer Hardware-GPU-Benchmark, kein Gameplay-Test |
| Origin repository | [ProjectLeonidaL/leonida](https://github.com/ProjectLeonidaL/leonida) |
| Code commit(s) | [0b2fc6a – Ein-Record-Clamp und Benchmark](https://github.com/ProjectLeonidaL/leonida/commit/0b2fc6a6a2d35511735c4363b8d1eb68963b694f) |
| Evidence commit(s) | [ba53e40 – Messprotokoll](https://github.com/ProjectLeonidaL/leonida/commit/ba53e40ba968240ab9f41a9664ba8a0dd28584a4); [4dbb00e – korrigierte Einordnung der Ausreißer](https://github.com/ProjectLeonidaL/leonida/commit/4dbb00e4f717dc9f7ffb730576c83ea5795dd76f) |
| Merged PR | [#40](https://github.com/ProjectLeonidaL/leonida/pull/40), [Merge b5fa843](https://github.com/ProjectLeonidaL/leonida/commit/b5fa843361eff27a0e778e7811f3481eff9c5224) |
| Subsystem / Mechanism | D3D12 Indirect-Argument-Clamp: optional ein Thread statt 64 für genau einen Record; keine gruppenweiten Arrays/Barrieren für diesen Spezialfall. Budget-Sättigung, Argumentlimits und UAV-Barrieren bleiben im Quellprojekt erhalten; allgemeiner Clamp bei Mehrfachrecords und als Fallback. |
| Metric; Before; After; Delta | **GPU-Zeit pro synthetischem Benchmark-Frame mit 3.200 eigenen Dreiecks-Draws und GPU-lokalem Budget:** Median der Laufmediane 26,532 ms → 21,392 ms; −5,140 ms (−19,4 %). CPU-gemapptes Budget: 26,236 ms → 26,553 ms, kein klarer Gewinn; CPU-Aufzeichnung ca. 4,9 ms unverändert. |
| Test conditions | RTX 5090 32 GB, NVIDIA 580.173.02, Core i9-14900KS, Pop!_OS 22.04, Rust 1.98.1 Windows-MSVC Release, Proton 11/Wine 11, vkd3d-proton; identische Barrieren und Budget-Platzierung, acht Prozesse mit neun Stichproben je Fall, alternierende Reihenfolge nach Warm-up. Synthetische Dreiecke, keine Spielszene oder Gameplay-Auflösung. |
| Evidence | `AUTHOR_MEASURED` – Hardware-Timestamps; 72 Samples je Fall, Werte als Median der acht Laufmediane. Quelle berichtet 1.624 Paritätsfälle, einschließlich Budget und Ausgabe; keine unabhängige Reproduktion. |
| Trade-offs | Kein klarer Gewinn bei CPU-gemapptem Budget; einzelne Ausreißer bis 208 ms dort, Ursache nicht gemessen. Keine Gameplay-FPS- oder Baseline-Messung, andere Karten ungeprüft; Schalter standardmäßig aus. |
| Integration | Passende Indirect-Argument- und Budgetsemantik, Auswahl für exakt einen Record und Shader-Fallback erforderlich. |
| Baseline integration | `BASE-0001`, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57), `ADAPTATION_REQUIRED`: [Baseline indirect.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/indirect.rs) enthält den gruppierten 64-Thread-Clamp mit Shared Arrays; dort zusätzliche Budgetdiagnostik (Offsets 12/16) gegenüber dem gezeigten Quellpatch erhalten. Nicht unverändert übernehmen; GPU-Parität und Laufzeit separat prüfen. |
| Provenance / License | Original-Commit im Quellprojekt; [Workspace GPL-3.0-or-later](https://github.com/ProjectLeonidaL/leonida/blob/2689d10a43fe401a9b0e2d74ae0dfab359ba9a87/Cargo.toml). Einzelne Vorarbeiten und Drittkomponenten vor Übernahme prüfen. |
| Adoptions | `none recorded` |

### PERF-0004 — Three copy and refresh switches

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [LooperSalty/AnyXboxX](https://github.com/LooperSalty/AnyXboxX) |
| Code commit(s) | [26c11d4 – Portierung #383](https://github.com/LooperSalty/AnyXboxX/commit/26c11d4d9ad468e092507005dfd310e26fe0a7fe); [92a1949 – Aktivierung #386](https://github.com/LooperSalty/AnyXboxX/commit/92a194902faa9b96306ee3a97cce37a01f7e5ea9) |
| Evidence commit(s) | [92a1949 – settings.rs mit A/B-Werten](https://github.com/LooperSalty/AnyXboxX/commit/92a194902faa9b96306ee3a97cce37a01f7e5ea9); [a5b0aed – neue versionierte Messübersicht](https://github.com/LooperSalty/AnyXboxX/commit/a5b0aed87a8ab50cf14a7580aa08295557698ab5) |
| Merged PR | [#383](https://github.com/LooperSalty/AnyXboxX/pull/383), [#386](https://github.com/LooperSalty/AnyXboxX/pull/386) |
| Subsystem / Mechanism | `VICTORY_STEP_INDEX` indexiert Kopien pro Schritt; `VICTORY_CARD_SYNC_BATCH` gruppiert Kopien, Barrieren und Hashes; `VICTORY_PARALLEL_REFRESH` parallelisiert Texturprüfungen. Nur das Dreierpaket gemeinsam gemessen. |
| Metric; Before; After; Delta | Mediane Welt-FPS: 12,7 aus → 14,8 ein; +2,1 FPS (+16,5 %, Quelle gerundet +17 %). Keine Einzelwirkung ableitbar. |
| Test conditions | Windows 11; RTX 4070 SUPER; i7-13700KF; Series X; derselbe Lauf mit Wechsel alle 20 s; erste 6 s pro Wechsel verworfen; 17 Ein-/19 Aus-Stichproben; exakte Auflösung, Szenenfolge und Gesamtlaufdauer unknown. Bildqualität nicht unabhängig verifiziert. |
| Evidence | `AUTHOR_MEASURED` – dokumentierter In-Session-A/B in #386, Zahlen im Aktivierungscommit und erneut in neuem Dokumentationscommit. Keine unabhängige Reproduktion. |
| Trade-offs | Kein Crash oder Device Loss laut Quelle; Langzeitstabilität und Bildgleichheit nicht unabhängig geprüft. Kein additiver Gewinn mit anderen Findings. |
| Integration | Copy-step-Index, GPU-Barrier-/Hash-Reihenfolge und Refresh-Threading gemeinsam gegen vorhandenen Renderer abgleichen; getrennt validieren. |
| Baseline integration | BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; `ADAPTATION_REQUIRED`. [host.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs) enthält bereits multicore parallel refresh; [card_steps.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host/card_steps.rs) zeigt separaten Card-Step-Pfad. Keine Identität des gesamten gemessenen Switch-Pakets nachgewiesen; Überlappungen verhindern eine ungeprüfte Übernahme. |
| Provenance / License | Portierung in #383 mit Herkunftsangaben aus fxceless/victory-gta6 (84c5dc00, 6aed0780, ceb9e1f7); [THIRD_PARTY.md](https://github.com/LooperSalty/AnyXboxX/blob/26c11d4d9ad468e092507005dfd310e26fe0a7fe/THIRD_PARTY.md) nennt GPL-3.0. Ursprüngliche Kurzreferenzen nicht separat abrufbar; konkrete Portierungsdiffs und Messcommit geprüft. Lizenz jeder übernommenen Komponente vor Integration prüfen. |
| Adoptions | none recorded |

### PERF-0005 — Reduced descriptor heap scan cadence

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [LooperSalty/AnyXboxX](https://github.com/LooperSalty/AnyXboxX) |
| Code commit(s) | [ed60e31 – Scan-Intervall](https://github.com/LooperSalty/AnyXboxX/commit/ed60e312e634fe89a3ea96190b3a995fd4b9be28); [49786c8 – Aktivierung und Messung](https://github.com/LooperSalty/AnyXboxX/commit/49786c8cd85faf626c39c0db14e8e83ac4ce847c) |
| Evidence commit(s) | [49786c8 – settings.rs mit A/B-Werten](https://github.com/LooperSalty/AnyXboxX/commit/49786c8cd85faf626c39c0db14e8e83ac4ce847c); [a5b0aed – neue versionierte Messübersicht](https://github.com/LooperSalty/AnyXboxX/commit/a5b0aed87a8ab50cf14a7580aa08295557698ab5) |
| Merged PR | [#394](https://github.com/LooperSalty/AnyXboxX/pull/394); [Merge](https://github.com/LooperSalty/AnyXboxX/commit/ce0f90f1e469132cb0a866e0dcec9ca0f53e874a) |
| Subsystem / Mechanism | `VICTORY_HEAP_SCAN_ALT` reduziert vollständige Heap-Scans von jedem Durchlauf auf jeden 16. Durchlauf; normale Aktualisierungspfade bleiben relevant. |
| Metric; Before; After; Delta | Mediane Welt-FPS: 14,2 → 15,2; +1,0 FPS (+7,0 %). Ergänzend: 5-s-Fenster über 100 ms/Frame: 4 aus vs. 1 ein; wegen ungleicher Stichprobenzahlen kein normierter Stutter-Ratenvergleich. |
| Test conditions | RTX 4070 SUPER; Windows 11; Series X; Wechsel alle 20 s im selben Lauf; 11 Aus-/13 Ein-Stichproben. Exakte Auflösung, Gesamtdauer und Szene unknown; Gesichter laut PR visuell geprüft. |
| Evidence | `AUTHOR_MEASURED` – #394 und versionierter settings.rs-Messkommentar; neuer Dokumentationscommit bestätigt die Messung. Keine unabhängige Reproduktion. |
| Trade-offs | Verzögertes Entdecken übersehener Deskriptoränderungen möglich; Bildqualität und Langzeitstabilität nicht umfassend validiert. Kein additiver Gewinn mit PERF-0004 nachgewiesen. |
| Integration | Scan-Takt mit Write-Journal, Hot-Page-Fallback und Descriptor-Invalidierung abgleichen; keine Blindübernahme. |
| Baseline integration | BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; `ADAPTATION_REQUIRED`. [draw.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/draw.rs) enthält heap_textures; [descriptor/host.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/descriptor/host.rs) und [port_descriptors.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/port_descriptors.rs) verwenden verwandte Descriptor-/Fallback-Mechanismen. Der separate AnyXboxX-heap_pass-Pfad und identische Switch-Semantik sind nicht nachgewiesen. |
| Provenance / License | Portierungsdiff ed60e31 nennt Cherry-Pick 4e3592d599fc5d42c37f9ee0640ea5cd1bf36715; dieser ursprüngliche Commit war separat nicht abrufbar. Erreichbarer Portierungsdiff und Aktivierung belegt; Einzelpatch-Lizenz not verified. |
| Adoptions | none recorded |

### PERF-0006 — Asynchronous copy-back relay

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [ProjectLeonidaL/leonida](https://github.com/ProjectLeonidaL/leonida) |
| Code commit(s) | [c96b4f2 – schnellere Defaults inklusive Relay, fxceless](https://github.com/ProjectLeonidaL/leonida/commit/c96b4f2b8365bf43d54ab074940923ef8267f4c6) |
| Evidence commit(s) | [a8beb52 – neuer Ein-/Aus-Vergleich in fork-notes.md](https://github.com/ProjectLeonidaL/leonida/commit/a8beb52f13e4e7f3519134307e58e84a0fa9604a); [850aa03 – Merge](https://github.com/ProjectLeonidaL/leonida/commit/850aa035f9912c95365efb8aa71fbc23f33c9e42) |
| Merged PR | [#46 – Messdokumentation](https://github.com/ProjectLeonidaL/leonida/pull/46); ursprünglicher Code-PR n/a |
| Subsystem / Mechanism | Relay-Thread übernimmt Copy-back neben der Submission; Stufe 2 begrenzt Warten auf tatsächlich betroffene Datenbereiche. Messung isoliert Relay-Schalter, nicht das gesamte frühere Default-Änderungspaket. |
| Metric; Before; After; Delta | Übliche Welt-Frametime (nicht als Median ausgewiesen): Relay aus 274 ms → Relay ein 126 ms; −148 ms (−54,0 %). |
| Test conditions | RTX 3080 12 GB, Ryzen 7 7800X3D, Treiber 621.24; je ein unbeaufsichtigter Diagnostics-Lauf auf base; Welt nach ca. 70 s, Vergleich 170–330 s, 4.650–4.950 Draws, periodische Tastatureingaben. Auflösung und exakte Bildqualität unknown. |
| Evidence | `AUTHOR_MEASURED` – explizites Relay-Aus-/Ein-Ergebnis in neuem versioniertem fork-notes.md; keine unabhängige Reproduktion. |
| Trade-offs | Ein Lauf je Zustand; keine nachgewiesene Langzeitstabilität oder exakte Bildgleichheit. Kein zusätzlicher Baseline-Gewinn. |
| Integration | Relay-Stufe, Copy-back-Reihenfolge und Queue-Synchronisation müssen zusammenpassen. Vorhandene Implementierung nicht erneut portieren. |
| Baseline integration | BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; `ALREADY_PRESENT`. [Baseline host.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs) enthält Relay und relay_exact (Stufe 2 bei unset/2), [shadow.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/shadow.rs) enthält standardmäßig aktiven Relay. Entsprechende Funktionen im [Quell-host.rs](https://github.com/ProjectLeonidaL/leonida/blob/1c5bd30911b19ee076d0187d60a80b2910de27bc/crates/leonida-gpu/src/host.rs) und [Quell-shadow.rs](https://github.com/ProjectLeonidaL/leonida/blob/2689d10a43fe401a9b0e2d74ae0dfab359ba9a87/crates/leonida-gpu/src/shadow.rs) geprüft; keine Behauptung identischer Gesamtleistung. |
| Provenance / License | Default-Aktivierung c96b4f2 von fxceless; [Quell-Workspace](https://github.com/ProjectLeonidaL/leonida/blob/2689d10a43fe401a9b0e2d74ae0dfab359ba9a87/Cargo.toml) GPL-3.0-or-later; ursprünglicher Implementierungscommit nicht separat ermittelt. |
| Adoptions | Technische Wirkung bereits in BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; kein neuer Port. |

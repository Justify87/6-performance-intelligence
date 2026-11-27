# FINDINGS.md — Kanonisches Performance-Register

**Einzige maßgebliche Datenquelle** für gemessene Gewinne und ihre Baseline-Integrationsrelevanz. Historische Datensätze bleiben unabhängig von ihrer heutigen Relevanz vollständig erhalten. Neue Änderungen, die erstmals in einem **seit dem jeweiligen Cursor neuen Standardbranch-Commitbereich** bewertet werden und den Aufnahmevertrag in [`AGENTS.md`](AGENTS.md) einschließlich konkreter Integrationsrelevanz erfüllen, dürfen eingetragen werden. Ausdrücklich beauftragte historische Scope-Reviews gemäß WATCHLIST sind die einzige Ausnahme zur regulären Range-Grenze; ihr gesonderter Umfang muss im Datensatz belegt sein, ohne Cursor-Vorschub. Neue Evidenz in einem neuen Commit darf einen älteren Code-Commit als Ursache referenzieren.

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
| [PERF-0007](#perf-0007--rt-submission-ordering-and-forwarded-fills) | RT-Submission-Reihenfolge und Weiterleitung von Fills | `FRAME_TIME_MS` | 131,1 ms → 118,7 ms | −12,4 ms (−9,5 %) | `AUTHOR_MEASURED` |
| [PERF-0008](#perf-0008--lean-indirect-spread-dispatch) | Indirekte Compute-Dispatches über einfache Signatur | `FRAME_TIME_MS` | 225 ms → 208 ms | −17 ms (−7,6 %) | `AUTHOR_MEASURED` |
| [PERF-0009](#perf-0009--retain-translated-shader-on-cache-miss) | Shader-Cache-Miss ohne erneutes Lesen und Dekodieren | `LOAD_TIME_S` | 0,0002975 s → 0,0002209 s | −0,0000766 s (−25,7 %) | `AUTHOR_MEASURED` |
| [PERF-0010](#perf-0010--wake-live-shader-worker-during-concurrent-jobs) | Shader-Worker bei parallelen Jobs sofort wecken | `LOAD_TIME_S` | 0,0504–0,0508 s → 0,0017–0,0024 s | −0,0480 bis −0,0491 s (konservative Bereichsdifferenz) | `AUTHOR_MEASURED` |
| [PERF-0011](#perf-0011--wine-placeholder-reservation-tracking) | Wine-Placeholder-Tracking statt wiederholter Speicherabfragen | `FRAME_TIME_MS` | 362 ms → 38 ms | −324 ms (−89,5 %) | `AUTHOR_MEASURED` |
| [PERF-0012](#perf-0012--reserved-vram-room-for-large-upscaler-buffers) | VRAM-Reserve für große Upscaler-Puffer | `FRAME_TIME_MS` | 55,7–70,8 ms → 0,10–0,28 ms | −55,42 bis −70,70 ms pro Pass (Bereichsdifferenz) | `AUTHOR_MEASURED` |
| [PERF-0013](#perf-0013--vendor-specific-small-texture-pool-alignment) | 4-KiB-Texturpool-Alignment auf Nicht-AMD-GPUs | `VRAM_GIB` | 16,7 GiB → 13,3 GiB | −3,4 GiB (−20,4 %) | `AUTHOR_MEASURED` |

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
| Baseline integration | Baseline-ID + vollständiger SHA; technischer Status und Integrationsrelevanz gemäß untenstehendem Vertrag; konkreter Code-/Herkunftsbeleg, Neuartigkeit und Einschränkungen |
| Provenance / License | Herkunft und am konkreten Code geltende Lizenz mit SHA-bezogenem Beleg, einschließlich Drittkomponenten; bei historischen Unsicherheiten `not verified` beibehalten; keine Lizenzfreigabe aus technischer Ähnlichkeit ableiten |
| Adoptions | Spätere übernommene Commits anderer Forks oder `none recorded` |

Ein Datensatz wird **nur** mit allen Pflichtfeldern angelegt. Lange Erklärungen dürfen zusätzlich unter `#### Technical notes` erscheinen; Standardfelder nicht umbenennen. Bei Übernahme oder erneuter Messung **bestehenden Datensatz ergänzen**, nicht zweite ID vergeben. Eine neue Messung ist kein Beweis für identische Hardware-/Szenenbedingungen.

## Baseline-Integrationsvertrag

| Status | Semantik |
| --- | --- |
| `ALREADY_PRESENT` | Technische Wirkung am fixierten Baseline-SHA nachweisbar enthalten |
| `PORTABLE` | Voraussichtlich direkt übertragbar; passende Schnittstellen und Herkunft nachgewiesen, noch nicht implementiert |
| `ADAPTATION_REQUIRED` | Gemeinsame oder anschlussfähige Architektur, Anpassungs-/Validierungsbedarf |
| `INCOMPATIBLE` | Konkreter technischer Konflikt oder außerhalb des definierten nativen Zielpfads; eine begrenzte sinnvolle Übertragung setzt einen nicht vorhandenen Kernpfad oder Architekturwechsel voraus |
| `UNKNOWN` | Evidenz zur Kompatibilität fehlt |

Die technische Einstufung bezieht sich ausschließlich auf [BASELINE.md](BASELINE.md) und den angegebenen SHA; Lizenz steht separat in Provenance / License. Sie ändert keine gemessenen Quellwerte und ist keine Zusicherung einer lauffähigen Integration.

Im selben Feld zusätzlich genau eine Integrationsrelevanz als `Relevanz:` gefolgt von genau einem der folgenden Codewerte notieren:

| Relevanz | Semantik |
| --- | --- |
| `CANDIDATE` | Technischer Status PORTABLE oder ADAPTATION_REQUIRED, konkreter fehlender Mechanismus, zurechenbare Messung und grundsätzlich zulässige belegte Provenienz/Lizenz; tatsächliche Baseline-Wirkung und Integrationsprüfung offen |
| `CONDITIONAL` | Historisch erfasste Messung, aber Zusatznutzen, Attribution, Voraussetzung, Übertragbarkeit oder Lizenz noch nicht hinreichend belegt; keine aktive Integrationsrangliste |
| `REFERENCE_ONLY` | Historische Evidenz ohne belegten neuen Integrationsbedarf: bereits vorhandener äquivalenter Mechanismus, außerhalb des Zielumfangs oder konkreter Ausschlussgrund; keine aktive Integrationsrangliste |

Neuaufnahme nur mit CANDIDATE nach AGENTS. Bestehende IDs und historische Messdaten niemals löschen oder neu vergeben. Neue relevante Evidenz, Adoptionen und Gegenbelege ergänzen, Relevanz bei belastbarer Änderung neu bewerten. Eine reine Konfigurationsänderung für bereits vorhandenen Code zählt nicht als neuer Code-Integrationsgewinn. Die Relevanz ist allein in diesem Feld kanonisch; PERFORMANCE leitet die Darstellung daraus ab.

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
| Baseline integration | `BASE-0001`, [`2de998014dc85ad68cd6422a250400c7084ace57`](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57), `ADAPTATION_REQUIRED`: gemeinsamer Code-Vorfahr c9c932c und [Baseline-Deskriptor-Fallback](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/port_descriptors.rs) geprüft; Hot-page-Schalter fehlt. Port-2-Pfad und weitere VInAMD-Abhängigkeiten/Launcher-Konfiguration bei späterer Übernahme gezielt prüfen. Messgewinn auf Baseline unbekannt. Relevanz: `CONDITIONAL`. **Integrationsreview 2026-11-27:** Hot-page-Erweiterung technisch plausibel, aber 82→72 ms misst den gesamten aktualisierten Port 2 gegen ausgeschaltet; Port 2 ist in der Baseline vorhanden. Fehlender Hot-page-Teil muss isoliert oder als zurechenbare zusätzliche Messgruppe belegt werden; kein isolierter 12,2%-Gewinn. |
| Provenance / License | Attribution zu den verlinkten VInAMD-Commits; mögliche Vorarbeiten anderer Forks vor Übernahme prüfen. Lizenzkompatibilität: `not verified`. Deklarierte Quell-Workspace-Lizenz am [Code-Commit](https://github.com/iLucasRT/VInAMD/blob/69a99e313a764370fda3bb850ef8df1c1018fb72/Cargo.toml) GPL-3.0-or-later; Herkunft möglicher Vorarbeiten weiter separat prüfen. |
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
| Baseline integration | `BASE-0001`, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57), `INCOMPATIBLE`: [Baseline-Cache](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-pipeline/src/cache.rs) verwendet einen inhaltsadressierten Konvertierungscache, [Pipeline-Worker](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/pipeline/make.rs) lokale Condvar-Wartebudgets. Der konkret gemessene Datei-IPC-Pfad ist dort nicht nachgewiesen; Übertragbarkeit des Mechanismus hängt von einem entsprechenden Provider-Pfad ab, Zusatznutzen unbekannt. Relevanz: `REFERENCE_ONLY`. **Integrationsreview 2026-11-27:** Datei-IPC-Polling setzt fremden Provider-/Request-Reply-Vertrag voraus; native Baseline nutzt lokale Worker/Cache. Kein passender nativer Zielpfad; Einführung einer neuen IPC-Architektur nicht zulässig. |
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
| Baseline integration | `BASE-0001`, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57), `ADAPTATION_REQUIRED`: [Baseline indirect.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/indirect.rs) enthält den gruppierten 64-Thread-Clamp mit Shared Arrays; dort zusätzliche Budgetdiagnostik (Offsets 12/16) gegenüber dem gezeigten Quellpatch erhalten. Nicht unverändert übernehmen; GPU-Parität und Laufzeit separat prüfen. Relevanz: `CANDIDATE`. **Integrationsreview 2026-11-27:** Konkrete 64-Thread-Clamp-Zielstelle und messbare Spezialisierung; GPU-lokales Budget, Offsets 12/16 und CPU-gemappte Negativmessung beachten. Kein Gameplay-Gewinn behauptet. |
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
| Baseline integration | BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; `ALREADY_PRESENT`. [host.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs) enthält bereits multicore parallel refresh; [card_steps.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host/card_steps.rs) zeigt separaten Card-Step-Pfad. Keine Identität des gesamten gemessenen Switch-Pakets nachgewiesen; Überlappungen verhindern eine ungeprüfte Übernahme. Relevanz: `REFERENCE_ONLY`. **Integrationsreview 2026-11-27:** Erweiterter Codeabgleich: [stale_under_flights](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs#L3173) und [declared_writes](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs#L3965) nutzen ByCopy-Indizes/Seen-Set statt linearer Suche; [GPU-Sync](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs#L4477) zeichnet Kopien, Barrier und Hash-Pässe in drei Phasen; [wide_textures](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs#L8256) und [refresh_textures](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs#L8560) bündeln parallele Texturprüfungen (multicore standardmäßig). Damit sind alle drei gemessenen Optimierungsprinzipien vorhanden, auch wenn Implementierungen/Schalter differieren. Kein zusätzlicher Anteil der Paketmessung nachgewiesen; nur separat belegte Restunterschiede neu bewerten. |
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
| Baseline integration | BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; `ALREADY_PRESENT`. [draw.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/draw.rs) enthält heap_textures; [descriptor/host.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/descriptor/host.rs) und [port_descriptors.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/port_descriptors.rs) verwenden verwandte Descriptor-/Fallback-Mechanismen. Der separate AnyXboxX-heap_pass-Pfad und identische Switch-Semantik sind nicht nachgewiesen. Relevanz: `REFERENCE_ONLY`. **Integrationsreview 2026-11-27:** Erweiterter Abgleich: [heap_spread / HEAP_ROUND](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/draw.rs#L583) und [heap_buffer_pass](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/draw.rs#L861) prüfen geänderte Buffer-Views sowie die übrigen verteilt über 16 Durchläufe, standardmäßig aktiv (alternativ alle 16 zusammen). Der [Quellpfad](https://github.com/LooperSalty/AnyXboxX/blob/ed60e312e634fe89a3ea96190b3a995fd4b9be28/third_party/victory/crates/victory-gpu/src/heap_pass.rs#L163) reduziert den Rohrecord-Vollscan der gehaltenen Buffer-Views auf 16. Gleiche Optimierungsrichtung bereits realisiert; andere Invalidierungssemantik, keine Identität oder zusätzlicher Vorteil belegt. Keine pauschale weitere Scan-Verzögerung. |
| Provenance / License | Portierungsdiff ed60e31 nennt Cherry-Pick 4e3592d599fc5d42c37f9ee0640ea5cd1bf36715; dieser ursprüngliche Commit war separat nicht abrufbar. Erreichbarer Portierungsdiff und Aktivierung belegt; Einzelpatch-Lizenz not verified. [Vendored Workspace am Portierungscommit](https://github.com/LooperSalty/AnyXboxX/blob/ed60e312e634fe89a3ea96190b3a995fd4b9be28/third_party/victory/Cargo.toml) GPL-3.0-or-later; nicht separat belegte ursprüngliche Einzelpatch-Herkunft bleibt offen. |
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
| Baseline integration | BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; `ALREADY_PRESENT`. [Baseline host.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs) enthält Relay und relay_exact (Stufe 2 bei unset/2), [shadow.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/shadow.rs) enthält standardmäßig aktiven Relay. Entsprechende Funktionen im [Quell-host.rs](https://github.com/ProjectLeonidaL/leonida/blob/1c5bd30911b19ee076d0187d60a80b2910de27bc/crates/leonida-gpu/src/host.rs) und [Quell-shadow.rs](https://github.com/ProjectLeonidaL/leonida/blob/2689d10a43fe401a9b0e2d74ae0dfab359ba9a87/crates/leonida-gpu/src/shadow.rs) geprüft; keine Behauptung identischer Gesamtleistung. Relevanz: `REFERENCE_ONLY`. **Integrationsreview 2026-11-27:** Relay-Stufe 2 vorhanden und im Produktionspfad standardmäßig aktiv, sofern Konfiguration und Snapshot-Verträge dies zulassen; nicht erneut als Integrationsgewinn zählen. |
| Provenance / License | Default-Aktivierung c96b4f2 von fxceless; [Quell-Workspace](https://github.com/ProjectLeonidaL/leonida/blob/2689d10a43fe401a9b0e2d74ae0dfab359ba9a87/Cargo.toml) GPL-3.0-or-later; ursprünglicher Implementierungscommit nicht separat ermittelt. |
| Adoptions | Technische Wirkung bereits in BASE-0001, `2de998014dc85ad68cd6422a250400c7084ace57`; kein neuer Port. |

### PERF-0007 — RT submission ordering and forwarded fills

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [avatarmanz/VIctory-7900XTX](https://github.com/avatarmanz/VIctory-7900XTX) |
| Code commit(s) | [84917c34f63e88ba866e2fa45dc796385ba56f2c](https://github.com/avatarmanz/VIctory-7900XTX/commit/84917c34f63e88ba866e2fa45dc796385ba56f2c) |
| Evidence commit(s) | [014d52dedf837d1ac660a0bd256c4704be9eeb40](https://github.com/avatarmanz/VIctory-7900XTX/commit/014d52dedf837d1ac660a0bd256c4704be9eeb40) |
| Merged PR | n/a |
| Subsystem / Mechanism | VICTORY_PORT_RT erzwingt die vorhandene Placed-Order und schneidet Submissions hinter abhängigen Kopien. Innerhalb derselben Liste liest forward_fill die passende Kopierquelle statt einer globalen Wartephase; konservative Ablehnung nicht auflösbarer Abhängigkeiten bleibt. |
| Metric; Before; After; Delta | Stadt gehend: 131,1 → 118,7 ms (−12,4 ms; −9,5 %). Stadt stehend: 145,3 → 126,2 ms (−19,1 ms; −13,1 %). Boot gehend: 94,5 → 75,6 ms (−18,9 ms; −20,0 %); Boot stehend: 58,2 → 46,2 ms (−12,0 ms; −20,6 %). |
| Test conditions | Series X, Ausgabe 4K/intern 1440p, RT an, Ports 1/2/5/6/7, -Walk, rts-3/2 und rts-5/4. Projekt-README nennt Windows 11, i9-12900K, RX 7900 XTX und 64 GB RAM als Testsystem; Einzelrun-Hardware und Dauer nicht separat angegeben. |
| Evidence | AUTHOR_MEASURED: vier Vergleichswerte im Evidenzcommit; Bilder laut Autor wie Kontrolle, kein Crash; keine unabhängigen Rohdaten. |
| Trade-offs | Opt-in; keine Langzeitmessung. RT-BVH-Walker weiter 56–60 ms GPU-Zeit pro Frame laut [d13e9e0](https://github.com/avatarmanz/VIctory-7900XTX/commit/d13e9e0c6713c419ed67ebeaf8ab07d90f8f0820); Directory-Lookup ohne messbaren Kostenanteil, keine zusätzliche Optimierung aus dem Benchmark ableiten. |
| Integration | Placed-Order, Card-Step-Planung, Quellbereichsvalidierung und tatsächliche Copy-/Fill-Reihenfolge zusammen prüfen. |
| Baseline integration | BASE-0001, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57); `ADAPTATION_REQUIRED`: [crates/victory-gpu/src/host/card_steps.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host/card_steps.rs) enthält placed_order/cut_for und BEHIND_PLACED; [crates/victory-gpu/src/host.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/host.rs) enthält noch kein forward_fill. Vorhandene Vorarbeiten erhalten, neue Quelle und Synchronisationsregeln ergänzen. Relevanz: `CANDIDATE`. **Integrationsreview 2026-11-27:** [Git-Compare](https://github.com/avatarmanz/VIctory-7900XTX/compare/2de998014dc85ad68cd6422a250400c7084ace57...84917c34f63e88ba866e2fa45dc796385ba56f2c) bestätigt 17 vorausliegende, 0 zurückliegende Commits; konkrete forward_fill-Lücke und gleicher nativer RT-Pfad. Notwendige Zwischenabhängigkeiten und korrekte Bild-/Synchronisationssemantik später prüfen; RT-aus-Nutzen nicht belegt. |
| Provenance / License | Original belegter Code-Commit 84917c3, identischer späterer Commit in cool_fox (gleiche Author-Zeit, spätere Committer-Zeit). [GPL-3.0-or-later Workspace](https://github.com/avatarmanz/VIctory-7900XTX/blob/84917c34f63e88ba866e2fa45dc796385ba56f2c/Cargo.toml); Drittkomponenten getrennt prüfen. |
| Adoptions | [DoubleGhosty/cool_fox](https://github.com/DoubleGhosty/cool_fox/commit/f2e5852498cdef30629f811f51084fb356506da4) (Codeübernahme; keine eigene A/B-Messung). |

### PERF-0008 — Lean indirect spread dispatch

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [fxceless/victory-fork](https://github.com/fxceless/victory-fork) |
| Code commit(s) | [a7ee5dfa46123b74176a3d3bd39dc5e22a18d666](https://github.com/fxceless/victory-fork/commit/a7ee5dfa46123b74176a3d3bd39dc5e22a18d666) |
| Evidence commit(s) | [a7ee5dfa46123b74176a3d3bd39dc5e22a18d666](https://github.com/fxceless/victory-fork/commit/a7ee5dfa46123b74176a3d3bd39dc5e22a18d666) |
| Merged PR | [#1352](https://github.com/fxceless/victory-fork/pull/1352) |
| Subsystem / Mechanism | VICTORY_SPREAD_LEAN teilt bis 64 Records in einfache und Spread-Records auf, führt nur nötige Spread-Dispatches per Prädikation aus und behält große Batches auf dem alten Pfad. Keine Kürzung großer Dispatches auf 65.535 Gruppen. |
| Metric; Before; After; Delta | Frame: 225 → 208 ms (−17 ms; −7,6 %). GPU-Zeit pro Frame: 108 → 91 ms (−17 ms; −15,7 % Differenz der zusammengefassten Werte); Median der gepaarten GPU-Differenzen separat −12,8 ms. |
| Test conditions | RTX 3060 12 GB, stehend, neun Paare, Wechsel alle 20 s im selben A/B-Modus; CPU, exakte Szene, Auflösung und Profil unknown. WARP-Test: gleiche Group-IDs bei 100.000 Gruppen, gemischte Records. |
| Evidence | AUTHOR_MEASURED: GPU-Zeit in jedem Paar niedriger; Code und Messung im selben erreichbaren Commit. |
| Trade-offs | Vorbereitungszeit +4 ms; opt-in. Größere Batches behalten Spread-Signatur; Bildgleichheit und Dauerbetrieb nicht unabhängig geprüft. Komplettes Abschalten von DISPATCH_SPREAD führte laut PR bei 2 von 4 Runs zu Device Loss; nicht mit Lean gleichsetzen. |
| Integration | Spread-ABI, Root-Constants, Clamp-Layout, Predicate und Shader-Group-ID-Semantik zusammenführen; nicht mit PERF-0003s Ein-Record-Clamp verwechseln. |
| Baseline integration | BASE-0001, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57); `UNKNOWN`: [crates/victory-gpu/src/indirect.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/indirect.rs) und [crates/victory-gpu/src/draw.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/draw.rs) zeigen verwandten D3D12-Indirect-Pfad ohne Lean-Spread-Symbole; zuerst benötigten Spread-Dispatch-Pfad und Budget-/Diagnostik-Parität prüfen. Relevanz: `CONDITIONAL`. **Integrationsreview 2026-11-27:** Der konkrete Spread-Dispatch-/Root-Constant-ABI-Pfad des Quellpatches ist am Anker nicht nachgewiesen ([draw](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/draw.rs), [indirect](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/indirect.rs)); erst Voraussetzungen und begrenzte Anpassbarkeit belegen. Zusätzliche Quell-Budgetdiagnostik verwendet Offsets 12/16, die in der Baseline schon anders belegt sind; zusammen mit PERF-0003 prüfen. |
| Provenance / License | Code und Messung im Commit a7ee5df (Autor grosterr laut GitHub, PR #1352); [GPL-3.0-or-later Workspace](https://github.com/fxceless/victory-fork/blob/a7ee5dfa46123b74176a3d3bd39dc5e22a18d666/Cargo.toml); Vorarbeiten vor Integration prüfen. |
| Adoptions | none recorded |

### PERF-0009 — Retain translated shader on cache miss

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [ProjectLeonidaL/leonida](https://github.com/ProjectLeonidaL/leonida) |
| Code commit(s) | [3b4834f2573e4abb8307fb1a1f1c967c83c756d0](https://github.com/ProjectLeonidaL/leonida/commit/3b4834f2573e4abb8307fb1a1f1c967c83c756d0); ursprünglicher [Marvisak/VIctory bca3ecd](https://github.com/Marvisak/VIctory/commit/bca3ecd00a3e0cf4814e0061cafc510f0a286ba2) |
| Evidence commit(s) | [e72f2b1a348b9f952a5c942800f37561fc250614](https://github.com/ProjectLeonidaL/leonida/commit/e72f2b1a348b9f952a5c942800f37561fc250614); [Korrektur der Ausreißer](https://github.com/ProjectLeonidaL/leonida/commit/ff305642ecaf2d4d7491fdd8701e72c5ca04b5c3) |
| Merged PR | [#51](https://github.com/ProjectLeonidaL/leonida/pull/51) |
| Subsystem / Mechanism | Übersetzter HostShader bleibt im Converter erhalten und wird bei Cache-Miss direkt zurückgegeben, statt neu geschriebene Datei zu lesen und zu dekodieren. Fehler beim Publizieren verwerfen den nutzbaren Shader nicht. |
| Metric; Before; After; Delta | 64 KiB: 297,5 → 220,9 µs je Lookup (−76,6 µs; −25,7 %). 8 KiB: 276,3 → 204,8 µs (−25,9 %); 256 KiB: 541,5 → 330,2 µs (−39,0 %). Registermetrik LOAD_TIME_S bezeichnet hier ausschließlich die synthetische Cache-Lookup-Latenz. |
| Test conditions | i9-14900KS, Pop!_OS 22.04/ext4, Proton 11/Wine 11, Rust 1.98.1 Windows Release, identische Adapter und realer Cache, synthetischer Clone statt Shader-Übersetzung. 12 gepaarte Runden, wechselnde Reihenfolge, 128 Lookups/Sample, 144 Samples einschließlich Hits, OS-Dateicache nicht geleert; keine GPU-Arbeit. |
| Evidence | AUTHOR_MEASURED: Messwerte in versioniertem fork-notes.md, Ausgabe/gespeicherte Bytes/Hit-Miss-Zählung geprüft. |
| Trade-offs | Cache-Hits: gepaarte Medianänderungen −1,0 %, −1,7 %, +1,1 %, kein Gewinn behauptet. Zwei langsamere gepatchte 8-KiB-Paare (korrigierter Bericht); einmalige Cache-Neubildung, native Windows-/Gameplay-Wirkung unknown. |
| Integration | Vorhandenes Cache-Format, atomare Publikation, Fehler- und Refresh-Pfade, Fingerprint und Hit-/Miss-Diagnostik bewahren. |
| Baseline integration | BASE-0001, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57); `ADAPTATION_REQUIRED`: [crates/victory-gpu/src/shader_cache.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/shader_cache.rs) liest in cached_by die gerade geschriebene Datei erneut; zusätzlich SOURCE-/Stale-/Refresh-Pfade vorhanden, die der gemessene Leonida-Patch nicht unverändert abdeckt. Relevanz: `CANDIDATE`. **Integrationsreview 2026-11-27:** Redundantes Einlesen im konkreten cached_by-Zielpfad belegt; begrenzte Anpassung mit Erhalt der SOURCE-/Stale-/Refresh-/Fehlersemantik. Gemessen ist Cache-Miss-Latenz, kein Spiel-FPS-Gewinn. |
| Provenance / License | Upstream-Original bca3ecd geprüft; ProjektLeonidaL-Adaptation und eigene synthetische Messung im neuen Bereich. GPL-3.0-or-later laut bereits verankertem [Quell-Workspace](https://github.com/ProjectLeonidaL/leonida/blob/2689d10a43fe401a9b0e2d74ae0dfab359ba9a87/Cargo.toml). [Original-Workspace am Code-Commit](https://github.com/Marvisak/VIctory/blob/bca3ecd00a3e0cf4814e0061cafc510f0a286ba2/Cargo.toml) ebenfalls GPL-3.0-or-later. |
| Adoptions | [AnyXboxX Vice-Tay-Import](https://github.com/LooperSalty/AnyXboxX/commit/7e3e672acbcbf7806496de3aa5a0c0b332631330) enthält laut [versioniertem shader_cache.rs](https://github.com/LooperSalty/AnyXboxX/blob/5a08a334ef216eff98d2c7d9a9166f94c9ef70ed/third_party/victory/crates/victory-gpu/src/shader_cache.rs) ebenfalls made-/Direktrückgabe und Stale-Pfade; keine separate Messung dieser Übernahme. |

### PERF-0010 — Wake live shader worker during concurrent jobs

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [brcly/TuxBox](https://github.com/brcly/TuxBox) |
| Code commit(s) | [2f0473bd676422f7e4dd8ab5c2aded14a35eb8c8](https://github.com/brcly/TuxBox/commit/2f0473bd676422f7e4dd8ab5c2aded14a35eb8c8) |
| Evidence commit(s) | [2f0473bd676422f7e4dd8ab5c2aded14a35eb8c8](https://github.com/brcly/TuxBox/commit/2f0473bd676422f7e4dd8ab5c2aded14a35eb8c8) |
| Merged PR | [#283](https://github.com/brcly/TuxBox/pull/283) |
| Subsystem / Mechanism | Ein inotify-Poll wartet zugleich auf neue Anfragen und eine Pipe für fertige Worker-Jobs. Ein laufender Compile verhindert nicht mehr die sofortige Bearbeitung neuer Requests; ohne inotify bleibt begrenzter Rescan. |
| Metric; Before; After; Delta | Median der Reply-Latenz über je drei Runs: 50,4–50,8 → 1,7–2,4 ms (Bereichsdifferenz 48,0–49,1 ms; mindestens 95,2 %). LOAD_TIME_S bezeichnet hier isolierte Host-Request-Latenz, nicht Spielstart. |
| Test conditions | 30 aufgezeichnete kurze ProjectScarlett-Requests neben einem langen Compute-Compile, vier Worker, reales xshader/DXC/dxv, drei Runs je Zustand; keine Guest-/GPU-Arbeit, CPU/GPU unknown. |
| Evidence | AUTHOR_MEASURED: Code-Diff und Messung im selben Commit, neuer Regressionstest; 381 Python-Tests und No-GPU-Check laut Quelle bestanden. |
| Trade-offs | Ohne parallelen Job 2,3–2,9 ms in beiden Zuständen, kein Gewinn. Keine volle GPU-Prüfung, Linux-inotify-Pfad; kein Gameplay-FPS-Nachweis. |
| Integration | Benötigt den asynchronen dateibasierten Host-Worker und Benachrichtigung für neue Requests und Fertigstellungen; anderer Mechanismus als Guest-Reply-Polling PERF-0002. |
| Baseline integration | BASE-0001, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57); `INCOMPATIBLE`: [crates/victory-gpu/src/pipeline/make.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/pipeline/make.rs) nutzt lokale Condvar-Signale/wait_timeout; kein gleichartiger TuxBox-Datei-IPC-Pfad nachgewiesen. Nur bei entsprechender Provider-Architektur sinnvoll. Relevanz: `REFERENCE_ONLY`. **Integrationsreview 2026-11-27:** Gemessener Python-/inotify-/Pipe-Worker ist Linux-/Datei-IPC-spezifisch; Baseline hat bereits ereignisbasierte native Rust-Condvar-Worker für neue Aufgaben und Abschlüsse. Kein entsprechender Zielpfad. |
| Provenance / License | NickStallman PR #283; [Apache-2.0-Lizenz](https://github.com/brcly/TuxBox/blob/2f0473bd676422f7e4dd8ab5c2aded14a35eb8c8/LICENSE); keine Aussage zu Drittkomponenten. |
| Adoptions | none recorded |

### PERF-0011 — Wine placeholder reservation tracking

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [DoubleGhosty/cool_fox](https://github.com/DoubleGhosty/cool_fox) |
| Code commit(s) | [f9d2da84941ef3204aeed5f0f097efb8bee3ece0](https://github.com/DoubleGhosty/cool_fox/commit/f9d2da84941ef3204aeed5f0f097efb8bee3ece0) |
| Evidence commit(s) | [f9d2da84941ef3204aeed5f0f097efb8bee3ece0](https://github.com/DoubleGhosty/cool_fox/commit/f9d2da84941ef3204aeed5f0f097efb8bee3ece0) |
| Merged PR | n/a |
| Subsystem / Mechanism | Wenn Wine MemoryRegionInformationEx nicht unterstützt, beantwortet ein vom Runtime-Allocator gepflegter BTreeMap-Index Placeholder-Fragen; Reservierung und Freigabe aktualisieren ihn. Wiederholte VirtualQuery-Abfragen unter Wine-Lock entfallen. |
| Metric; Before; After; Delta | Guard-Zeit beim Recording pro Frame: 362 → 38 ms (−324 ms; −89,5 %). Hole-Abfragen pro Frame: 10.210 → 17; erste Welt-Cinematic ~3 → ~5 FPS laut Quelle (gerundete Begleitwerte). |
| Test conditions | Wine, Cold-Cache-Run, erste Welt-Cinematic; GPU, CPU, Profil, Auflösung, Wiederholungen und exakte Bildqualität unknown. Gleiche dokumentierte Abfrage-/Recording-Metrik; kein nativer Windows-Test. |
| Evidence | AUTHOR_MEASURED: Vergleich und tatsächliche Watch-/XMem-Änderung in f9d2da8. |
| Trade-offs | Keine Langzeit- oder Bildparitätsmessung. Vollständiges Reserve/Free-/Mapping-Tracking nötig; partielles Freigeben verwirft konservativ Einträge. Native Windows-Abfrage bleibt unverändert. |
| Integration | Watch-/XMem-Reserve-/Freigabe-Verträge und Alias-Lebensdauer gemeinsam prüfen. |
| Baseline integration | BASE-0001, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57); `INCOMPATIBLE`: [crates/victory-gpu/src/watch.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/watch.rs) kennt Placeholder über NtQueryVirtualMemory Klasse 7, gibt bei Fehlschlag false zurück; kein Tracking-Fallback. Baseline ist native Windows-x64; Zusatznutzen dort nicht nachgewiesen, relevant für Wine. Relevanz: `REFERENCE_ONLY`. **Integrationsreview 2026-11-27:** Wine-spezifische fehlende MemoryRegionInformationEx-Abfrage; [native Baseline](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/watch.rs#L2036) verwendet NtQueryVirtualMemory Klasse 7. Technisch verwandter Code, aber gemessener Problemmechanismus nicht im nativen Windows-Zielpfad belegt. Wine-Portierung ist außerhalb des aktuellen Zielumfangs. |
| Provenance / License | Belegter Quellcommit f9d2da8; [GPL-3.0-or-later Workspace](https://github.com/DoubleGhosty/cool_fox/blob/f9d2da84941ef3204aeed5f0f097efb8bee3ece0/Cargo.toml); weitere Vorarbeiten nicht verifiziert. |
| Adoptions | none recorded |

### PERF-0012 — Reserved VRAM room for large upscaler buffers

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [ZigZagBoi1/ScarlettPort](https://github.com/ZigZagBoi1/ScarlettPort) |
| Code commit(s) | [f059bbf03963e8e407e4ca0d0d579239c89d26aa](https://github.com/ZigZagBoi1/ScarlettPort/commit/f059bbf03963e8e407e4ca0d0d579239c89d26aa) |
| Evidence commit(s) | [f059bbf03963e8e407e4ca0d0d579239c89d26aa](https://github.com/ZigZagBoi1/ScarlettPort/commit/f059bbf03963e8e407e4ca0d0d579239c89d26aa); [versionierter README-Messabschnitt](https://github.com/ZigZagBoi1/ScarlettPort/blob/f059bbf03963e8e407e4ca0d0d579239c89d26aa/README.md#-new-in-v050) |
| Merged PR | n/a |
| Subsystem / Mechanism | SCARLETT_CARD_BIG_ROOM reserviert zusätzliche 256 MiB oberhalb des normalen Copy-Caps ausschließlich für Puffer ab 16 MiB. Ein 24-MB-Netzwerktensor des Upscalers passt in lokalen GPU-Speicher statt PCIe-Zugriff; normale Budgetgrenze bleibt. |
| Metric; Before; After; Delta | GPU-Zeit pro Frame für Pass 8cbd7f5b: 55,7–70,8 → 0,10–0,28 ms; konservative Bereichsdifferenz 55,42–70,70 ms (mindestens 99,5 %). Weitere Pässe f4611739/f7fc6c98 laut Quelle unter 0,1 ms statt 43,7–50,0/34,7–40,1 ms. |
| Test conditions | RTX 3060 12 GB, Ryzen 5 5600X, 32 GB RAM, Series S, gleiche geskriptete Route, 180 s Welt, jeweils ein Boot; 0.4.x gegen 0.5.0 Defaults, exakte Auflösung unknown. |
| Evidence | AUTHOR_MEASURED: versionierte Tabelle und Code-Diff; derselbe namentlich identifizierte Pass, keine unabhängige Reproduktion. |
| Trade-offs | Gesamtänderung umfasst auch Save- und Shader-Patches; 1,6 → 3,4 Welt-FPS und 244 → 178 s Boot sind nicht als isolierter Gewinn des VRAM-Mechanismus zu werten. P95-Frame verschlechtert 4.250 → 4.464 ms; Cache-Neubildung, Reserve verdrängt ggf. andere Kopien. Bildparität nicht separat gemessen. |
| Integration | Zusätzliche Größenklasse und Budget-/Copy-Lebensdauer mit bestehender VRAM-Admission abstimmen; keine pauschale Reserve ohne Profilierung. |
| Baseline integration | BASE-0001, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57); `ADAPTATION_REQUIRED`: [crates/victory-gpu/src/shadow.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/shadow.rs) besitzt card_limits/card_room_for, globale Buffer-Reserve und Budgetprüfung; keine exklusive Zusatzreserve für Puffer ab 16 MiB. Bereits vorhandene Budgetregeln und gegebenenfalls größere vorhandene Cap-Einstellungen berücksichtigen. Relevanz: `CONDITIONAL`. **Integrationsreview 2026-11-27:** [Baseline card_limits/card_room_kept/card_room_for](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/shadow.rs#L217) besitzt GPU-skalierte Caps, standardmäßige globale Buffer-Reserve bis 1 GiB und Admission bis 97% bei Reserve; mögliche Zusatzreserve ab 16 MiB nur bei belegtem Cap-Engpass. Quellvergleich mehrerer gleichzeitiger 0.5.0-Defaults isoliert den Effekt der Reserve nicht. Kein zusätzlicher Pass-/FPS-Gewinn auf der Baseline nachgewiesen. |
| Provenance / License | Integrierter ScarlettPort-Codecommit f059bbf; [UPSTREAM.md](https://github.com/ZigZagBoi1/ScarlettPort/blob/f059bbf03963e8e407e4ca0d0d579239c89d26aa/engine/UPSTREAM.md) nennt GPL-3.0-or-later und VIctory/fxceless-Abstammung; einzelne Drittkomponenten prüfen. [Engine-Workspace am Code-Commit](https://github.com/ZigZagBoi1/ScarlettPort/blob/f059bbf03963e8e407e4ca0d0d579239c89d26aa/engine/Cargo.toml) GPL-3.0-or-later; XCL-Teil außerhalb des erlaubten Umfangs. |
| Adoptions | none recorded |

### PERF-0013 — Vendor-specific small texture pool alignment

| Feld | Inhalt |
| --- | --- |
| Status | `VERIFIED_AUTHOR` |
| Origin repository | [TaynanGT/Vice-Tay](https://github.com/TaynanGT/Vice-Tay) |
| Code commit(s) | [7d62e536e09234289a77474db3f830a44c7e01e3](https://github.com/TaynanGT/Vice-Tay/commit/7d62e536e09234289a77474db3f830a44c7e01e3); neuer In-Scope-Import [7e3e672acbcbf7806496de3aa5a0c0b332631330](https://github.com/LooperSalty/AnyXboxX/commit/7e3e672acbcbf7806496de3aa5a0c0b332631330) |
| Evidence commit(s) | [7d62e536e09234289a77474db3f830a44c7e01e3](https://github.com/TaynanGT/Vice-Tay/commit/7d62e536e09234289a77474db3f830a44c7e01e3); [importierter Messbericht](https://github.com/LooperSalty/AnyXboxX/blob/7e3e672acbcbf7806496de3aa5a0c0b332631330/third_party/victory/docs/findings-rtx4090.md) |
| Merged PR | [Vice-Tay #45](https://github.com/TaynanGT/Vice-Tay/pull/45); [AnyXboxX #454](https://github.com/LooperSalty/AnyXboxX/pull/454) |
| Subsystem / Mechanism | Kleine gepoolte Texturen (bis 64 KiB) werden auf geeigneten Nicht-AMD-Karten an 4-KiB- statt 64-KiB-Grenzen platziert; AMD behält die konservative 64-KiB-Strategie. Verwendet die bereits vorhandene Opt-in-Mechanik. |
| Metric; Before; After; Delta | Max. Card Memory 16,7 → 13,3 GiB (−3,4 GiB; −20,4 %). Mediane private RAM-Belegung 32,3 → 29,1 GiB (−3,2 GiB; −9,9 %). Quelle schreibt GB; versioniertes [Panel](https://github.com/TaynanGT/Vice-Tay/blob/a2523483dc8f5f7378d1f41160d914648fbab649/crates/victory-gpu/src/host/panel.rs) rechnet Bytes durch 2^30; keine höhere Genauigkeit behauptet. |
| Test conditions | RTX 4090, Ryzen 9 9950X3D, Windows, Series S, 30 min, gleicher Testkontext laut findings-rtx4090.md; exakte Auflösung unknown. Je ein Lauf; FPS-Mediane 22,7/22,1, 1%-Low 17,0/18,0 laut PR. |
| Evidence | AUTHOR_MEASURED: Originalcommit enthält absolute Werte und Code-Diff; Import-Code am neuen Default-Branch-HEAD bestätigt. |
| Trade-offs | Kein FPS-Gewinn; beide Runs ohne Device Loss. Zuvor zwei Device Losses bei kleinen Placements auf RX 7900 XTX; AMD-Ausnahme ist entscheidend, keine Übertragung auf AMD erzwingen. |
| Integration | D3D12-Small-Resource-Placement-Support und herstellerspezifische Stabilität prüfen; keine Änderung von Texturauflösung oder Mips. |
| Baseline integration | BASE-0001, [2de998014dc85ad68cd6422a250400c7084ace57](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57); `ALREADY_PRESENT`: [crates/victory-gpu/src/pool.rs](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/pool.rs) besitzt VICTORY_POOL_SMALL=1 für dieselbe Platzierung; standardmäßig aus. Automatische Herstellerwahl ist eine Anpassung, aber der gemessene Mechanismus ist bereits per Schalter testbar; auf AMD bewusst nicht einschalten. Relevanz: `REFERENCE_ONLY`. **Integrationsreview 2026-11-27:** D3D12-Small-Resource-Placement bereits per VICTORY_POOL_SMALL testbar; Herstellerautomatik ist Konfiguration, kein neuer gemessener Mechanismus. AMD-Device-Loss-Risiko respektieren; keine pauschale Aktivierung. |
| Provenance / License | Vice-Tay Originalcommit 7d62e53; [GPL-3.0-or-later Workspace](https://github.com/TaynanGT/Vice-Tay/blob/a2523483dc8f5f7378d1f41160d914648fbab649/Cargo.toml); In-Scope-Übernahme durch AnyXboxX, keine eigene neue Messung. [Workspace am Originalcommit](https://github.com/TaynanGT/Vice-Tay/blob/7d62e536e09234289a77474db3f830a44c7e01e3/Cargo.toml) GPL-3.0-or-later; konkrete Herkunft/Device-Loss-Belege bleiben maßgeblich. |
| Adoptions | [AnyXboxX Pool-Code am Snapshot](https://github.com/LooperSalty/AnyXboxX/blob/5a08a334ef216eff98d2c7d9a9166f94c9ef70ed/third_party/victory/crates/victory-gpu/src/pool.rs) bestätigt den Hersteller-Default. |

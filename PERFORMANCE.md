# PERFORMANCE.md — Abgeleitete Performance-Übersicht

**Nur Darstellung.** Quelle jeder Tabellenzeile ist ein vorhandener Datensatz in [`FINDINGS.md`](FINDINGS.md). Diese Datei darf keine neuen Messwerte oder unreferenzierten Spitzenreiter erfinden.

## Ranglistenvertrag

| Feld | Semantik |
| --- | --- |
| Rank | Rang **innerhalb** ausreichend vergleichbarer Hardware-/Szenen-/Qualitätsgruppe |
| Finding | Verlinkte `PERF-XXXX`-ID |
| Before → After | Werte wie im kanonischen Fund |
| Gain | Korrekt umgerechnete absolute und ggf. relative Verbesserung |
| Context | GPU, Szene/Profil, Auflösung und Bildqualität |
| Evidence | `AUTHOR_MEASURED` oder `INDEPENDENT_REPRODUCED` |

**Keine gruppenübergreifende Gesamtkrone.** Entwickler-Messungen nicht als unabhängig reproduziert ausgeben. Abweichende Auflösung/GPU/Profil oder aktivierte Frame Generation erfordern getrennte Gruppen oder eine ungerankte Tabelle.

## FPS / Frametime — vergleichbare Gruppen

| Rank | Finding | Before → After | Gain | Context | Evidence |
| --- | --- | --- | --- | --- | --- |
| 1 (RX 7900 XTX / Open World) | [PERF-0001](FINDINGS.md#perf-0001--descriptor-hot-page-fallback) | 82 ms → 72 ms | −10 ms mediane Frametime (−12,2 %) | Windows 11, RX 7900 XTX 24 GB, Ryzen 7 5800X3D; Open World/Stadt; exakte Auflösung unbekannt | `AUTHOR_MEASURED` |
| 1 (RTX 5090 / synthetischer Indirect-Draw-Benchmark) | [PERF-0003](FINDINGS.md#perf-0003--single-record-indirect-clamp) | 26,532 ms → 21,392 ms | −5,140 ms GPU-Zeit pro synthetischem Frame (−19,4 %) | RTX 5090, Proton 11, 3.200 Dreiecks-Draws, GPU-lokales Budget; keine Gameplay-Frametime | `AUTHOR_MEASURED` |
| — | [PERF-0004](FINDINGS.md#perf-0004--three-copy-and-refresh-switches) | 12,7 FPS → 14,8 FPS | +2,1 FPS (+16,5 %) | RTX 4070 SUPER, i7-13700KF, Windows 11, Series X; ein In-Session-A/B, Auflösung unbekannt | `AUTHOR_MEASURED` |
| — | [PERF-0005](FINDINGS.md#perf-0005--reduced-descriptor-heap-scan-cadence) | 14,2 FPS → 15,2 FPS | +1,0 FPS (+7,0 %) | RTX 4070 SUPER, Windows 11, Series X; separater In-Session-A/B, Auflösung unbekannt | `AUTHOR_MEASURED` |
| — | [PERF-0006](FINDINGS.md#perf-0006--asynchronous-copy-back-relay) | 274 ms → 126 ms | −148 ms (−54,0 %) | RTX 3080 12 GB, Ryzen 7 7800X3D; Weltphase 170–330 s, übliche Frametime; Mechanismus bereits in Baseline | `AUTHOR_MEASURED` |
| — | [PERF-0007](FINDINGS.md#perf-0007--rt-submission-ordering-and-forwarded-fills) | 131,1 ms → 118,7 ms | −12,4 ms (−9,5 %) | Series X 4K/1440p, RT an, Stadt gehend; RX 7900 XTX laut Projekt-Testsystem; keine Baseline-Messung | `AUTHOR_MEASURED` |
| — | [PERF-0008](FINDINGS.md#perf-0008--lean-indirect-spread-dispatch) | 225 ms → 208 ms | −17 ms (−7,6 %) | RTX 3060 12 GB, stehend; neun 20-s-A/B-Paare; Auflösung unknown | `AUTHOR_MEASURED` |
| — | [PERF-0011](FINDINGS.md#perf-0011--wine-placeholder-reservation-tracking) | 362 ms → 38 ms | −324 ms (−89,5 %) | Wine, Cold-Cache, Recording-Guard-Zeit je Frame; Hardware unknown | `AUTHOR_MEASURED` |
| — | [PERF-0012](FINDINGS.md#perf-0012--reserved-vram-room-for-large-upscaler-buffers) | 55,7–70,8 ms → 0,10–0,28 ms | −55,42 bis −70,70 ms pro Pass (Bereichsdifferenz) | RTX 3060 12 GB, Series S, identische Skriptroute, Upscaler-Pass 8cbd7f5b; ein Boot je Zustand | `AUTHOR_MEASURED` |

## RAM / VRAM — gemessene Einsparungen

| Rank | Finding | Before → After | Gain | Context | Evidence |
| --- | --- | --- | --- | --- | --- |
| — | [PERF-0013](FINDINGS.md#perf-0013--vendor-specific-small-texture-pool-alignment) | 16,7 GiB → 13,3 GiB | −3,4 GiB (−20,4 %) | RTX 4090, Series S, 30 min, 64-KiB vs 4-KiB-Placement; VRAM-Maximum | `AUTHOR_MEASURED` |

## Stabilität / Ladeverhalten — quantifizierte Verbesserungen

| Rank | Finding | Before → After | Gain | Context | Evidence |
| --- | --- | --- | --- | --- | --- |
| 1 (RTX 4070 Ti SUPER / Startzeit) | [PERF-0002](FINDINGS.md#perf-0002--shader-reply-polling) | 32 s → 26 s | −6 s bis stabile Frames (−18,75 %) | TuxBox/ProjectScarlett, keine gespeicherten Übersetzungen/DXC-Ergebnisse, warme Treiber-Caches; ein Boot je Zustand | `AUTHOR_MEASURED` |
| — | [PERF-0009](FINDINGS.md#perf-0009--retain-translated-shader-on-cache-miss) | 0,0002975 s → 0,0002209 s | −0,0000766 s (−25,7 %) | Synthetische Cache-Miss-Latenz je 64-KiB-Lookup, Wine/ext4, i9-14900KS; keine Spiel-Ladezeit | `AUTHOR_MEASURED` |
| — | [PERF-0010](FINDINGS.md#perf-0010--wake-live-shader-worker-during-concurrent-jobs) | 0,0504–0,0508 s → 0,0017–0,0024 s | −0,0480 bis −0,0491 s (konservative Bereichsdifferenz) | TuxBox Host-Worker, 30 Shader-Anfragen neben langem Compile; kein Spielstarttest | `AUTHOR_MEASURED` |

## Technischer Integrationsanker

Die verbindliche technische Ausgangsbasis und ihre versionierten Leistungsbelege stehen ausschließlich in [BASELINE.md](BASELINE.md). Sie ist kein aus relativen Finding-Gewinnen abgeleiteter Gesamtsieger. Die obenstehenden Tabellen bleiben ausschließlich aus den kanonischen Findings abgeleitet; eine globale Rangfolge ist ohne vergleichbare Gesamttests nicht belegt.

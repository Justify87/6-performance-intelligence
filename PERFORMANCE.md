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

## RAM / VRAM — gemessene Einsparungen

| Rank | Finding | Before → After | Gain | Context | Evidence |
| --- | --- | --- | --- | --- | --- |
| — | Noch kein qualifizierter Fund | — | — | — | — |

## Stabilität / Ladeverhalten — quantifizierte Verbesserungen

| Rank | Finding | Before → After | Gain | Context | Evidence |
| --- | --- | --- | --- | --- | --- |
| 1 (RTX 4070 Ti SUPER / Startzeit) | [PERF-0002](FINDINGS.md#perf-0002--shader-reply-polling) | 32 s → 26 s | −6 s bis stabile Frames (−18,75 %) | TuxBox/ProjectScarlett, keine gespeicherten Übersetzungen/DXC-Ergebnisse, warme Treiber-Caches; ein Boot je Zustand | `AUTHOR_MEASURED` |

## Technischer Integrationsanker

Die verbindliche technische Ausgangsbasis und ihre versionierten Leistungsbelege stehen ausschließlich in [BASELINE.md](BASELINE.md). Sie ist kein aus relativen Finding-Gewinnen abgeleiteter Gesamtsieger. Die obenstehenden Tabellen bleiben ausschließlich aus den kanonischen Findings abgeleitet; eine globale Rangfolge ist ohne vergleichbare Gesamttests nicht belegt.

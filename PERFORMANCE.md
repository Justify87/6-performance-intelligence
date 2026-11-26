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

## RAM / VRAM — gemessene Einsparungen

| Rank | Finding | Before → After | Gain | Context | Evidence |
| --- | --- | --- | --- | --- | --- |
| — | Noch kein qualifizierter Fund | — | — | — | — |

## Stabilität / Ladeverhalten — quantifizierte Verbesserungen

| Rank | Finding | Before → After | Gain | Context | Evidence |
| --- | --- | --- | --- | --- | --- |
| — | Noch kein qualifizierter Fund | — | — | — | — |

## Referenzportierungen

| Vergleichsgruppe | Führender belegter Kandidat | Mess- und Szenenbeleg | Einschränkung |
| --- | --- | --- | --- |
| Noch keine vergleichbare Gruppe | **Kein Sieger bestimmt** | Keine qualifizierten Funde seit dem Bootstrap | Einzelne README-Höchstwerte reichen nicht aus |

**Regel:** Ein Fund beschreibt eine **relative** Verbesserung. Daraus allein folgt nicht, dass die gesamte Portierung insgesamt die höchste FPS oder beste Stabilität bietet. Spitzenkandidaten nur mit direkt vergleichbaren Gesamttests nennen und auf den belegenden Fund verweisen; andernfalls offen lassen.

# WATCHLIST.md — Repository-Verzeichnis

**Autorität:** Diese Datei entscheidet **welche** Quellen geprüft werden; der tatsächlich zu lesende Branch ist **immer** der zur Laufzeit über GitHub abgefragte `default_branch`. Die zuletzt beobachteten Branch-Namen und SHA-Cursor stehen ausschließlich in `state/scan-state.json`.

## Feldvertrag

| Spalte | Werte / Semantik |
| --- | --- |
| Repository | Eindeutiges kanonisches `owner/name`, mit Link |
| Tier | `P1` = in jedem Stundenlauf vorrangig; `P2` = danach, ohne Commits zu überspringen |
| Status | `ACTIVE` = prüfen; `PAUSED` = nicht prüfen, Cursor behalten; `RETIRED` = nur historische Zuordnung |
| Fokus | Technischer Bereich, kein Erfolgsversprechen |

## Beobachtete Quellen

| Repository | Tier | Status | Fokus |
| --- | --- | --- | --- |
| [Marvisak/VIctory](https://github.com/Marvisak/VIctory) | P1 | ACTIVE | Upstream-GPU-/Runtime-Schicht |
| [avatarmanz/VIctory-7900XTX](https://github.com/avatarmanz/VIctory-7900XTX) | P1 | ACTIVE | AMD-Renderer, Frametime |
| [LooperSalty/AnyXboxX](https://github.com/LooperSalty/AnyXboxX) | P1 | ACTIVE | Integrierter Runtime-Stand, längere Stabilitätstests |
| [fxceless/victory-fork](https://github.com/fxceless/victory-fork) | P1 | ACTIVE | Renderer, Shader, CPU-/GPU-Speicher |
| [iLucasRT/VInAMD](https://github.com/iLucasRT/VInAMD) | P1 | ACTIVE | AMD-Tuning und Benchmark-Läufe |
| [xcl-dev/xcl](https://github.com/xcl-dev/xcl) | P2 | ACTIVE | Alternative Xbox-Series-Kompatibilität |
| [proneonn/Vertex](https://github.com/proneonn/Vertex) | P2 | ACTIVE | Rendering, Abstürze, GPU-Diagnostik |
| [Gamination/victory-4060](https://github.com/Gamination/victory-4060) | P2 | ACTIVE | Speicherverwaltung, begrenzter VRAM |
| [ubiquitarianism/leonida](https://github.com/ubiquitarianism/leonida) | P2 | ACTIVE | Alternativer Recompilation-Ansatz |

## Änderungsvertrag

Neue Projekte nur bei nachvollziehbar eigenständiger technischer Arbeit aufnehmen; reine Kopien/Mirrors ausschließen. Bei Aufnahme zugleich einen Bootstrap-Cursor für den **aktuellen Default-Branch-HEAD** in `state/scan-state.json` speichern. Bis zu diesem SHA wird **keine** rückwirkende Auswertung behauptet. Statuswechsel dürfen die vorhandene Cursor-Historie nicht löschen. Bei Umbenennung/Transfer GitHub-Identität prüfen und keine doppelte Repository-Zeile erzeugen.

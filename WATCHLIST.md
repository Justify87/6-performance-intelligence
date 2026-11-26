# WATCHLIST.md – beobachtete Quell-Repositories

Diese Liste entscheidet **welche** Repositories beobachtet werden. Der zu lesende Branch wird **bei jedem Lauf dynamisch aus `default_branch` der GitHub-Repository-Metadaten** bestimmt. Nur dessen Commits sind im Scope. Die untenstehenden Branch-Namen sind **keine Konfiguration**, sondern ausschließlich als initialer Befund in `state/scan-state.json` festgehalten.

| Priorität | Repository | Begründung |
| --- | --- | --- |
| Hoch | [Marvisak/VIctory](https://github.com/Marvisak/VIctory) | Ursprünglicher VIctory-Kompatibilitäts-Layer |
| Hoch | [avatarmanz/VIctory-7900XTX](https://github.com/avatarmanz/VIctory-7900XTX) | AMD-GPU-/Renderer-Performance; dokumentierte Benchmark-Vergleiche |
| Hoch | [LooperSalty/AnyXboxX](https://github.com/LooperSalty/AnyXboxX) | Integrierter VIctory-Fork mit längeren Laufzeit-Messungen |
| Hoch | [fxceless/victory-fork](https://github.com/fxceless/victory-fork) | Häufige technische Renderer-/Shader-/Speicher-Optimierungen |
| Hoch | [iLucasRT/VInAMD](https://github.com/iLucasRT/VInAMD) | AMD-spezifische Laufzeit-/Leistungsdaten |
| Mittel | [xcl-dev/xcl](https://github.com/xcl-dev/xcl) | Allgemeine Xbox-Series-Kompatibilitätsschicht |
| Mittel | [proneonn/Vertex](https://github.com/proneonn/Vertex) | Grafik- und Stabilitätsmessungen anderer Hardware |
| Mittel | [Gamination/victory-4060](https://github.com/Gamination/victory-4060) | VRAM-Verwaltung; **Standardbranch bei Einrichtung: `work`** |
| Mittel | [ubiquitarianism/leonida](https://github.com/ubiquitarianism/leonida) | Alternativer Portierungs-/Recompilation-Ansatz |

**Umfang:** Nur neue Commits nach dem je Repository gespeicherten Cursor auf dem aktuellen Standardbranch. Keine Scans anderer Branches oder offener PRs. Bereits gemergte PRs dürfen als Quelle eines im Standardbranch enthaltenen Commits verlinkt werden.

**Erweiterung:** Weitere Repositories nur ergänzen, wenn sie eigenständige relevante Entwicklungen zeigen. Bloße Mirrors/Kopien erhalten keine eigene Priorität. Für ein neu aufgenommenes Repository einen Bootstrap-SHA auf dessen aktuellem Standardbranch erfassen; alte Historie nur nach ausdrücklichem separatem Auftrag prüfen.

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
| [R0Y00/Xbox2Windows](https://github.com/R0Y00/Xbox2Windows) | P1 | ACTIVE | Statische Recompilation, Upload-Ring, Shader-Lowering |
| [Belobug/VIctory-Belobug-AMD](https://github.com/Belobug/VIctory-Belobug-AMD) | P1 | ACTIVE | AMD-Renderer, Textur-Uploads, FSR |
| [DexNom/VIctory](https://github.com/DexNom/VIctory) | P1 | ACTIVE | RayTime, Radeon-Renderer, FSR und Raytracing |
| [DoubleGhosty/cool_fox](https://github.com/DoubleGhosty/cool_fox) | P1 | ACTIVE | GPU-Residency, Zero-Copy-Buffer, CBV und Wine |
| [ZigZagBoi1/ScarlettPort](https://github.com/ZigZagBoi1/ScarlettPort) | P1 | ACTIVE | Shader-Warm-up, PSO-Caching, Renderer-Integration |
| [myokex/victory-amd](https://github.com/myokex/victory-amd) | P1 | ACTIVE | GPU-Write-Watch, Deskriptoren und Texturverwaltung |
| [uiliannn/VItoriaBr](https://github.com/uiliannn/VItoriaBr) | P1 | ACTIVE | D3D12-Renderer, Deskriptor-Heap, UAV-Writeback |
| [stellarr-studio/kelvaroq](https://github.com/stellarr-studio/kelvaroq) | P1 | ACTIVE | Runtime-Patches, GPU-Speicherverwaltung und DXR |
| [PastiiJay/klbf-lab](https://github.com/PastiiJay/klbf-lab) | P1 | ACTIVE | AMD-Renderer, Pipeline-Prebuild, Benchmark-Methodik |
| [DesertGamer/VIPort](https://github.com/DesertGamer/VIPort) | P1 | ACTIVE | Lazy-Texturallokation, Shader-Hardwaresemantik |
| [ka1qux/BRVIctory-ka1qux](https://github.com/ka1qux/BRVIctory-ka1qux) | P1 | ACTIVE | AMD-GPU, Depth-Bounds und Renderer-Regressionstests |
| [californiaAlmonds/OpenScarlett](https://github.com/californiaAlmonds/OpenScarlett) | P1 | ACTIVE | D3D12/Vulkan, VRAM-Management, Mesh-Shader |
| [ThatRecompDev/five-star-recomp](https://github.com/ThatRecompDev/five-star-recomp) | P1 | ACTIVE | Recompilation, D3D12X-Shims, Raytracing und GPU-Stabilität |
| [ProjectLeonidaL/leonida](https://github.com/ProjectLeonidaL/leonida) | P2 | ACTIVE | Statische Recompilation, Loader und Konsoleninstruktionen |
| [brcly/TuxBox](https://github.com/brcly/TuxBox) | P2 | ACTIVE | Linux/Windows, Vulkan, CBV-Kompatibilität |
| [sammwyy/xvalkyrie](https://github.com/sammwyy/xvalkyrie) | P2 | ACTIVE | Xbox-HLE, RDNA-Shader-Übersetzung |
| [pragyaangaur/VIctory-macOS](https://github.com/pragyaangaur/VIctory-macOS) | P2 | ACTIVE | Apple Silicon, Wine/D3DMetal, Shader-Waves und GPU-Puffer |
| [ranveersoni98/Xenora](https://github.com/ranveersoni98/Xenora) | P2 | ACTIVE | ARM64-Recompilation, Metal und Loader-Runtime |
| [zortos293/XSEmu](https://github.com/zortos293/XSEmu) | P2 | ACTIVE | XCL-basierter Renderer, DXIL und GPU-Heap-Kompatibilität |
| [PlutonusDev/OpenEra](https://github.com/PlutonusDev/OpenEra) | P2 | ACTIVE | GameCore/Era-Runtime, Kernel-Shims und Speicherverwaltung |

## Änderungsvertrag

Neue Projekte nur bei nachvollziehbar eigenständiger technischer Arbeit aufnehmen; reine Kopien/Mirrors ausschließen. Bei Aufnahme zugleich einen Bootstrap-Cursor für den **aktuellen Default-Branch-HEAD** in `state/scan-state.json` speichern. Bis zu diesem SHA wird **keine** rückwirkende Auswertung behauptet. Statuswechsel dürfen die vorhandene Cursor-Historie nicht löschen. Bei Umbenennung/Transfer GitHub-Identität prüfen und keine doppelte Repository-Zeile erzeugen.

# BASELINE.md — Technische Integrationsbasis

**Autorität:** Diese Datei definiert den festen technischen Ausgangspunkt für ein späteres, separates Integrationsprojekt. Sie ist weder Scan-Cursor noch globale FPS-Rangliste. Auswahl und Aktualisierung erfolgen ausschließlich gemäß `AGENTS.md`.

## Aktiver Anker

| Feld | Wert |
| --- | --- |
| Baseline ID | `BASE-0001` |
| Status | `PROVISIONAL` |
| Repository | [avatarmanz/VIctory-7900XTX](https://github.com/avatarmanz/VIctory-7900XTX) |
| Default branch bei Auswahl | `main` |
| Reference SHA | [`2de998014dc85ad68cd6422a250400c7084ace57`](https://github.com/avatarmanz/VIctory-7900XTX/commit/2de998014dc85ad68cd6422a250400c7084ace57) |
| Code-Stand | [340619293e1822be7e1ade7df128309dbd178d44](https://github.com/avatarmanz/VIctory-7900XTX/commit/340619293e1822be7e1ade7df128309dbd178d44); bis Reference SHA nur README-Änderungen laut Compare |
| Evidenz | [README am Reference SHA](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/README.md#where-it-stands-2026-11-26); `AUTHOR_MEASURED` |
| Architektur | Windows x64, Rust-Workspace, D3D12/RDNA-Shader-Übersetzung; gemeinsame VIctory-Codefamilie |
| Lizenz | [GPL-3.0-or-later laut Workspace](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/Cargo.toml); Drittkomponenten vor Integration einzeln prüfen |
| Auswahlstand | 2026-11-26; Watchlist 29 Quellen |
| Freigabe zur Implementierung | Keine; ausschließlich Recherche |

Der Reference SHA fixiert den ausgewählten Quellstand und enthält die Messdokumentation. Er ist **kein unabhängig verifizierter Benchmark-Build-SHA**; die README benennt nicht für alle Messreihen den exakten Build-Hash. Der Compare vom Code-Stand 3406192 bis zum Anker zeigt ausschließlich Änderungen an README.md. Der Anker bleibt auch bei neuen Commits des Baseline-Repositories unverändert.

## Technologischer Kompatibilitäts- und Integrationsvertrag

**Zielarchitektur:** native Windows-x64-Ausführung des Gast-x86-64-Codes mit bestehendem VIctory-Loader, HLE, D3D12-Renderer, RDNA-Shaderübersetzung, GPU-/CPU-Speichermodell, Formaten und Pipeline-/Cache-System am Reference SHA. AMD ist die Referenzmesshardware, keine Bindung der Integrationsbasis an einen GPU-Hersteller. NVIDIA-/Intel-spezifische Optimierungen sind zulässig, wenn sie als bedingter Pfad in der bestehenden Architektur laufen und andere unterstützte Pfade nicht beschädigen. Wine-/Linux-/macOS-spezifische Effekte ohne belegte Wirkung oder Mechanismus im nativen Windows-Zielpfad sind außerhalb des Integrationsziels; dort gemessene plattformunabhängige Mechanismen können mit entsprechendem Semantiknachweis relevant bleiben.

**Definition:** Eine Optimierung ist technologisch kompatibel, wenn ihre konkrete Ursache, Ausführungssemantik, Schnittstellen, Abhängigkeiten und technischen Voraussetzungen mit mindestens einem bestehenden Subsystem am fixierten Reference SHA nachweislich vereinbar sind oder durch begrenzte Anpassungen vereinbar gemacht werden können. Dabei bleiben grundlegende Zielplattform und Architektur sowie die funktionalen, qualitativen, Sicherheits- und Synchronisationsverträge der Baseline erhalten. Ein Quell-Repository ist als Quelle geeignet, wenn es mindestens einen solchen konkret belegten Teilbereich mit grundsätzlich zulässiger Übernahmelizenz besitzt. Die Quelleneinstufung beweist nicht die Kompatibilität aller Commits.

| Subsystem | Konkreter Anschlussmaßstab |
| --- | --- |
| CPU / Loader / HLE | Native x86-64-/PE- und Win32-Semantik; passende ABI, TLS, Import-/Thread-/Speicherverträge. Keine neue CPU-Emulation oder AOT-Runtime voraussetzen. |
| GPU / Renderer | D3D12- bzw. Xbox-D3D12-Übersetzung mit gleichwertigen Command-, Descriptor-, Queue-, Barrier-, Fence-, Residency- und Lebensdauerverträgen. Andere Backends nur bei konkret isolierbarem, gleichwertigem Teilmechanismus. |
| Shader / Indirect | RDNA-Gastsemantik, HLSL/DXIL oder nachweislich äquivalente isolierte Transformation, identische ABI, Layouts, Wave-/Gruppen- und Budgetsemantik. |
| Formate / I/O | Gleiche Input-/Output-Verträge, z. B. BCPack-BCn-Dekodierung oder RDNA2-Layout, unveränderte verlustfreie bzw. funktional gleiche Ausgabe; keine bloß ähnlich bezeichnete Kompression. |
| Cache / Worker / Speicher | Gleiche Funktion und Lebensdauer, gültige Schlüssel-/Invalidierungs-, Publikations-, Allokations-, Fehler- und Synchronisationsverträge. Keine Einführung einer nicht benötigten fremden Infrastruktur. |

Zulässig sind lokale Patches, Anpassung vorhandener Schnittstellen, begrenzte Hilfsmodule und abgestimmte Änderungen verwandter Komponenten, auch eine begrenzte Portierung aus einer anderen Programmiersprache. Unzulässig als regulärer Integrationskandidat sind der Austausch oder grundlegende Neubau von Renderer, Shader-Recompiler, Loader/Runtime oder Speichermodell, ein Plattformwechsel oder eine umfangreiche Nachimplementierung fremder Architektur. Kein willkürliches Zeilen-/Zeitlimit: Nachweis anhand der tatsächlich betroffenen Schnittstellen und Abhängigkeiten. Gemeinsame Sprache, API, Codefamilie oder abstrakte Idee allein sind kein Nachweis.

**Lizenz separat:** Technologische Kompatibilität ist keine Lizenzfreigabe. Für die beabsichtigte Übernahme muss die am betroffenen Code geltende Lizenz mit GPL-3.0 vereinbar sein und die Herkunft nachvollziehbar sein. GPL-2.0-only ist für eine Codekombination unter GPL-3.0 ohne zusätzliche kompatible Erlaubnis gesperrt; separat lizenzierte Teilbereiche sind separat zu prüfen. Deklarierte GPL-3.0(-or-later), MIT oder Apache-2.0 können grundsätzlich anschlussfähig sein, unter Beachtung ihrer Bedingungen und Drittkomponenten. Keine proprietären Spiel-/SDK-Daten, keine unbelegte Rechteübernahme; keine Vermutung einer zulässigen eigenständigen Neuimplementierung als Freigabe. Vollständige Einzelpatch-Lizenzklärung vor tatsächlicher Integration bleibt Pflicht.

Technische Passung ist ferner von empirischer Performance-Evidenz, tatsächlicher Neuartigkeit und Zusatznutzen zu trennen. Bereits vorhandene äquivalente Mechanismen sind kein zusätzlicher Gewinn; bei Paketmessungen und abweichenden Hardware-/Plattformbedingungen keine isolierte oder kumulierte Baseline-Wirkung behaupten. Die Klassifikation der Quellen steht nur in WATCHLIST, die konkreter Findings nur in FINDINGS; die Ablauf- und Wechselregeln stehen in AGENTS.

## Dokumentierte Leistung

Quelle: [versionierte README](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/README.md#where-it-stands-2026-11-26). Windows 11, i9-12900K, RX 7900 XTX, 64 GB RAM; je 270 s in einer Stadtszene mit Stehen, Gehen und Kameradrehung. Renderer-Patches 1, 2, 5, 6 und 7 aktiviert. Werte vom Projekt berichtet, nicht selbst reproduziert.

| Profil / Ausgabe | RT | Stehend | Gehend | RAM laut Quelle |
| --- | --- | --- | --- | --- |
| Series X, 1080p | Aus | 40 ms (~25 FPS) | ~43 ms (~23 FPS) | 42 GB |
| Series X, 4K (intern 1440p) | Aus | 49 ms (~20 FPS) | ~62 ms (~16 FPS) | 45 GB |
| Series X, 4K (intern 1440p) | An | 99 ms (~10 FPS) | ~110 ms (~9 FPS) | 45 GB |
| Series S | Aus | 43 ms (~23 FPS) | ~64 ms (~16 FPS) | 37 GB |

VRAM: früherer Messstand ca. 10,5 GB; **nicht** als exakter VRAM-Verbrauch der obigen vier Profile behaupten. Langzeitstabilität (30+ min) nicht nachgewiesen; dokumentierte Bildfehler, Textur-Streaming, Beleuchtungsflackern sowie Raytracing-Probleme. Keine universelle Bestleistung oder Kompatibilität mit einer bestimmten NVIDIA-Karte bewiesen.

## Kandidatensichtung

Einmalige gezielte Baseline-Recherche, **kein** inkrementeller Performance-Scan. README-Sichtung sämtlicher 29 Watchlist-Quellen an den gespeicherten Standardbranch-Snapshots; aktuelle Default-Branch-HEADs der vier stärksten/nah verwandten Kandidaten (avatarmanz, AnyXboxX, Gamination, Belobug) zusätzlich ermittelt. Andere Quellen nicht flächendeckend bis zum aktuellen HEAD oder in allen Benchmark-Dateien nachrecherchiert.

| Kandidat / Quelle | Dokumentierte Gameplay-Leistung | Architektur / Einschränkung |
| --- | --- | --- |
| [avatarmanz/VIctory-7900XTX](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/README.md) | ~23 FPS gehend, Series X 1080p RT aus; ~16 FPS gehend 4K RT aus | Direkte VIctory-Linie, zahlreiche Renderer-Patches; **gewählt** |
| [Gamination/victory-4060](https://github.com/Gamination/victory-4060/blob/30504d328cd53ea094be99cae707d2038f991c80/README.md) | 21,8 FPS im Prolog auf RTX 5070 mit Series S, intern 960×540; 6 FPS Series X voller Scale | Direkter Abkömmling; NVIDIA-Optimierungen relevant, aber reduzierte Qualität/andere Szene und Frame-Generation separat; jüngster Merge nicht mit den älteren Messungen isoliert validiert |
| [LooperSalty/AnyXboxX](https://github.com/LooperSalty/AnyXboxX/blob/74dfb48b9538aa8a433cd4ae24066909fe6531d2/README.md) | ~16–17 FPS median, Series X, RTX 4070 SUPER; 28–30 GB RAM | VIctory vendored, integrierter Launcher und NVIDIA-Nachweise; andere Hardware, weniger RAM; kein Gleichheitsvergleich |
| [iLucasRT/VInAMD](https://github.com/iLucasRT/VInAMD/blob/5101a92f1fb5fce0e2973299c37044ddbb0e86ab/README.md) | 72 ms (~14 FPS) Stadt/Open World, RX 7900 XTX | Nachweislich gemeinsame Basis c9c932c; eigener 5800X3D; gezielte Optimierungsquelle |
| [Belobug/VIctory-Belobug-AMD](https://github.com/Belobug/VIctory-Belobug-AMD/blob/cd8dfc06ace7ccb91a19431e74bc2d0de2a10145/README.md) | ~22 FPS Series S in README | Wiederholte Angaben des 7900XTX-Forks; eigene Bestleistung am jüngsten Stand nicht belegt |
| [stellarr-studio/kelvaroq](https://github.com/stellarr-studio/kelvaroq/blob/e062a82e04853bbcae281d166980e0a9b65c975a/README.md) | ~10 FPS typisch, 7–8 FPS schwere Szenen, 4K RTX 5080 | Interessante gemeinsame Runtime und ausführliche Messmethodik, anderes Profil |
| [PastiiJay/klbf-lab](https://github.com/PastiiJay/klbf-lab/blob/0c529ca05c68e3d8a77f2a66143cb43b6b90e225/README.md) | 14 FPS durchschnittlich über 37 min für beta-2, nicht Standardbranch-Build | Nicht als Default-Branch-Referenz werten |
| [uiliannn/VItoriaBr](https://github.com/uiliannn/VItoriaBr/blob/1361e190b7f46df996f00ea1b540ecf66785127f/README.md) | 4–6 FPS Standardpreset; 22 FPS experimenteller Build | Experimentelle Messung nicht als Standard-Stand werten |
| [ka1qux/BRVIctory-ka1qux](https://github.com/ka1qux/BRVIctory-ka1qux/blob/05e4d5017c62f2ccb3ebe98f06a5cdcbe7064563/README.md) | ~15 FPS Series S RT aus, ~10,5 FPS Series X RT aus | 32 GB RAM, andere Hardware |

Weitere gesichtete Repositories: Marvisak/VIctory, fxceless/victory-fork, xcl-dev/xcl, proneonn/Vertex, ubiquitarianism/leonida, R0Y00/Xbox2Windows, DexNom/VIctory, DoubleGhosty/cool_fox, ZigZagBoi1/ScarlettPort, myokex/victory-amd, DesertGamer/VIPort, californiaAlmonds/OpenScarlett, ThatRecompDev/five-star-recomp, ProjectLeonidaL/leonida, brcly/TuxBox, sammwyy/xvalkyrie, pragyaangaur/VIctory-macOS, ranveersoni98/Xenora, zortos293/XSEmu, PlutonusDev/OpenEra. In den gesichteten READMEs kein hinreichend belegter stärkerer und technisch passender Standardbranch-Stand; dies ist keine Aussage über nicht geprüfte Dokumente oder künftige Commits.

## Technische Anschlussfähigkeit

| Bereich | Bewertung |
| --- | --- |
| Gemeinsame Codebasis | [Rust Cargo-Workspace](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/Cargo.toml), insbesondere victory-gpu, victory-shader, victory-hle, victory-loader |
| Konkreter Verwandtschaftsnachweis | [Compare im VInAMD-Fork](https://github.com/iLucasRT/VInAMD/compare/c9c932cbcc38defd8f1683d62dd298187f565031...5101a92f1fb5fce0e2973299c37044ddbb0e86ab): 40 Commits voraus, 0 zurück; c9c932c ist wiederum Vorfahr des Baseline-SHA mit ausschließlich README-Änderung |
| Vorhandene Finding-Prüfung | PERF-0001: [Baseline-Datei](https://github.com/avatarmanz/VIctory-7900XTX/blob/2de998014dc85ad68cd6422a250400c7084ace57/crates/victory-gpu/src/port_descriptors.rs) besitzt Deskriptor-Fallback, aber keinen Hot-page-Schalter; gezielte Übernahme plausibel, Code- und Laufzeitprüfung erforderlich |
| Andere Runtime-Familien | XCL/Leonida/alternative Recompilation nicht automatisch kompatibel; Schnittstellen, Speicher-/Shader-Semantik und Lizenzen gesondert beurteilen |
| Lizenzgrenze | Kein Kopieren aus GPL-2.0-only in GPL-3.0-or-later ohne nachgewiesene kompatible Erlaubnis; keine proprietären Spiel-/SDK-Daten übernehmen |

**Entscheidung:** `PROVISIONAL` ist verbindlicher Arbeitsanker, keine behauptete globale FPS-Krone. Die dokumentierte bewegte Series-X-Spielszene und der direkt verwandte, modular strukturierte Code machen ihn zum bestbegründeten Ausgangspunkt im geprüften Material. Ein Wechsel benötigt nachvollziehbar bessere technische Eignung und Leistungs-/Stabilitätsevidenz; alte Findings müssen dann gegen den neuen SHA neu eingeordnet werden. Keine automatische Bewegung des Ankers, keine Umsetzung von Fremdpatches durch den Monitor.

# GTA VI Performance Intelligence

Ein dokumentationsbasiertes, KI-kuratiertes Verzeichnis **nachgewiesener Performance-Verbesserungen** in GTA-VI-PC-Portierungen und verwandten Kompatibilitätsschichten.

**Langfristiges Ziel:** die wirksamsten und rechtlich/technisch integrierbaren Optimierungen für ein späteres eigenes Portierungsprojekt identifizieren. Dieses Repository speichert **nur Beschreibungen, Messwerte, Commit-/PR-Links und Herkunftsnachweise** – keinen fremden Quellcode, keine Spieldateien und keine Installer.

## Betriebsmodell

Eine **separate, stündliche ChatGPT-Aufgabe** soll GitHub mit den verbundenen Werkzeugen durchsuchen, neue Änderungen **ausschließlich am jeweils aktuellen Standardbranch** auswerten, Performance-Nachweise beurteilen und die Dokumentation samt Scan-Zustand fortschreiben.

- **Keine** GitHub Actions, Python-Skripte, sonstigen Executables oder automatisierten GPU-Benchmarks in diesem Repository.
- **Strikt inkrementell:** pro Quellprojekt ausschließlich die Commits **nach** dem in `state/scan-state.json` gespeicherten SHA bis zum beim Lauf festgehaltenen aktuellen Standardbranch-HEAD, beide auf derselben Git-Historie.
- Standardbranch **dynamisch** über die Repository-Metadaten ermitteln: kann `main`, `master`, `work` oder anders heißen. Keine Feature-Branches, offenen PR-Branches oder sonstigen Nebenbranches untersuchen.
- Nur tatsächlich durch nachvollziehbare **Vorher-/Nachher-Messungen** belegte Performance-Verbesserungen als Funde aufnehmen. Ein README-Versprechen oder technisch plausibler Code ohne Messwerte reicht nicht.
- Bereits auf den Standardbranch integrierte Pull Requests dürfen als Quellen verlinkt werden; eigenständige PR-Scans unterbleiben.
- Bei Fehlern oder unvollständiger Auswertung keinen ungesicherten Cursor weitersetzen.

## Semantik der Dateien

| Datei | Vertrag |
| --- | --- |
| [`AGENTS.md`](AGENTS.md) | Verbindliches Harness für jeden manuellen und geplanten ChatGPT-Lauf |
| [`WATCHLIST.md`](WATCHLIST.md) | Erlaubte Quell-Repositories und ihre Monitoring-Priorität; keine fest verdrahteten Branches |
| [`FINDINGS.md`](FINDINGS.md) | **Einzige maßgebliche Sammlung** qualifizierter Performance-Verbesserungen |
| [`PERFORMANCE.md`](PERFORMANCE.md) | Aus `FINDINGS.md` abgeleitete Übersicht und Einordnung; keine unabhängigen Behauptungen |
| [`state/scan-state.json`](state/scan-state.json) | Persistente SHA-Cursor und Scan-Metadaten je Quell-Repository |

## Initialisierung

Die in `scan-state.json` abgelegten SHAs sind **Start-Anker** von den bei der Einrichtung beobachteten Standardbranches. **Ältere Commits sind dadurch nicht als geprüft zu betrachten.** Der reguläre inkrementelle Scan beginnt strikt **nach** diesen Ankern. Eine rückwirkende Prüfung erfordert einen gesonderten, expliziten Auftrag.

Die ChatGPT-Automation ist noch **nicht** angelegt. Ob stündliche, vollständig unbeaufsichtigte Schreibvorgänge über die GitHub-Verbindung funktionieren, muss vor einem Produktivstart in einem echten geplanten Lauf geprüft werden.

Für Details zu Evidenzstandard, Abbrüchen und Wiederanläufen siehe [`AGENTS.md`](AGENTS.md).

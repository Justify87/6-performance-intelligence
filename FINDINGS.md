# FINDINGS.md – bestätigte Performance-Verbesserungen

**Kanonisches Register.** Hier werden ausschließlich Performance-Änderungen aufgenommen, die im **neu untersuchten Standardbranch-Commitbereich** durch konkrete, nachvollziehbare **Vorher-/Nachher-Messungen** gestützt und auf Codeänderungen zurückführbar sind.

## Aufnahmevertrag

Ein Eintrag verwendet die nächste stabile ID `PERF-0001` usw. und enthält:

- **Ursprung:** Quellrepository, Original-Commit(s), gegebenenfalls bereits gemergter PR, exakte Links zur Messdokumentation
- **Änderung:** Betroffenes technisches Subsystem, Ursache und relevante Anpassungen
- **Messung:** Messgröße, Ausgangswert, neuer Wert, absolute/prozentuale Veränderung mit korrekten Einheiten
- **Vergleich:** GPU, CPU, RAM/VRAM, Treiber, Auflösung, Spielprofil, Spielsituation, Dauer und Grafikqualität soweit dokumentiert
- **Nachweisstatus:** vom Entwickler berichteter Vergleich oder unabhängig reproduzierter Vergleich; Unsicherheiten und fehlende Details
- **Nebenwirkungen:** Bildqualität, Stabilität, zusätzliche Voraussetzungen, Regressionen
- **Integration:** mögliche Abhängigkeiten, Überschneidungen mit anderen Patches, Herkunft und Lizenzstatus
- **Übernahmen:** spätere Branch-Integrationen in anderen beobachteten Repositories als Links **im selben Fund**

Nicht aufnehmen: Menü-FPS als Beleg für Welt-FPS, nur theoretische Einsparungen, „FPS boost“ ohne Zahlen/vergleichbaren Test, diagnostische Anzeigen ohne Leistungsgewinn, reine PR-Branch-Ergebnisse.

## Registrierte Funde

_Noch keine: dieses Register beginnt mit dem initialen Bootstrap-Commit. Die historischen Commits vor den Scan-Ankern wurden nicht nachträglich bewertet._

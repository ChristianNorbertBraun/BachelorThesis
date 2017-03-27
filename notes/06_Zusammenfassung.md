Zusammenfassung
===============

* Ziel der Arbeit war herauszufinden ob Buchungssystem mit DFS realisierbar ist
* Und ob die Sklaierbarkeit davon profitieren kann
* Dabei wurden zunächst aktuelle Banken Systeme angeschaut
* Historische Entwicklung von eigen entwicklung zu drittanbietern
* Anforderungen eines Buchungssystems
* Sicher Abspeichern der Buchungen
* Hohe Verfügbarkeit
* Speichern vieler Daten
* Neue Herausforderung durch PSD2
* Sehr viel mehr Anfragen erwarten
* Vorallem Lesezugriffe

* Probleme die dadurch eben auftreten
* Systeme sind sehr alt
* Wurden ursprünglich nicht für offene Schnittstellen entwickelt
* Und auch nicht für Skalierung
* Die eingesetzten Persistenzschicht bremst die Skalierung des Systems aus
* Und schrenkt auch verfügbarkeit ein
* Und das aulesen von Blöcken an Buchungen ist auch langsam
* Und auch großes Problem sind die Kosten der IT der Banken
* 20 % des Verwaltungsaufwandes für die IT
* 13 Euro pro Konto

* Auf Grundlage der Probleme wurde DFS vorgestellt
* Dabei erstmal das BASE eben Verfügbarkeit in Vordergrund stellt
* AUch dadurch leichter skalieren lässt
* Die Wahl fällt nicht zwischen keiner Konsistenz und Verfügbarkeit
* Sondern ein bisschen Konsistenz verlust und Verfügbarkeit

* Nach dem Base Prinzip weiter zu erklärung zweier DFS im Detail 
* Wie schreiben diese
* Wie können sie Skaliert werden
* In welchen SItuationen sind sie Inkonsistent
* Dafür für Haystack und Google FS analysiert
* Weil diese beiden auch im Konzept angewendet werden sollten

* Dann wurden Probleme der aktuellen Buchungsysteme nochmal herangezogen
* und aufgezeigt, dass Skalierbarkeit, Ausfallsicherheit, Verfügbarkeit, 
Kosteneffizienz und Leistung von DFS gut sind

* Erstmal analysiert ob überhaupt alle Konten eine starke Konsistenz benötigen
* Annahmen getroffen für ein DFS für Banking
* Viele kleine Dateien
* viele lese zugriffe
* Viel auslesen zusammenhängender Daten
* Auf Basis der vorgestellten DFS
* Konzept entwickelt um Termineinlage und Ratenkredite abzubilden
* Einsatz von Buckets zum bündeln
* namespaceflattening um Speicher des Masters zu entlasten
* Und prefatching für entlastung des Masters

* zusätzliche relationale Datenbank soll auch andere Konten ermöglichen
* Speichert Kontostand und Buchungen 

* Das so entwickelte Konzeot nutzt vorteile des DFS und kombiniert sie mit der
Konsistenz relationaler Datenbanken
* Von der Skalierbarkeit sehr vielversprechend

* Implementierung versucht so nah am konzept wie möglich zu bleiben
* Aber das geht nicht, weil es kein DFS gibt, dass so funktiniert
* Das was am besten mit kleinen Dateien umgehen kann ist Haystack
* Deswegen Open Source Implementierung SeaweedFS gewählt
* Die drei Punkte die realisiert werden mussten waren
* Zusammenfassen von mehreren dateien in eine
* Pfad angeben
* Suchen nach Dateien
* SeaweedFS kann die letzten beiden mit dem filer lösen
* Die dritte wurde durch den account Updater realisiert
* Das Implementierte System ist Konzept sehr nahe
* unterschiede sind
* Das Informationen nicht in Buckets gefüllt werden sondern
* bestehende accountinfos immer überschrieben werden
* und das man immer ganze accountinfo lesen muss
* weil kein bucket index file
* deswegen können auch nur weniger Daten zusammengefasst werden

* Die Evaluation des Systems zeigt auf
* Fazit

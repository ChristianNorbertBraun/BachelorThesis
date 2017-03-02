Distributed File System als Grundlage eines Buchungssystems
===========================================================

* Wie bereits erwähnt kommen Skalierungs und Kosten Probleme auch durch die
ACID basierten Systeme
* Dabei wird die Funktionalität von ACID vll. nur für einen Bruchteil der
Operationen wirklich benötigt
* Trotzdem leidet die Skalierbarkeit und die Kossteneffizienz des ganzen
Systems darunter
* Systeme die das Problem der Skalierbarkeit und der Kosteneffizient gelöst 
haben funktioniern nach BASE
* In diesem Kapitel soll zunächt die Bedeutung von BASE erklärt werden
* Auch die Nachteile und Vorteile
* besonders bezogen auf Konsistenz
* Es gibt viele BASE basierte Systeme
* Skalierbarkeit und Verfügbarkeit Ausfallsicherheit , Kosteneffizienz ist
Königsdisziplin von Distributed File Systeme
* Wie einige Vertreter das machen soll erklärt werden
* Auf Vorteile und Grenzen von DFS eingehen
* Wo kann ein DFS innerhalb einer Bank eingesetzt werden?
* Was sind die Kriterien dafür?

## BASE
* Base erklären
* basically available soft state eventual consistency
* Was ist das CAP theorem
* ACID keine availability und BASE keine Konsistenz
* Beispiel mit 99,8% verfügbarkeit
* Gerade Konsistenz ist aber sehr dehnbar
* CAP ist kein 100%iger ausschluss von dem einen übrigen Attribut
* Ganz so ein schwarz weiß denken ist es nicht
* Für Availability muss Konsistenz etwas gedehnt werden
* Es gibt viele unterschiedliche Konsistenz arten
* Erklären von normaler und weak und eventual consistency
* Wieso ist das vielleicht gar kein Problem für den Nutzer
* Wieso lässt sich das viel besser skalieren?
## Funktionsweise
* Wo kommen die DFS überhaupt her?
* Wie werden Files gespeichert auf dem Rechner
* Wieso funktioniert das nicht mit Big Data?
* Object Storage tritt auf
* Was genau ist Object Storage
* Wieso kann das soviel mehr?
* Beispiele nennen Ceph, Chirp, Haystack, Google FS, Hadoop
* Generell viele ähnliche Ansätze
* Hier gehen wir auf zwei ein die unterschiedlich sind
* grobe funktionsweise erklären
* Google FS
* Haystack
* Was ist die Hauptaufgabe des Systems
* Welche Komponenten sind daran beteiligt?
* Was passiert um ein File zu speichern?
* Was passiert um ein File zu lesen?
* Wieso ist es Fehlertolerant und Skalierbar?
* Vll. Benchmarks
## Vorteile und Nachteile
* Viele auf große Files ausgelegt
* Immer sehr spezifischer Anwendungsfall
* Billig
* Gut Skalierbar
* Sehr gute Performance
* Können sehr viele Daten handeln
* Nehmen Komplexität vom Client
* Dynamisch skalierbar

## Anwendungsbereich
* Aufgrund der genannten Nachteile kommen nur Bereiche in Frage, die in den 
Händen der Banken liegen. 
* Eventuelle Konsistenz kann gegeben sein
* System muss natürlich zu einem vorhersehbaren Punkt konsistent sein
* Muss vom Kunden verziehn werden können


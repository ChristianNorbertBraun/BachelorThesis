Consistency in Non-Transactional Distributed Storage Systems
============================================================

S.0
* Mit der Zeit wurden unterschiedliche Bedeutungen mit dem Word Consistency
verbunden
* Am Anfang in den 80igern starke Consistency heutzutage wurde die Bedeutung von
Consistency im Rahmen der Availible und Scaleable Systems geschwächt und 
verwischt

S.1
## Introduction
* Distributed Systems sollen stärker Skalierbar und Fehler toleranter als
Zentrale Systeme sein
* Starke Konsistenz und hohe Verfügbarkeit sind aber bewiesen als sich 
gegenseitig ausschließend (CAP)
* Entweder Scalability, availability oder consistency aufweichen
* Mit dem Anstieg der kommerzielen Internet Anwendungen entscheiden sich 
Designer häufiger für availability und consistency als für konsistenz
* Daraus folgte die weak oder eventual consistency
* 50 unterschiedliche Konsistenzarten in diesem paper ?!

## Non-transactional consistency semantics
### Linerizability and related string consistency semantics S.8
* Der Goldstandard und die zentrale Konsistenz non-transaktionaler Systeme ist 
linerizability oder auch starke Konsistenz
* Linerizability ist eine Richtigkeits Bedingung, welche fordert, dass jede 
Operation sofort ausgeführt wird zwscihen Request und Response
* Wurde Lange Zeit für das Ideale gehalten von Distributed Systemen
* Auch wenn es sehr sinnvoll klingt, ist es schwer diese Bedingung zu 
implementieren, Nach Cap kann es aber nicht linerizability und Availability sein
* Atomare Operationen = linerizability
* Atomic register semantic single writer multi reader
* Jeder Read gibt den letzten tatsächlich geschrieben Wert zurück

### Weak and eventual consistency S.9
* Am anderen Ende des Konsistenz Spektrums liegt die schwache Konsistenz
* Wurde ursprünglich für jede Konsistenz verwendet die schwächer als die
sequential consistency
* Jetzt aber genauer definiert
* Weak consistency garantiert nicht, dass reads die neuesten geschrieben werte
zurückliefern
* Es gibt auch keine Ordnungsgarantie
* Scheint in vielen Situation undbrauchbar, ist aber häfig implementiert, wenn 
synchrone Protokolle zu teuer wären und es nur wichtig ist, dass die 
Informationen zwischen den replikas irgendwie ankommen
* Reads müssen nicht immer die exakt gleichen Daten zurückliefern

* Eventual consistency ist ein bisschen stärker als weak consistency
* Bei Eventual Consistency geht man davon aus, dass irgendwann alle reads gleich
sind, sofern keine weiteren Updates durchgeführt werden
* Wenn es keine writes auf ein Objekt gibt, wird das irgendwan konsistent sein
* Häufig genutzt in highly availible Systems
* Wird häufig genutzt wenn koordination nicht praktisch oder zu teuer ist
* Ordnung muss nicht eingehalten werden

* Auch definiert über folgende Eigenschaften ausgehend von den replicas
	* Eventual delivery: Wenn ein replica den write ausführt, wird er auch 
	irgendwann so auf allen ausgeführt
	* Convergence: Alle replicas mit den writes werden irgendwan konsistent
	* Termination: Alle Operationen werden ausgeführt

### PRAM und sequential consistency S.10
* PRAM stellt sicher (FIFO), dass die Prozesse die writes so durchführen,
wie sie auch aufgerufen wurden von der Reihenfolge her

* Sequential Consistency ist irgendwie nur auf einen Prozess bezogen nicht auf
alle

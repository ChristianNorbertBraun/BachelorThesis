CAP Twelve Years Later
======================
* Das CAP Theorem nimmt an, dass jedes über ein Netzwerkt Verteilte Datensystem 
nur 2 der 3 wünschenswerten Eigenschaften haben kann.
* Es ist aber möglich einen generellen Tradeoff zwischen den Eigenschaften 
herzustellen

* Es ist nicht so, dass es immer nur zwei sind und das dritte gar nicht mehr erfüllt wird
* Vielmehr ist es so, dass das dritte nicht perfekt erfüllt werden kann
* Es muss natürlich immer noch gewählt werden zwischen Konsistenz und 
Verfügbarkeit, aber die Entscheidung kann mittlerweile sehr flexibel sein
* Das heutige CAP Ziel ist die bestmögliche Kombination von Consistency und
Verfügbarkeit zu finden für eine Anwendung


## Why 2 of 3 is misleading
* Beispiel mit zwei Knoten Verfügbar und Inkonsistenz
* Will man das gewährleisten müssen die Knoten kommunizieren
* BASE findet ANwendung bei Suchmaschinen, proxy caches, und content
distribution systems
* Availability ist das wichtigste 
* CAP tauchte 1998 das erste mal auf wurde 1999 in einer Keynote zum 2000
Symbosium of Principles of Distriibuted Computing vorgtragen und das hat zu 
seinem Beweis geführt
* Innerhalb eines Systems können diese Entscheidungen mehrfach auftreten
und einmal C und einmal A gewinnen
* Die ganzen Attribute sind ja fließend  wie zum Beispiel A von 0 - 100 Prozent

## CAP Latency Connection
* Standard CAP ignoriert Latenz
* Die Entscheidung für CAP fällt immer genau in den Momenten wo Netzwerk Latenz
Eine Rolle Spielt
* Wird die Operation gecancelt
* Oder läuft sie durch und man riskiert Inkonsistenz

* Two Phase Commit verzögert diese Entscheidung nur

## Managing Partitions
* Ziel ist es den Partitioning Effekt auf Konsistenz und Verfügbarkeit zu
minimieren
* Partitions müssen irgendeine Form von partition recovery haben Wenn Partition
Aufhört

* Die Absicht aufnehmen und später durchführen zum Beispiel beim 
Aufladen einer Kreditkarte
* Erstellen von Einträgen geht aber
* Man kann klar nachvollziehen wenn sich Keys überschneiden
* Bei Updates ist das schwerer

* ABlauf ist immer 
	* Konsistenter Stand
	* Partitionierung
	* Wieder Konsistenten Stand herstellen

* Man fixed den State indem man da anfängt wo er noch nicht geteilt war

**Hinweis**
* Bei BASE muss man sich auch viele Gedanken über Updates machen
* Aber ich brauche das nicht, da es das ja nicht gibt
* Einfachste Variante Last wins
* Haupt sinn des Papers ist zu sagen, man sollte erkenne, wenn eine partitioning
vorliegt und darauf entsprechend reagieren und nicht einfach von vorneherein
Auf etwas verzichten

### Cap Confusion
* A und C werden häufig missverstanden
* Es geht nicht darum, dass der Service gar nicht mehr erreichbar ist
Und auch nicht darum, dass die gesamte Konsistenz drauf geht

A bedeutet, dass die Invarianten nach partition wiederhergestellt werden müssen

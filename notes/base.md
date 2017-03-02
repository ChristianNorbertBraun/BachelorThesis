BASE
====
**In Partinioned databases, trading some consistency for availability
can lead to dramatic improvements in scalability**

* Große Anwendungen brauchen viele Transaktionen
* Wenn die Anwedung auf Persistenz hängt, wird die Speicherung von Daten
zum Flaschenhals
* Zwei unterschiedliche Arten zur Skalierung von Anwendungen
	* Vertikal und Horizontal
* Vertikal ist super simpel aber hat Limitation
* Geht nicht beliebig weit und ist sehr teuer
* Horizontale Skalierung kann mehr ist aber komplexer
* Functional scaling -> Daten nach funktion verteilen
* Die zweite ist das aufteilen innerhalb von Funktionen sharding

## Functional Partitioning
* Für die Skalierbarkeit sinnvoll
* Konsistenz wird dann über foreign Keys sichergestellt
* Um die Constraints zu erhalten müssen die Tabellen auf einem einzigen
Datenbank Server laufen

## CAP Theorem
* Webservices können nicht ale drei Elemente gleichzeitig halten
* Consistency
	* Operationen werden immer auf allen Datensets gleichermaßen ausgeführt
* Availability 
	* Jede Operation muss in einer Antwort enden
* Partition tolerance
	* Operations werden fertig gestellt auch wenn Teile des Systems nicht
	erreichbar sind
* Jede Scaling Technik basiert auf Data Partitioning deswegen muss man sich 
immer zwischen Availability und Konsistenz entscheiden

## ACID Solutions
* Atomicity
	* Alle oder keine
* Consistency
	* DIe Datenbank wird in einem Konsistenten Stand sein wenn fertig
* Isolation
	* Transaktionen laufen ab als wären sie die einzigen
* Durability
	* Die Transaktion hat einen dauerhaften Stand geändert

* Man hat früh rausgefunden, dass man Datenbanken aber über die Server hinweg
Verteilen muss
* Deswegen der 2 phase commit
	1. Erst wird jede Datenbank gefragt ob ein precommit möglich ist, erst wenn
	alle zusagen gehts in phase 2
	1. die Zweite Phase commited dann die tatsächlichen Daten
* Wenn eine Datenbank was gegend en commit hat, werden alle zurückgerollt

* Dadurch bekommt man Konsistenz über die Plattformen
* Dadurch wird die availability aber beeinflusst
* Die Verfügbarkeit der gesamten Datenbanken muss berechnet werden
* Ist eine Datenbank nicht erreichbar läuft der 2PC nicht durch

## An ACID Alternative
* Acid geht auf Konsistenz aber wie bekommt man availability stattdessen
* Dafür gibt es BASE
* ACID ist pessimistisch nach jeder Transaktion muss konsistent sein
* BASE sagt, Konsistenz kann auch irgendwann eintreten
* BASE sorgt für Skalierungsmöglichkeiten, welche mit ACID nicht erreicht werden
können
* Beispiel mit Usern über 5 servern = nur 20 % der Nutzer sind betroffen

* BASE erfordert ein aufwendigere Analyse der Operationen und der logischen 
Transaktionen die im System durchgeführt werden als bei ACID Systemen

## Consistency Patterns
* Immer erstmal gruselig zu sagen man schraubt an der Konsistenz
* Inkonsistenz kann nicht vom Enduser versteckt werden
* Man schaut sich die Daten an und überlegt sich was ist wirklich wichtig?
* Welche Daten dienen nur zur performance
* Man kann über eine persistente Message queue auch Konsistenz sicherstellen
* Indempotenz der Operations checken, Anstelle von + - vielleicht einfach 
festen Wert setzen
* Extra Tabelle für die Transaktionen die erfolgreich durchgeführt wurden

## Soft State/Eventualle Consistenz
* Welchen Einfluss hat der Handel von Konsistenz für Availability
* Es gibt eine Zeit, in der der Nutzer Inkonsistente Daten sieht
* Aber aus Nutzer sicht, ist das kein Problem
* Interessant ist nur, dass der Stand irgendwann Konsistent sein wird
* 

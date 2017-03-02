Cluster Bases Scalable Network Services
=======================================

## Base Semantics
* Auf der einen Seite gibt es ACID was die stärkste Semantik aber auch die
größte Komplexität und die höchsten Kosten mit sich bringt
* ACID gibt keine Garantie in Bezug auf Verfügbarkeit, im Gegenteil
Es würde lieber nicht auf eine ANfrage Antworten als einen inkonsistenten Stand
zu antworten

* Für den Nutzer ist aber möglicherweise hohe Konsistenz und Durability gar 
nicht so wichtig
* Wichtiger sind
	* Stale data kann zeitweise tollerirt wrden wenn es wieder konsistent wird
	* Soft State
	* Approximate Abschätzung auf Basis des soft state und stale data
* Alles was nicht strikt ACID ist, ist BASE
* Umgang mit partial failure ist damit einfacher und billiger
* Konsistenz wird auch für EInfachheit getauscht
* Base schaft raum für bessere Performance 
	* BASE fordert keinen dauerhaften und konsistenten Stand zwischen 
	allen Partitionen
	* Mit Base können Kommunikationen vernachlässigt werden oder man schiebt sie
	auf eine spätere Zeit zu der sie besser passen
* Kombination von ACID und BASE macht durchaus sinn
* ACID für User verwaltung und Generelle Daten in Base
* BASE vereinfacht die Implementierung von Fehler Toleranz und Verfügbarkeit
enorm und erlaubt performance optimierungen

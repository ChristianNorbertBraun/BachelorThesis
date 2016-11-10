insatz eines Distributed File Systems (als Ersatz für die Relationale Datenbank - RDBMS) zur Verbesserung der Skalierung eines Banking-Buchungssystems.

* Aktuelle Kernbankensysteme arbeiten mit Relationalen Datenbanken
	(Kordoba)
	* Zu viele Daten, als das sie noch schnell verarbeitet werden können
	* Skalierung ist schwierig (Dimensionen nicht getrennt)
	* Durchsatz ist ein Problem (Sehr viele Transaktionen, die Tabellen einträge sperren)

* Distributed File System
	* Kommt bei anderen Big Data Firmen zum Einsatz
		* Google, Yahoo, Facebook
	* Bringt viele Vorteile mit sich
		* Stark skalierbar, sehr fehlertolerant
		* Kann aber keine Transaktionen
	* Entscheidende Werte sind File und Blockgröße
		* Anzahl Master und Namespace daten (Arten der Replicas)
	* Richtige Anwendung von Locks und Auswahl der Blocks sind auch relevant
	* Welche Konsistenz ist gewünscht?




* Sharing Economy (Kostendruck S3)
* Banken müssen mitspielen
* Banken haben keine Anwenderschnittstelle
* 10 Euro pro Jahr für ein Bankenkonto (Kordoba)
* Kosten reduzieren Kernbanksysteme on Demand
* Commodity Hardware


* Speichern wo ich gerade beim Lesen des Files bin

# Was bringen wir der Welt mit einem Skalierbaren Datenbanksystem
* Alles in die Frage stellen (Doktor Werner Vogel)
* Was brauche ich um diese Anforderungen zu erfüllen
* ACID vs CAP Denken immer revolutiönar
* Manches lässt sich nicht realisieren wegen Kosten
* Datenbanksystem überdenken
* Habe ich kein Girokonto, wieso dann diese Funktionalität
* Kostenreduktion durch On Demand
* Die Summe aller Kredit ist die Summer aller Debit
* Doppelte Buchführung(Double Entry Book keeping)
* Hoch performantes Lesen das ist das wichtigste
* Hoch performantes Schreiben
* Verstreuen der Files

* Umsatztabelle ist sehr generisch
* Durchsatz ist ein Problem (Transaktion ist minütlich)
* Es sind zuviele Daten

* System mit beliebig vielen Informationen (Und performant zurückgeben / Also Partionierbar)
	* mehrere Dimensionen (Kontonummer, Zeit)
	* Datenbank macht Fehler / Dimensionen nicht getrennt
	* Distribuetd File System löst das Problem

* Schreiben macht Problem
	* Journal immer zwei Konten sind betroffen
	* Jede Buchung ist ein Vorgang

* Saubere Api

* Was ist ein Umsatzsystem

* SweetFS (Filesystem)
* Golang

* Forschungsfrage:
Ersetzen des Transaktionssystem durch FileSystem

* Konsistenz und Partitionstolerant

* Estragon (Amazon)

* Anforderungen
	* Reinlesen und auslesen -> Ausfallsicher

* Api bauen
	* Queue Buchungsdateien auslesen und verteilen
* Distributed Lock


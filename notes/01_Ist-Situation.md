Was sind die Bestandteile eines Buchungssystem?
Wieso ist ein Datenbankread teurer als ein DFS Read?

Ist-Situation
=============
* Wie ist eine Bank Software technisch überhaupt aufgebaut?
	* Kernbankensystem als Herz
	* Je nach Bank unterschiedlich aber mittlerweile häufig Drittanbieter
	* Man hat früh gemerkt das Standardisierung wichtig ist (Fraglich ob relevant)
	* Beispiele von Anwendungen (CRM, Analyse, etc.)
	* Siloartige Anwendungen in deren Herz das Buchungssystem schlummert
* Welche Prozesse gehen über das Buchungssystem?
	* Jede Kontobewegung, Anlegen neuer Konten, Analysen auf Kontodaten
	* Konkret: Alle Bestandteile des Aktiv- und Passivgeschäfts der Bank
	* Aktiv und Passiv geschäft erklären
	* Darunter fallen Ratenkredite, Termineinlagen, Girokonto, Wertpapiere, etc.
* Welche Anforderungen ergeben sich daraus?
	* Sehr viele Read Zugriffe
	* Viele Write Zugriffe
	* Buchungssystem muss eine klar definiert Schnittstelle nach außen haben
	* Es muss extrem ausfallsicher sein
* Wie ist das ganze Technologisch umgesetzt?
	* Aktuell eigentlich immer mit einer Relationalen Datenbank
	* Fis Kordoba Flyer
	* Auch über den Einsatz von Hadoop wurde gesprochen
		* Fraud detection
		* Bessere Kundenanalyse
* Gibt es denn überhaupt ein Problem?
	* Systeme Alt und Wartungsintensiv
	* Generell ist die Leistungsfähigkeit von SQL Datenbanken gut (Info)
	* Problem ist der Zuwachs der Vertriebswege
	* Das sorgt auch für generell mehr Daten
	* Daraus ergeben sich mehr Anfragen insbesondere mehr Read Zugriffe
		* Beispiele wieso : Handy Kontostand checken
		* Einkaufen im Laden mit Kreditkarte oder Einer App wie Google Wallet 
		(Checken)
	* Offenlegung der Daten weiteres Problem
* Welche Probleme treten denn überhaupt auf?
	* Kosten für die Wartung und Instandhaltung werden höher (Beispiel) 
	* Datenbanksysteme sind ausgelastet
	* Wachstum ist nur begrenzt möglich (SPOF)
	* Es wird Funktionalität für Konten bereit gestellt, welche diese gar nicht 
	bräuchten
	* (MEHR)
* Was sind die Nachteile des aktuellen Systems 
	* Alle Daten, welche keine Transaktionssicherheit brauchen(Kredite, 
	Sparkonten)
	* Daten, welche selten gelesen werden stören andere Daten
	* Bald müssen alle Daten bereit gestellt werden PSD2
	* (MEHR)


(Ist das noch relevant?)
* Wie läuft eine Buchung zum Beispiel ab :
	* Zahlungsdienstleister
	* Verteilen der Buchungen
	

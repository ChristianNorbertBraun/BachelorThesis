* Wieso keine NoSQL Datenbank sondern ein DFS? Was sind die unterschiede?

Einführung
==========

## Motivation
* Banken sind Big Data (Hier brauche ich noch Infos!!!)
	* Kreditanträge
	* Kontoumsätze
	* Wo kommen all diese Daten her?
		* Online und Mobile Banking
		* Auch  der Einsatz von Banking APIs ermöglicht drittanbieter apps auf die Daten der Banken zuzugreifen und auch neue zu generieren.
		* Die Vielzahl an Verwendungsmöglichkeiten erhöht natürlich die Datenmenge
	* Probelm ist jedoch, dass die eingesetzte Technologie nicht hinterherkommt
		* Veraltete Kernbankensysteme
		* Die Systeme müssen hohe Bedingungen erfüllen, daher ist ein Wechsel schwierig
		* Alte Kernbankensysteme bremsen Fortschritt aus
	* Was sind die Herausforderungen, die es zu bewältigen gilt?
		* Analyse der Daten
		* Skalierung der Systeme
		* Kostenprobleme Speicher und Hardware kostet
		* Anfragen der Kunden bewältigen
		* Die Architektur muss entsprechend angepasst werden
		* Kernbankensysteme sind nicht darauf ausgerichtet
	* Welche VOrteile bringt uns Big Data im Bankensektor?
		* Erkennen von ungewöhnlichen Transaktionen
		* Für Kundenzugeschnittene Angebote
		* Kreditwürdigkeit und Risiko analyse wird genauer
		* Welcher Finanzsektor ist am Wachsen, beziehungswiese interessant
* Wie haben andere das Problem gelöst?
	* Firmen wie Google, Yahoo oder Facebook haben das gleiche Problem
	* Was genau hat Sharing Economy mit dem ganzen Thema zu tun?
	* Anwendung von einem DFS auf die Banking Welt.

* Durch Anpassen der Achitektur auf die Bedürfnisse kann Speicher und Rechenzeit sehr flexibel berechnet werden
* Daraus folgt ein wirtschaftlicher Vorteil
	* Kernbankensysteme müssen je nach Model unterschiedlich abkassieren
		* Leasing, Miete, pro Transaktion etc.
	* Kostenersparnis bei Servern
	* Kostenersparnis bei Speicher
	* Diese Ersparnis fällt im besten Fall sogar auf den Kunden zurück
	* Banken müssen sich den neuen Herausforderungen stellen
		* Sharing Economy und Fin Techs sind Konkurrenten
* Was zusätzlich noch mitkommt ist eine beliebige Skalierbarkeit und Ausfallsicherheit
	* Und bessere Performance

## Umfeld
* In wie weit sich die Sharing Economy durch den Einsatz eines Distributed File Systems auf ein Banking Buchungsystem übertragen lässt soll in dieser Arbeit bearbeitet werden
* Partner ist hierbei adorsys
* IT Consulting Firma, welche selbst einige Kunden im Versucherungs und Finanz sektor hat
* Beispielsweise Easy Credit wurde mit adorsys umgesetzt
* Desweiteren war auch die Umsetzung eines Open Source Kernbankensystems durch adorsys geplant

## Zielsetzung
* Relationale Datenbanken sind mit die ältesten Systeme
	* Steuern die Art wie Systeme gebaut sind
	* Doch Transaktionen und Indizes werden vielleicht auch gar nicht immer gebraucht
* Rausfinden Welche Teile eines Buchungssystems sich  durch ein Distributed File System realisieren lassen
* Methriken finden um die Ergebnisse zu Messen
* Es sollen weitestgehend alle Anforderungen eines Buchungssystems realisiert werden können
* Kostenersparnis sichtbar machen (Nur wie!!!!!!)




* Nicht im Rahmen der aktuellen Technologie denken, sondern Anfiorderungen hinterfragen(Vielleicht auch Motvation)
* Stark transaktionsbasiertes System nach echten Anforderungen hinterfragen
* Bessere Skalierbarkeit, Ausfallsicherheit und Performance
* Beantwortung der Frage ob ein Buchungssystem bzw. welche Teile/ Konten lassen sich realisieren
* Was bringt es mir tatsächlich das ganze umzusetzen

	* Könnten auch Faktoren zur Methodik sein
		* Infrastruktur
		* Data Storage
		* Data Processing and Management
		* Data Analytics
Methodik
========
Eher analytisch weniger Empirisch
Aber wieso eigentlich eher analytisch?
## Analyse der Ist-Situation (Nur Vorgehensweise erläutern)
* Aktuelle Kernbankensysteme und Buchungssysteme erläutern
* Anforderungen herausfinden
	* Insbesondere auf Skalierung und Ausfallsicherheit
* Zusammenarbeit mit Axel Wilkens und vll. Teambank
* Vor- und Nachteile herausarbeiten
* Gewichten der Anforderungen
## Analyse und Auswahl eines Distributed File Systems (Muss die Analyse auch mit in der Arbeit stehen?)
* Allgemeine Infos zu Distributed File Systemen sammeln
* Auf Basis der Anforderungen und Pros und Contras von Buchungssystemen Anforderungen an DFS herausarbeiten
* Ausgesuchte DFS müssen den Anforderungen entsprechen
	* Entsprechend der Anfoderungen analysieren
## Entwicklung des Konzepts
* Möglichkeiten zur Abbildung der Buchungssprozesse auf DFS erarbeiten
* Schritte für eine Buchung und das Lesen einer Buchung herrausarbeiten
## Beispielhafte Implementierung
* Auf Basis des Konzepts eine Implementierung anfertigen
## Bewertung der Lösung
* Performance Tests auf Implementierung
* Speicher Preise vergleichen
* Skalierung vergleichen
* Ausfallsicherheit vergleichen

Wesen und Probleme eines Buchungssystems (Hier fehlt mir noch viel)
========================================
* Kernstück eines Kernbankensystems
* Kernbankensystem erklären
## Begriffserklärung
* Abwicklung aller Prozesse 
* Zahlungsverkehr
* Nachverfolgung von Resourcen Nutzung
## Bestandtteile (FIS Kordoba Flyer) (Fällt vll. mit Begriffserklärung zusammen)
* Avaloq banking suite nutzt PL/SQL
* Zahlungsverkehr
* Aktivgeschäfte
* Passivgeschäfte
## Anforderungen(Quellen?)
* Ausfallsicherheit
* Skalierbar
* Vielleicht aus Arbeit zur Bankings Architektur
* Mandantenfähig(http://www.enzyklopaedie-der-wirtschaftsinformatik.de/lexikon/informationssysteme/Sektorspezifische-Anwendungssysteme/Finanzsektor--Anwendungssysteme-im/kernbanksystem)
* Es muss Modular sein um sich in ein Kernbankensystem eingleidern zu können
## Ablauf einer Buchung
* Posting trifft  ein
* Posting wird validiert
* Transaktion zum Kontoauslesen

## Probleme
* Aus Anforderungen ergeben sich Probleme
* RDBMS lässt sich nicht beliebig skalieren
* Locking verlangsamt Transaktionen
* Skalierung nur bedingt machbar
	* Sehr teuer und aufwendig (Nicht auf commodity Hardware)

Dirstributed File System als Backbone für Buchungssysteme
=========================================================
## Funktionsweise
* Abspeichern von Unstrukturierten Dateien auf vielen Data Clustern
## Anwendungsbereiche
* Big Data 
	* Weblinks, Bilder
	* Google, Facebook, Yahoo
## Vorteile und Nachteile
* Leicht skalierbar
* Ausfallsicher
* Aufwendige Analysen
* Keine Transaktionen
* Sie lösen das mit hoch skalierbaren Systemen, die sich dynamisch entsprechend der Anforderungen verhalten
* Infrakstruktur wächst entsprechend der Anforderungen
* Immer nur das Bezhalen was gebraucht wird

Konzept
=======
* Es gibt bestimmt Kontoarten, die sich nicht mit einem DFS realisieren lassen
## Transaktionen aufgeben
* Keine Transaktionen werden benötigt für
	* Ratenkreditkonto
	* Sparkonto
	* Termineinlage
* Diese Konten können realisiert werden
## Verwendung des Distributed File Systems
* Wie kommt hier das DFS ins Spiel?
## Aufbau der Anwendung
* Journal
* Client
* Backend
* DFS
* Rest Schnittstelle
## Erstellen einer Buchung
* Verschlüsselung?
## Lesen einer Buchung
## Skalierung
* Wie lässt sich das Konzept skalieren?
* SOA Prinzip beim Backend
* DFS von Haus aus stark skalierbar
* Keine Transaktionen sind sehr sicher
* Nur eventuelle Konsistenz
## Ausfallsicherheit (Vielleicht kein Teil des Konzepts)
* Ausfallsicherheit ist sehr wichtig
* Mein System muss auch Ausfallsicher sein
* DFS sind sehr ausfallsicher
* Wie Konfiguriere ich das System?

Implementierung
===============
* Als Open Source Projekt!
## SeaweedFS
## golang
## Schnittstelle zu SeaweedFS
## Bibliothek zur Abbildung von Buchungen
## RESTful Webservice

Evaluierung
===========
* Benchmark Tests
* Paper zu Skalierung eiens DFS lesen und Metriken entnehmen

Ausblick
========
* Einsatz von Amazon S3
* Validierungen implementieren

Zusammenfassung
===============





Was ist ein Buchungssystem? (!!! Schwer an Infos zu kommen)
===========================
* Rolle des Buchungssystems in einer Bank
	* Wichtiger Teil des Kernbankensystems
	* Was wird alles darüber abgewickelt?
* Aktiv-, Passivgeschäfte
* Welche Kontenarten sind daran beteiligt
* Transaktionen die über das Buchungssystem ablaufen
* Daraus folgende Anforderungen an das System
* Was sind Probleme bzw. Grenzen des Systems?
	* Skalierung ist aufwendig? Aber wieso?
	* Einsatz teurer Konten zur Abbildung simpler Operationen

Einsatz eines DFS
=================
* Von Anforderungen eines Buchungssystems auf Anwendungsbereiche von DFS verweisen
	* Banken sind Big Data!
	* Auch Sharing Economy ist hier Thema! (Amazon S3)
* Was sind Stärken bzw. Schwächen von DFS
	* Stark transaktionsbasiert (Was heißt das genau?)
	* Speichern sehr vieler unstrukturierter Daten
	* Einfache Skalierbarkeit
	* Ausfallsicher
* Was sind grundlegende Anwendungsbereiche von DFS?
	* Big Data(Facebook, Google, Yahoo, etc.)
	* GFS, Hadoop, Haystack, etc.
* Von Problemen des aktuellen Buchungssystems auf Lösung durch DFS führen

**Überleitung zur Motivation**
* Welche Kontoarten und Geschäfte kommen für ein DFS überhaupt in Frage?
	* Systeme die Konsistenz aufgeben können
* BASE vs. ACID und CAP Theorem eventuell einbauen.
* Konsistenz wird gar nicht immer benötigt, aber von Bankensystem immer bereitgestellt
* Kostensenkung als Ziel
* Anzahl benötigter Konten für Ratenkredit etc.
* Braucht ein Sparkonto Konsistenz zu jedem Zeitpunkt? Diese Frage will ich hier beantworten!

**Konkrete Anforderungen an ein DFS in einem Buchungssystem**
* Kleine Files lesen
* Map Reduce um Analysen durchzuführen
* Schnelle Read Zugriffe und nicht blockierende Write Zugriffe

## Seaweed-fs
* Seaweed-fs als Implementierung von Facebooks Haystack
* Implementierung von Haystack kurz anreißen und Vorteile erwähnen
	* Sehr gut im Lesen kleiner Files
	* Ausfallsicher und leichtgewichtig

Konzept
=======
* Wie realisiere ich das alles?
* Bezug auf adorsys [wiki](https://wiki.adorsys.de/display/dev/General+Accounting)
* Grundlegendes Konzept erklären
	* Aufbau der Anwendung
	* Wie wird DFS genutzt
	* Wie wird Schnittstelle genutzt  
<img src="images/architektur.jpg" width="250"/>
* Darausfolgende Arbeitsschritte 
	* API für Seaweed schreiben
	* Lib für Zugriff auf Seaweed API und DFS schreiben
	* REST Service implementieren
	* Testing

Implementierung (!!!)
===============
* HTTP Calls zu Seaweed und Filer
* Distributed Locking in DFS Lib
* Implementierung eines Journals

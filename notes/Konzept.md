Einführung
==========

## Motivation
* Banken sind Big Data (Hier brauche ich noch Infos!!!)
* Sharing Economy (Amazon s3)
* Wirtschaftlicher Vorteil
* Skalierbarkeit von Buchungssystemen

## Umfeld
* adorsys
* Consulting IT Firma mit Versicherungen und Banken als Kunden
* Easy Credit
* Open Source Kernbankensystem

## Zielsetzung
* Nicht im Rahmen der aktuellen Technologie denken, sondern Anfiorderungen hinterfragen(Vielleicht auch Motvation)
* Stark transaktionsbasiertes System nach echten Anforderungen hinterfragen
* Bessere Skalierbarkeit, Ausfallsicherheit und Performance
* Beantwortung der Frage ob ein Buchungssystem bzw. welche Teile/ Konten lassen sich realisieren
* Was bringt es mir tatsächlich das ganze umzusetzen

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
## Begriffserklärung
* Abwicklung aller Prozesse 
* Zahlungsverkehr
* Nachverfolgung von Resourcen Nutzung
## Bestandtteile (FIS Kordoba Flyer) (Fällt vll. mit Begriffserklärung zusammen)
* Zahlungsverkehr
* Aktivgeschäfte
* Passivgeschäfte
## Anforderungen(Quellen?)
* Ausfallsicherheit
* Skalierbar
* Vielleicht aus Arbeit zur Bankings Architektur
* Mandantenfähig(http://www.enzyklopaedie-der-wirtschaftsinformatik.de/lexikon/informationssysteme/Sektorspezifische-Anwendungssysteme/Finanzsektor--Anwendungssysteme-im/kernbanksystem)

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

Konzept
=======
* Es gibt bestimmt Kontoarten, die sich nicht mit einem DFS realisieren lassen
## Transaktionen aufgeben
* Keine Transaktionen werden benötigt für
	* Ratenkreditkonto
	* Sparkonto
	* Termineinlage
* Diese Konten können realisiert werden
## Ausfallsicherheit
* Ausfallsicherheit ist sehr wichtig
* Mein System muss auch Ausfallsicher sein

## Verwendung des Distributed File Systems
## Aufbau der Anwendung
## Erstellen einer Buchung
* Verschlüsselung?
## Lesen einer Buchung
## Skalierung

Implementierung
===============
## SeaweedFS
## golang
## Schnittstelle zu SeaweedFS
## Bibliothek zur Abbildung von Buchungen
## RESTful Webservice

Evaluierung
===========

Ausblick
========

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

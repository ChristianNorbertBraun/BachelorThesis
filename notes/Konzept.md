Einführung
==========

## Motivation
* Banken sind Big Data (Hier brauche ich noch Infos!!!)
	* Was heißt das konkret?
	* Welche VOrteile bringt und Big Data im Bankensektor?
	* Was entsteht daraus?
		* FinTech?? Hat das was damit zu tun?
	* Was sind die Herausforderungen, die es zu bewältigen gilt
* Wie haben andere das Problem gelöst?
	* Sharing Economy (Was ist das genau? Welche Bedeutung hat es? Wie löst es das Problem von Big Data?)
* Durch Anpassen der Achitektur auf die Bedürfnisse kann Speicher und Rechenzeit sehr flexibel berechnet werden
* Daraus folgt ein wirtschaftlicher Vorteil
	* Kernbankensysteme müssen je nach Model unterschiedlich abkassieren
		* Leasing, Miete, pro Transaktion etc.
	* Kostenersparnis bei Servern
	* Kostenersparnis bei Speicher
	* Diese Ersparnis fällt im besten Fall sogar auf den Kunden zurück
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
## Ausfallsicherheit (Vielleicht kein Teil des Konzepts)
* Ausfallsicherheit ist sehr wichtig
* Mein System muss auch Ausfallsicher sein
* DFS sind sehr ausfallsicher
* Wie Konfiguriere ich das System?
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

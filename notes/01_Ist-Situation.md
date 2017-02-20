
Was ist die rechtliche Grundlage von Buchungen?
Was ist eigentlich eine Buchung?
Generell Accounting
Was sind die Bestandteile einer Buchung
Double Entry Bookkeeping
Was sind die Bestandteile eines Buchungssystem?
Wieso ist ein Datenbankread teurer als ein DFS Read?

Wesen und Probleme eines Buchungssystems
========================================
* IT Strukturen von Banken sind alt und lange gewachsen Kernbanken Enzy
* Neue Anforderungen haben sich ergänzend zum bestehenden System hinzugefügt
* Dadurch sind Teile des Systems auch noch in Cobol und Assembler geschrieben
* Um die Bedeutung des Buchungssystems innerhalb der Anwendungssysteme von Banken
zu verstehen wird zunächst die Entstehung un die Struktur der Anwendungssysteme 
in Banken beleuchtet
* Danach werden die Aufgaben und Bestandteile eines Buchungssystems erarbeitet
* Welche Anforderungen ergeben sich für das Buchungssystem daraus
* Welche Probleme könnte das Buchungssystem dabei haben

## IT-Landschaft von Banken
* 1970 erste Programme für einzelne Anlagen ITidF S.9
	* Kredite, Einlagengeschäft, Wertpapierabwicklung
* Im Zentro stand das Konto ITidF S.19
* Das Zentrum war also das Buchungssystem
* Drum rum entwickelten sich Abwicklungssysteme 
* Darum haben sich wiederum Programme zu Unterstützung der Kundenberatung und
Sachbearbeiter gelegt SuPdIiB S.10
* Führte zu Silostruktur oder auch Spartensysteme SuPdIiB S.11
* Ursprünglich noch alles Eigenentwicklung ItidF S.52 Kernbanken Enzy
* Aber hohe Komplexität war für Banken nicht tragbar
* Viele heterogene Teilsysteme führten zu starken Abhängigkeiten und 
Interdepedanzen SuPdIiB S.13
* Lösung versprachen sich Banken vorallem durch Systeme von Drittanbietern
* Gesamtbankenlösungen 
* Standardisierung ITidF S.56
* Aufgrund der Komplexität hat sich Gesamtbankenlösung nicht durcgesetzt sondern
Standardisierung und Best Of Breed
* Genossenschaftsbanken Fiducia
* Sparkassen finanz Informatik
* Kleinere banken Kordoba, SAP etc.
* Nur noch große Banken wie Deutsche Bank kann sich eigenes System erlauben
Folien Kernbanksystem S.6
* Genereller Aufbau ist aber immer noch der gleiche.
* Ganz außen auf der Abbildung ist Visualisierungsschicht ITidF S.104
	* Schnittstelle zu Benutzer
	* Läuft beim Client
* Darstellungsschicht
	* Wandelt Daten um
* Anwendungsschicht
	* Geschäftslogik für Teilbereich
	* Verarbeitet Daten
	* Umfangreiche Funktionen zur Manipulation von Daten
	* Schnittstelle auch zu neuen Vertriebswegen
	* Teilweise noch in Assembler und oder Cobol geschrieben
* Datenhaltungsschicht
	* Verwaltet Daten der verschiedenen Anwendungen
	* Datenbanken und Basisfunktionalität für Daten
	* Host + Datenbank
	* In der Finanzindustrie immer! Relationale Datenbanken IBM DB2 oder IMS/DB
	ITidF S.105
* Die Datenhaltungsschicht im Zentrum entspricht dem Buchungssystem und ist das 
Herz  der Banken. Wird auch als Kernbankensystem bezeichnet. Teilweise
ist damit aber auch noch zusätzlich die Abwicklung von Geschäftsprozessen gemeint

## Das Buchungssystem und seine Aufgaben
* Das Buchungssystem steht also im Zentrum jeder Kontenbezogenen Transaktion.
* Als dieser Kern, welcher einem Großteil der Anwendungssysteme zugrunde liegt
wird das Buchungssystem auch als Kernbankensystem bezeichnet. ITidF S.58 
* Als ein solches bearbeitet es alle Kontenbasierten geschäftsvorfälle einer Bank.
Kernbank Enzy
* Es muss somit alle Funktionen der Banken unterstützen können DiF S.12
	* Zahlungsverkehrsfunktion
	* Passivgeschäfte
	* Aktivgeschäfte
* Und Stammdaten verwaltung als Kunden Daten Kontoführung  S.86
* Aktivgeschäfte ITidF S.58 DdF S.160, S.84 Bankwirtschaft S.96
	* Kreditabwicklung
	* Wertpapiergeschäft
	* Hypotheken
	* Baukredit
	* Investitionskredit
	* Klassischer Kredit
	* Steigende Anzahl der Kreditgeschäfte Statistik Bankenverband
* Passivgeschäfte S.20 DiF
	* Termineinlagen
	* Spareinlagen
* Zahlungsprozesse
	* Abwicklung der Zahlungsprozesse
	* Sehr wichtiges Geschäft Bankwirtschaft S.142
	* Schecks, Überweisungen, Lastschriften etc.
	* Immer relevant für Kunden
	* Gerade weil es immer mehr in Richtung Bargeldloses Bezahlen geht ein
	wachsender Ast Statistiken Bankverband Anzahl Bankkundenkarten online Banking
## Anforderungen
* Aufgabe des Buchungssystems ist eine sinnvolle Nachvollziehung aller Ressourcen 
Bewegungen
* Schon allein aus rechtlicher Sicht dürfen hier keine Fehler passieren oder 
Umsätze verloren gehen. Es müssen Rechnungsabschlüsse in gewissen Zeiträumen
erstellt werden. Zu einem gewissen Zeitpunkt müssen Konsistente Daten vorliegen.
Kernbanken Marktstudie
* Soll Ist Vergleiche Bilanzierungsstandards Kernbank Enzyklop
* Umsätze werden in der Regel auch mit der Doppelten Buchführung realisiert
* Das heißt die Debit und Kredit Seite müssen sich immer ausgleichen
* Gerade durch die Einführung neuer Services Capgemini S.4 und den
immer mehr steigenden Bargeldlosen Zahlungsverkehr steigt auch die Belastung der 
Buchungssysteme. Sie müssen folglich sehr skalierbar und ausfallsicher sein
* Systeme werden deshalb redundant angelegt und sichere und performante Netze
gestellt S.97
* Banken und Kunde fordern eine schnelle Abbildung bankfachlicher Anforderungen
Enzy Kernbank
* Sie benötigen sehr viel Speicher Big Data requires Big storage web
* Früher nur Speichern der Daten jetzt muss damit auch was gemacht werden
* In diesem Rahmen Stelle ich PSD 2 vor
* Extrem viel mehr Reads als zuvor
* Analysen sind möglich 
* Mehrere Apps gleichzeitig greifen auf Konten zu
* Nicht nur für PSD 2 sondern auch für die Bankeigenen dienste benötigt ein
Buchungssystem eine gut deifnierte Schnittstelle
## Probleme
* Banken stimmen zu, dass etwas getan werden muss Capgemini S.8
* Die Systeme der Banken sind alt und schwer zu ändern ITidF S.23
* Wegen komplexer Architektur hoher Wartungsaufwand
* Die Buchungssysteme sind nicht für die kommenden Anforderungen ausgelegt
* Generell sind relationale Datenbanen sehr performant aber sie sind nicht
zur Skalierung gemacht
* ACID Prinzip
* Auch wenn funktionalität nicht genutzt wird, wird sie bereit gestellt
* Locking Normales Locking gegen Optimistic Locking
* Aber häufig DB2 verwendet das kann das nicht! ITidF
* Gerade durch PSD2 und Neue Vertiebswege wird Lese Zugriffe erhöhen
* IT Systeme sind jetzt aber schon unfassbar teuer
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
	

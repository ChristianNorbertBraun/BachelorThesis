IT in der Finanzbranche
=======================
* IT als besondere Bedingung für Bank (Backbone) S. 9
* IT als Nervensystem
* Banken verkaufen Informationen nicht Kredite (!)
* 1970 Programme für einzelne Einlagen 
	* Kredite, EInlagengeschäft, Wertpapierabwicklung
* Buchungssystem Kontokorrent !!!
	* Um Buchungssystem stehen Abwicklungssysteme S. 19 ff
	* Silostruktur Grundlage für heutige Problematik (Why?)
* Aufteilung der Universalbanken S. 21
	* Transaktionsbanken
	* Produktbanken
	* Vertriebsbanken
	* Abwicklungsbank -> brauchts KBS

## Probleme der Bank IT S. 23
* Einführung der alten Tehchnologie in neue Architektur
	* Hoher Wartungsaufwand für Kontokorrent, Zahlungsverkehr
* Mehr Transaktionen über andere Services

## Kosten der IT S. 29
* Aufgeteilt in Run und Change
	* Run sind die Betriebskosten (70-80%)
	* Change die Kosten zum Verändern der IT-Struktur (20%)
* Deutsche Bank run 2.6 Mrd € 2004 S.30
* HypoVereinsbank mehr als 900 Mio € 2004
* Zehn größten Institute etwa 6 bis 6.5 Mrd € für IT oder gesamt 14 Mrd €
* S.31 IT Kosten in unterschiedlichen Services
* IT-Kosten pro Girokonto 57 € S.31
* 50 - 60 % reine Betriebskosten -> 28.5 € pro Girokonto im Jahr

### Kosten von bankbetrieblichen IT-Projekten S.33
* Kernbuchungssystem 3-5 Jahre > 120 Mio €

# 2. Gestaltungsansätze in der FDL-Informatik
* Strategische Perspektive Strategie S.40 - 43
* Organisatorische Perspektive Prozesse
* Informationssystem Perspektive Systeme

* Bank aus 4 Kernprozessen
	* Akquisition
	* Distribution
	* Abwicklung
	* Service
* IT kein Selbstzweck sondern sie soll Prozesse unterstützen
* S.49 Genereller Aufbau der Systeme
* S.56 Aufbau aus Standardsoftware die parametrisiert wird

**Hinweis**
* Kernbanksystem Eigenentwicklung oder von Drittanbietern
* Standardsoftware hat sich durchgesetzt mit Best of Breed Ansatz
* Gesamtbankenpakete in Deutschland nicht besonders erfolgreich S.56
* Generelle Systemlandschaft und Bedeutung des Buchungssystems (Zwiebeldesign)

* Anwendungsgebiete: S.56 
	* Zahlungsverkehr
	* Kontoführung
	* Kreditabwicklung
	* Wertpapiergeschäft
	* Sparverkehr
	* Kasse

* Wegen Best of Breed gibt es Systeme die nur noch den Buchungskern abbilden
	* Das sind die bekannten Kernbanksysteme

**Hinweis**
* Banken haben ursprünglich Systeme selbst gebaut jetzt aber mehr Drittanbieter
	* S.52 Eigenerstellung, S.56 Standardsoftware und Gesamtbankpakete
* Systeme wurden so Komplex, dass Komplettlösung nicht mehr reicht
* Standardtisierte Best of Breed Lösung hat sich durchgesetzt
	* Selbstentwicklung nur noch wo es nötig ist S.69
* In desem Kern sitzt der Buchungskern mit besagten Aufgaben
	* Dieser Buchungskern sind die Kernbankensysteme
	* Beispiele FIS Kordoba
	* Fiducia IT (S.78 Daten zur Fiducia) Von Daten auf Kontopreis
	* etc.
* Drum rum liegen die Anwendungen
* Was zahlen Banken für diese Architektur aktuell
	* Beispiele von Banken
	* Pro Girokonto

# 4. IT-Architektur in der Finanzbranche

* Aufgeteil in Anwendungsarchitektur und Systemarchitektur S.94
	* Anwendungsarchitektur ist die tatsächliche Anwendung S.99
	* Systemarchitektur ist die eingesetzte Technologie und Hardware S.95
* Die meisten geschäftskritischen Anwendungen laufen auf IBM Mainframes
* S.96 Deutsche Bank hat Rechenzentrum an IBM ausgelagert theoretisch auch AWS?
* S.96 Systeme redundant anlegen und S.97 Netze müssen Sicher und performant sein

## Anwendungsarchitektur in der Finanzbranche S.104
* Visualisierungsschicht
	* Schnittstelle zum Benutzer
	* Läuft beim Client
* Darstellungsschicht
	* Erhält Daten von Anwendungsschicht und bereitet sie auf 
	* Daten von der Visualisierungsschicht zu verarbeiten damit Anwendung sie
	verarbeiten kann
* Anwendungsschicht
	* Geschäftslogik der Bank.
	* Verarbeitung der Daten
	* Umfangreiche Funktionen zu manipulation von Daten
	* Nutzt Datenhaltungsschicht
	* Schnittstelle auch zu neuen Vertriebswegen
	* Häufig noch in Assembler und COBOL geschrieben
* Datenhaltungsschicht
	* Verwaltung der Daten der verschiedenen Anwendungen
	* Datenbanken und Basisfunktionalität von Daten
	* Host + Datenbank
	* Siehe Bild S.104
	* S.105 Datenhaltung erfolgt in der Finanzindustrie mit relationalen 
	Datenbanken. IBM DB2 oder IMS/DB (!!!!!!)
* Nicht alle Schichten müssen ausgeprägt sein
* Darstellungsschicht zum Beispiel raus

## Idee der Building Blocks S.107
* Anwendung aus Blöcken zusammenbauen S.108
* Aufteilung in Blöcke
	* Vertrieb, Beratung
	* Wertpapiere
	* Zahlungsverkehr
	* Geld und Devisen
* Für mein Thema irrelevant

## Pfad zur neuen FL-Informatik S.109 (Vielleicht doch ganz wichtig)
* Aktuell stehen Konto im Zentrum
* Es sollen aber Kunde und und Produkt bezogen sein
* Großprojekte sind schwer deswegen Erneuerung in Einzelschritte
	* Deckt sich mit meiner Idee Kredite etc. abzudecken

### Erstellung neuer Anwendungssysteme S.109
* Sollte sich an Unternehmensstrategie, Geschäftsprozessen und 
Informationssystemen orientieren
	1. An den Zielen des Geschäftssegmentes orientieren
	2. Auf Basis optimierter GEschäftsprozesse schlanke Anwendungssysteme 
	schaffen	
	3. Im Zentrum steht Auftragssteuerung an das Anwendungsmodule angekoppelt 
	werden
* S.110 Abbildung
* Neukonzeption der IT-Systeme
	* Entflechtung der IT-Systeme S.112
	* Vertriebssysteme
	* Abwicklungsprozesse (Kontokorrent, Einlagen, Kredite)
		* Die müssen groß gebaut werden S.113 oben

### Grundsanierung der operativen Systeme S.114
* S.115 Abbildung 4-11
* Altsysteme Schritt für Schritt entkernen
* Auftragsgesteuere IT-Architektur
* Implementierung des Steuerungssystems
* Erfordert herauslösen von Kontokorrent (Buchungskern) S.116 oben

* Bankensteuerung S.123

* Deposits Management S.124
	* Realisiert Zahlungsverkehr
	* Wertpapierhandel
	* Depotverwaltung
	* Führung und Abwicklung aller Saldobasierten Bankprodukte
	* Postbank führt so 4,6 Millionen Kontokorrent-Konten 2005
	* S.125 SAP System Vorteile (Auch Skalierung)

* Anzahl Transaktionen so groß, dass Postbank in jeder Niederlassung eigenes
Kernbankensystem Kordoba installiert um Transaktionen Herr zu werden S.129 unten
* Postbank 12,6 Mio Transaktionen täglich 2003 S.130

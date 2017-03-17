Implementierung
===============
* Ein DFS so wie im Konzept beschrieben existiert nicht
* Bei der Implementierung muss auf bestehendes DFS ausgewichen werden
* Wichtigste Entscheidung ist dabei das abspeichern vieler Dateien
* Wie bereits erarbeitet bietet sich haystack dazu mehr an
* Haystack hat aber leider keine zugängliche Version
* Deswegen Open Source Implementierung Seaweed fs verwendet

* Dann soll kurz über die Sprache Go gesprochen werden
* In der wurde alles implementiert auch seaweedfs
* Im Kapitel soll etwas näher auf Seaweedfs eingegangen werden
* Bezgen auf das was es bietet und wie man es nutzten kann
* Durch eigens entwickelte bib
* Dann betrachten wir Aufbau der Implementierung 
* Wie werden hier Buchungen gelesen und geschrieben

* Implementierung ist eher als Proof of concept anzusehen
* Hat nicht den Anspruch vollständig zu sein
* Gerade in bezug auf sicherheit und authentifizierung wurde kein Aufwand
unternommen
* Schwerpnkt lag auf Skalierung und Ausfallsicherheit

## golang
* Go ist eine Programmiersprache entwickelt Robert Griesemer, Rob Pike and Ken 
Thompson bei Google. 
* Wurde open Source 2009 und aktuell ist Version 1.8 veröffentlicht
* Im offiziellen FAQ beschreiben sie den grund für Go
* Furstiert über bestehende Sprachen zur System Programmierung
* Man muss sich entscheiden zwischen schneller compilierung, ausführung und
leichtigkeit beim Programmieren.
* Go versucht die leichtigkeit beim programmieren, die eine interpretierte 
und dynamische typisierte sprache mit sich bringt mit den vorteilen
einer statischen typisierung zu vermischen
* Außerdem ist go schnell im compileren auch großer programme
* hat eine umfangreiche standard tool chain
* compilieren, tests. Benchmark analysen kommt alles mit
* Tests wurden auch ausgiebig für eigene lib und das eigentliche
System benutzt
* Kompilieren für unterschiedliche betriebssysteme, so das man
binary einfach ausführen kann
* Keine VM benötigt

* Go kommt aus der C Familie, ähnelt vom syntax auch davon

* Besonders eignet sich go zur entwicklung von web servern
* Dafür wird es auch von vielen entwicklern eingesetzt wie die survey zeigt
* Go liefert eigenen web server mit 
* Und seine größte Stärke spielt sich hier voll aus, und zwar die go routines
* Go routines sind ein einfacher weg parallelität in go code zu bringen
* Dabei sind go routinen leichter als threads
* Brauchen weniger Hauptspeicher
* Sind schneller Hochgefahren und gestoppt
* Weil nicht vom Betriebssystem ausgeliefert sondern von der go runtime selbst
* \cite{Krishna Block}
\cite{gobook}
* In diesem Projekt ist sowohl seaweedFS in go programmiert
* Aber auch alle anderen Systeme und an einigen Stellen machen
wir von go routines gebrauch

## Seaweedfs
* Seaweedfs ist open source implementierung von haystack
* Das heißt read write läuft im endeffekt genauso ab
* Anlegen von files
* Schritte
* auslesen von files
* mit ID nach gleichem schema wie bei haystack
* Nun wurde das konzept ja auf einigen Ansätze entwickelt
* Die Haystack nicht erfüllt
* Darunter das vergeben eigener Schlüssel
* Also Bic Iban + timecode
* Und keine Möglichkeit nach files zu suche
* Lesen benötigt immer schlüssel
* Und auch das logisch zusammenhängende Dateien nicht in gleichen
* Volume landen
* Seaweedfs liefert aber einen filer, der die ersten beiden problee lösen kann
* da kann man Dateien einfach mit ordnern und pfaden ablegen
* Keine richtigen Ordner und pfade mit vielen metadaten
* Seaweedfs hat ansatz von haystack verinnerlicht
* Aber SeaweedFS möchte so wenig zugriffe auf die festplatte wie möglich
deswegen funktioniert das konzept ein bisschen anders
* um in seaweedfs von pfad und id auf file zu kommen werden folgende Schritte
benötigt
* file parent directory zu directory id
* directory id + filename zu file id
* mitd der fileid die datenblock finden
* 3. wird von seaweedfs schon geliefert
* dier ersten beiden schritte sind aufgaben des filers
* Die Ordner werden im Memory gespeichert 
* und die directoryid + file id in einer level db einem key value store

* Mit einem filer ist es jetzt möglich 
* Eigene schlüssel bzw namen für Dateien zu vergeben
* Und man kann sie in Ordner legen
* Die Ordner sollen den ersten Teil unserers Schlüssels darstellen
* Der filename den anderen

* Beispiel erstellen einer Datei
* Pfad und timestamp
* Legt file ab
* Beim auslesen liest filer file id aus und fragt selbstständig filer an
* und liefert daten zurück
* Aber man kann mit filer jetzt auch suchen
* Und zwar über die mitgabe eines latestfile names
* Dann werden alle files zurückgegeben, die entsprechend nach dem mitgegebenen
kommen
* Auch ein limit kann mitgegeben werden
* Für streng aufsteigende IDS ist das optimal
* Da der File erin Problem im sinne von single point of failure
und Skalierbarkeit darstellt,
* bietet Seaweed fs den einsatz eines distributed filers an
* Anstelle von level db nutzt dieser dnn entweder cassandra oder
Redis zum spiechern der mappings
* Man kann auch einfach mehrere seaweed filer starten die immer eigenen
namespace abspeichern und deren anfragen zugeteilt werden
* Im Rahmen dieser Arbeit reicht der Filer aber vollkommen aus

* Die Kommunikation mit seaweedfs wurde mit einer eigens entwickelten lib 
ermöglicht die open source ist
* Heißt Weedharvester und bietet die einfachsten funktionen an
* Das besondere, man muss keine files hochladen sondern einfach nur
* einen buffer mit bytes die dann entsprechend aabgelegt werden
* Es gibt sowohl möglichkeiten über master als auch über filer zu 
kommunizieren
* Im sinne der replikation bietet seaweedfs einmal die möglichkeit bei jeder
datei replikation zu bestimmen
* Oder für alle Dateien beim hochfahren des Servers
* Schema ist einfach 100
* Erste stelle datacenter
* zweite stelle diferent rack gleiches datacenter
* dritte stelle gleiches rack anderer server

* Beim starten einfach dem master mitgeben
* und volume server mit entsprechenden eingeschaften starten
* Neben der fähigkeit viele kleine files zu speichern, überzeugt vorallem
einfachheit des systems sie in dieser Arbeit einzusetzen
* Auto von Seaweedfs hat docker image veröffentlich, so dass es einfach
über ein docker-compose Datei konfiguriert, gestartet und gestopt werden kann
* Beispiel Bild einfügen
* Auch in dem Projekt wurde seaweedfs mittels eines docker-compose files 
verwendet.

## Aufbau der Implementierung
* Komplette Implemetierung ist under MIT License auf Github öffentlich zugänglich
* Das System besteht aus einem Backend
* Einem account updater master und slaves
* und dem DFS

* Backend
	* Schnittstelle zum client
	* Kommuniziert mit DFS und mit account updater
	* SPeichert zunächst jede Anfrage ab und teilt updater mit, dass
	es neue gab
	* Bearbeitet die Lesezugriffe
* Updater
	* Ist zusätndig für die Lösung des dritten Problems der
	durch einsatz von Seaweed fs nicht gelöst werden konnte
	* Er kümmert sich darum das logisch zusammenhängende Buchungen 
	Buchungen die für ein Konto sind  in einem größeren FIle zusammengefasst
	werden
	* Er bekommt immer nur vom Backend mit, dass es neue Buchungen für 
	ein Konto gibt und verarbeitet diese

* Ein Account sieht wie folgt aus:
* Json rep
* Kann initiall mit Balance angelegt werden
* Eindeutig ist die Kombinatoin aus BIC und IBAN
* Ein Account kann beliebig viele Buchungen empfangen
* Wobei eine Buchung so aussieht
* Json Rep Buchung
* Anhand eine Buchung kann nicht herausgefunden werden, ob sie jetzt anhand
online Banking, Lastschrift  oder Überweisung entstanden ist
* Nur Intendes Use könnte darüber Auskunft geben
* Wichtig ist, bei jedem Zahlungsverkehr werden ansich immer zwei Buchungen 
erzeugt
* Nur jeweils gespiegelt
* Also sender und empfänger getauscht und der Betrag gespiegelt
* Diese Buchungen werden dann immer im entsprechenden Konto abgelegt
* Im Sinne der Doppelten Buchuführen und die beiden beträge ergeben immer 0
* Um das verteilen der Buchungen kümmert sich aber ein sogenanter 
Zahlungsverkehrsdienstleister \cite{online}
* Das heißt Buchungssystem muss sich nicht darum kümmern, ob und wie
die beiden Buchungen ankommen, sondern kann jede Buchung getrennt verarbeiten

* Der Aufbau der Anwendung ist in Abbildung bla zu sehen
* Backend liefert die Funktionalität um einen Account über schnittstelle 
anzulegen. Dann wird der account zum einen wie im Konzept beschrieben
in relationale Datenbank abgelegt, und es wird auch eine account info im dfs
angelegt das passiert unter dem pfad bic/iban/timestamp in nanoseconds
* Timestamp aufzeigen
* Dabei ist das erzeugen des accounts und der accountinfo eine Transaktion
* Kommt eines nicht durch, wird das ganze abgebrochen
* Account info sieht wie folgt aus, speichert neben bic, iban und balance auch 
noch die Transaktionen des kontos und das datum der ältesten und der neuesten
Transaktion
Auf Basis der Accountinfo wird auch der rest der Anwendung aufbauen
* Account info dient zum bündeln von Transaktionen
* Account kann einzelnen über bla/bla oder alle gemeinsam über /bla 
ausgelesen werden
* Jetzt ist grundlage für das EMpfangen von Buchungen getätigt
* Buchungen können über Schnittstelle bla/bla mit POSt angelegt werden
* Jede Buchung bearbeitet einmal die Kontostand Datenbank
* Und einmal legt sie die Buchung mit dem Zeitpunkt ihrer Erstellung in
Seaweed fs unter /book/bic/iban/timestamp ab
* zusätzlich teilt das Backend einem Updater mit,
welches konto sich geupdatet hat

* Dieser Übergang ermöglicht einen Blick auf den Account updater
* Dieser ist generell aufgeteilt in master und slave
* beim starten der Anwendung sagt man einfach welcher update server master ist
* den slaves gibt man dann einfach nur adresse des masters mit
* Jeder Update server ist in der age Update requests vom Backend zu empfangen
* Dabei wird das benötigte Update in eine Mongo db abgelegt
* Code dazu sieht so aus (listing)
* Nur der Master aber kann diese Updates auf alle Update server
und sich selbst verteilen
* Heißt alle 10 Sekunden schaut er sich alle zu updatenden
Konten an und Teilt jedem Update service einen anteil über eine schnittstelle mit
* Die Update server beginnen dann damit für die gegebenen Bic und iban zunächst 
die bestehende account info auszulesen
* und zwar die älteste über directory search möglich
* sie vergleichen die darin enthaltene time und id der neuesten Transaction
mit dem Datum aus der Update tabelle
* wenn das update nach der letzten Buchung in der account info ist, 
* zieht sich updater alle Buchungen im book nach dem letzten Datum in der account
info und fügt sie in die account info ein und speichert sie letztendlich 
ab mit angepassten daten darüber was die letzte darin enthaltene war.
* Übersteigt die Anzahl der Transaktionen in einer account info einen bestimmten
schwellen wert, so wird eine neue account info angelegt und begonnen zu befüllen
* Auch diese hat dann als id wieder das datum der ältesten Transaktion
* spielt man mit der anzahl der Buchungen in einer account info, kann man
kontrollieren wieviele account infos angelegt werden
* Es beeinflusst auch wieviele später gelesen werden müssen um Daten zu bekommen
* Und wie feingrenular der Zugriff ist
* Das durchführen der Updates nur alle 10 Sekunden und nicht bei jeder eingehnden
Buchung hat einige gründe
* Führt man für jede einzelne Buchung update durch,
muss für jede Buchung eine neue needle im Volume angelegt werden
* Seaweedfs unterstützt wie das Vorbild haystack kein anhängen von Daten
* Das heißt für jede Buchung muss in memory mapping angelegt werden
* Wenn man nur regelmäßig durchführt, kann man hingegen 
manchmal mehrere Buchungen zu einer account info hinzufügen und entlastet
* Volume Server master und fier
* Außerdem läuft es ingesamt schneller ab
* Für mehrere Buchungen die zwei Festplatten zugriffe anstatt für eine Buchung
* Man kann Last auch besser vorhersehen und hat gegebenenfalls noch zeit
* Neue Update server als Slaves hinzuzufügen um die Buchungen zu verarbeiten.
* Wenn account info durchgeführt wurde, wird aber update noch nicht gelöscht
* Um mögliche Datenverluste oder Fehler zu verhindern, wird der update eintrag  
* sowie die entsprechenden buchungen im Book ordner erst gelöscht, wenn 
* Bei einem zweiten Update vorgang festgestellt wurde, das keine neue Daten 
gefunden wurden
* Das liefert zusätzliche Sicherheit gerade bei konten die stark frequentiert 
werden
* Vielleicht Bild das das anzeugt
* sollte aus irgendwelchen gründen zwei update server gleiche konten
updaten, gewinnt der lezte update server
* sollte der aber nicht alle buchungen berücksichtigt haben
* würde er die Buchung löschen und das update wäre verloren
* Das ist aber dann so nicht der fall
* Wird auch durch einen zweiten update service geprüft
* Die Transaktionen liegen immer mehrere Updates rum, bevor sie nur noch in 
account info und seinen replikas gespeichert wird
* Konten die wenig Transaktionen durchführen werden dagegen schon beim zweiten
* Update durchlauf gelöscht


* ist ein slave während dem update modus nicht erreichbar, entfernt der master
ihn aus der liste
* Durch regelmäßige Heartbeats bleiben die slaves und master im kontakt

* Die Account infos sind jetzt also die gruppierung der Buchungen zu den 
Konten
* Dadurch, dass es aber kein index file innerhalb der account infos gibt,
muss immer komplette account info geladen werden um Buchungen zu finden
* Wenn der client nach seinen buhcungen für einen gewissen zeitraum fragt
* Also /bla/bla from oder auch alternativ noch mit to statement
* /bla/bla?to
* Dann wird der filer zunächst nach den files nach diesem Datum gefragt
* Im fall das keine Account info zurück kommt, weil alle buchungen in 
einer liegen, deren id weiter in der vergangenheit liegt
* Wird die latest zurückgeliefert und die entsprechenden Transaktionen
herausgefiltert und ausgeliefert
* Im Falle, dass mehrere zurückgeliefert werden , werden alle im angegeben 
bereich ausgelesen und die darin enthaltenen Transaktionen in einer
account info zusammengefasst
* Zu letzt wird noch die Transaktion die eins vor der ältesten an der
Anfrage beteiligten Account info lag angefragt und überprüft ob darin 
auch Daten lagen die interessant sind
* Wenn ja dann wird das entsprechend hinzugefügt zu Anfrage


* Durch die Verzögerung bei updater kann es sein, das kurze inkonsistenzen 
auftreten
* Der Kontostand kann aber immer exakt über die Kontoführungs db ermittelt
werden

* Beispiel für das schreiben einer Buchung
* Buchung kommt an
* Wird in DB angepasst
* Wenn nicht existiert error
* Ansonsten wird buchung in books/bic/iban abgelegt
* und beliebigen update server bescheid gesagt, was da gerade
passiert ist
* Dann erhält client antwort ob das ganze gepasst hat

* Nach einiger zeit wird dann update angetreten, der alle Buchungen
nach letzter buchung in account info zusammen fasst

* Wie läuft das lesen ab
* Client gibt from oder auch to an
* Backend sammelt alle Accountinfos für entsprechenden zeitraum zusammen
* Gibt diese dem client zurück

**Vielleicht in neuen Kapitel**
* Spielraum in der Performance lässt die Anzahl der Buchungen pro
Account info und die Dauer zwischen den Updates und die Anzahl der
Updateserver

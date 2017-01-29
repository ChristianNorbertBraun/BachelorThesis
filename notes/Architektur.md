Backend
=======
1. Empfängt Client Anfragen zum Lesen und Schreiben von Postings
1. Validiert empfangenes Posting
	* Existiert Kontonummer
	* Ist Betrag gültig
1. Behandelt read zugriffe
	* Liest mehrere Dateien aus und gibt kombiniertes Ergebnis zurück
1. Einzige Komponente, die mit dem client direkt spricht. Alles andere befindet sich hinter einer Firewall und kann
nur innerhalb des gleichen Netzes erreicht werden.

Journal(Herzstück der Anwendung)
================================
1. Hält alle eingegangenen und validierten Events
1. Arbeitet Events von oben nach unten ab
1. Markiert abgearbeitete events
1. Muss auch bei Absturz wieder alten Zustand herstellen können
1. Beinhaltet auch Update Befehle??? (Fragwürdig)
	* Sollte eigentlich in extra Liste eingetragen werden (Command Liste oder ähnliches)

## Ablauf
1. Neues Event trifft bei Journal ein
1. Event wird in Journal Liste eingeordnet
1. Entscheidung ob auch ein Update Befehl eingefügt werden muss
1. Nach bestimmter Zeit oder wenn zu viele Events vorhanden sind wird Journal geleert
1. Nach dem ein Event geschrieben wurde, wird es aus Journal entfernt
1. Stürzt das System ab während das Journal arbeitet, wird vom letzten unbearbeiteten Punkt fortgeführt.

* Journal kümmert sich um das abspeichern der Postings aber nicht um das updaten 
* Dazu gibt es den Updateservice.
* Diesem gibt man die upzudatene Kontonummer mit


Updateservice
=============
* Erhält Aufgabe für eine gegebene Kontonummer die Kontodatei anzupassen
* Dabei fragt er die letzten Postings der besagten Kontonummer ab und fügt diese mit angebenenen Datum in die Kontodatei

Saldo Tabelle
=============
1. Beinhaltet für alle Konten das entsprechende Saldo
1. Wird auch verwendet, um zu prüfen ob Konto existiert


Abhängigkeiten
==============
* Backend benötigt Journal
* Backend benötigt Saldo Tabelle

* Journal benötigt Updateservice
* Saldo Tabelle ist unabhängig

Probleme
========
* Alle Services müssen genau wissen, wie das DFS aufgebaut ist
	* Entweder über Config Datei lösen oder Abstraktions lib schreiben
	* Frage ist, was muss in diese Lib rein
* Kümmert sich das Backend um das einfügen eines Postings in das Journal und den Updateservice?
	* Das würde bedeuten, dass der gesamte Prozess gelockt werden muss
	* Aber sowohl das Journal als auch der Updateservice müsste sich nur um einfache Dinge kümmern
* Alternativ könnte das Journal die Update requests erzeugen und mit dem Updateservice kommunizieren
	* Ist von außen schlechter nachvollziehbar und macht Journal abhängig
	* Auf der anderen Seite ist so eine sinnvolle Priorisierung der Updates möglich (Wenn mehrere Writes einer Kontonummer im Journal 
	muss Update danach kommen)
* Realisiere ich das alles als Microservices oder als ein mächtiges Backend?

## Pro MS
* Journal sowie Update Service können separat skalieren
* trifft daher sehr gut die Anforderungen der BA
* Auch Ausfallsicherheit von Journal, Backend und Updateservice separat testbar

## Pro Monolith
* Einfacherer Deploy
* schnellere Responsetime
* Für den Prototypen wahrscheinlich leichter
* Ausfallsicherheit für mich leichter zu testen
* leichter zu debuggen


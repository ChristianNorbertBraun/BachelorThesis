Evaluation
==========
* Einige der Designentscheidungen im Konzept und der Implementierung
* sind auf grund von Annahmen getroffen worden
* Welche Leistung das System tatsächlich bringt soll hier ermittelt 
werden
* Die Punkte die beachtet werden sind
* Interoperabilität
* Skalierbarkeit
* Performanz
* Ausfallsicherheit
* Wirtschaftlichkeit

* Es kann nur vergleichend vorgegangen werden zu aktuellen Systemen
was die Leistung der Persistenzschicht angeht
* Also relationale Datenbank
* Genaue Details zum Buchungssystem sind nicht bekannt

* Interoperaibilität
	* durch Go kann es auf viele Systeme kompiliert und eigesetzt werden
	* Nur das setzen einer Flag nötig
	* Auch SeaweedFS ist für viele Systeme vorhanden
	* Die Schnittstellen von Booking Systemen sind nicht genau bekannt
	* Wenn die entsprechend eingebetten werden, sollte
	leicht integreierbar sein
	* Je Nachdem wie die aktuelle Imlementierung ist, könnte Schnittstelle
	zu DFS auch weiter abstrahiert werden und einfach unter bestehends
	Buchungssystem geschnallt werden
	* Der Einyatz des DFS ist nicht ganz so gut
	* Junge Technologie, Banken kennen sich nicht damit aus
	* Auch das sich booking System und account updater DFS teilen
	macht es schwer austauschbar
	* Der Einsatz des RDBMS ist banken schon bekannt und daher leicht integrierbar
	* Vielleicht gibt es sogar schon Datenbank die Konten mit Kontostand hält

* Skalierbarkeit
	* Die Möglichkeiten der Skalierung eines DFS wurden ausgiebig besprochen
	* Master über mehr Ram, und Speicher über mehr OSDs
	* Auch das ist wieder mit SeaweedFs möglich
	* Einfach nur neue Store Machines starten 
	* Der Master stellt jedoch einen SIngle Point of Failure dar
	* Deswegen sollten shadow master hochgefahren werden
	* Das wird auch durch seaweed FS unterstützt
	* Desweitern ist skalierbar durch das starten mehrerer
	DFS
	* Das Backend ist beliebig skalierbar
	* Sogar nach Last theoretisch
	* Unabhängig von Datenbank und DFS
	* Account Updater sogar nich leichter
	* Benötigt keine Schnittstelle bzw.
	* Wird von keinem Service direkt angesprochen
	* Können einfach master mehrere slaves hochgefahren werden
	* Problem ist aber, dass wenn master ausfält die updates nicht
	durchgeführt werden bis neuer master da und neu konfiguriert
	* Das kann zu downtime führen
	* Aber Buchungen sind deshalb nicht verloren und 
	Buchungssystem bleibt weiterhin erreichbar
	* Buhcungen werden nur nicht zu account infos zusammengefasst
	* Das RDBMS lässt sich wie beschrieben nur begrenzt skalieren
	* Deswgen wurde darauf geachtet, dass nur wenige Daten darauf liegen
	* Die Buchungen werden regelmäßig aufgeräumt um benötigten Speicher 
	gering zu halten
	* Ansonsten sind Banken ja schin vertraut mit der Skalierung von RDBMS


* Performanz
	* Die Performanz des Systems wurde auf zwei mac minis getestet
	* mit den und den stacks
	* auf einem lief ein backend und ein account updater sowie postgres
	* auf anderem lief filer

	* Bedingungen erklären, 10 sekunden volle last

	* Daten zu lese
	* Account anlegen
	* und noch mehr
	* SeaweedFS Benchmark

	* Gegensatz zu system nur auf einem Mac mit postgres
	* Daten zu lese schreib und schreib

* Ausfallsicherheit
	* SeaweedFS kann mit beliebieger replikation gestartet werden
	* Auch über Datencenter hinweg ist das möglich
	* Und es wird von SeaweedFS aufgepasst, das Replikation auch entsprechen
	weiterhin existiert
	* Das heißt je nach Art der Replikation können mehrere Server , Racks oder
	sogar Datenzentren Versagen, bevor Daten verloren gehen
	* Dabei wird noch nicht betrachtet, welche Art zum Speichern genutzt wird
	* Also zum Beispiel RAID 10 etc
	* Sollte SeaweedFS mal nicht erreichbar sein,
	* Kann ads System auch komplett ohne DFS arbeiten
	* Nur auf Basis der RDBMS
	* Die Ausfallsicherheit wird also durch das RDBMS begrenzt
	* Ähnlich wie beim aktuellen Buchungssystem
	* Nur das deutlich weniger Daten verwaltet werden
	* und Operationen leichtgewichtiger sind


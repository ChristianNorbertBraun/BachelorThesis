Distributed File System als Grundlage eines Buchungssystems
===========================================================

* Wie bereits erwähnt kommen Skalierungs und Kosten Probleme auch durch die
ACID basierten Systeme
* Dabei wird die Funktionalität von ACID vll. nur für einen Bruchteil der
Operationen wirklich benötigt
* Trotzdem leidet die Skalierbarkeit und die Kossteneffizienz des ganzen
Systems darunter \cite{salt}
* Systeme die die Konsistenz gegen der Skalierbarkeit und Verfügbarkeit 
tauschen funktionieren nach dem BASE Prinzip 
* Diese sind zusätzlich auch häufig billiger als ACID SPeicher möglichkeiten
* In diesem Kapitel soll zunächt die Bedeutung von BASE erklärt werden
* Auch die Nachteile und Vorteile
* besonders bezogen auf Konsistenz
* Es gibt viele BASE basierte Systeme
* Skalierbarkeit und Verfügbarkeit Ausfallsicherheit , Kosteneffizienz ist
Königsdisziplin von Distributed File Systeme
* Wie einige Vertreter das machen soll erklärt werden
* Auf Vorteile und Grenzen von DFS eingehen
* Wo kann ein DFS innerhalb einer Bank eingesetzt werden?
* Was sind die Kriterien dafür?

## BASE
* Auf der einen Seite ist ACID bekannt
* Bringt sehr Starke Semantik mit ist aber auch gerade in verteilten Systemen
sehr komplex und bringt hohe Kosten mit sich
* Denn ACID gibt keine Garantie für Verfügbarkeit, lieber keine Antwort, als
eine falsche Antwort \cite{Cluster Base Scalable Network}
* Das macht Skalierung solcher Systeme schwer
* Wenn ein Teilsystem ausfällt ist möglicherweise gesamtes System nicht
erreichbar \cite{BASE}
* Ein System, dass Verfügbarkeit, Skalierbarkeit und Konsistentenz 
gleichermaßen erfüllt kann es jedoch nicht geben \cite{CAP}
* CAP Theorem bestätigt das in verteilten Systemen \cite{CAP}
* Parition Tolerance ist für jedes System zur Skalierung notwendig \cite{BASE}
* Deswegen Entscheidung zwischen Verfügbarkeit und Konsistenz
* Auf Konsistenz zu schrauben hört sich immer schlimm an
* Es ist wichtig zu verstehen, das man nicht nur 2 von den 3 CAP eigenschaften
haben kann
* Man nur in einem gewissen maße auf eines verzichen \cite{CAP}
* Es gibt mehr als 50 unterschiedliche Arten von Konsistenz \cite{consistenz} S.1-9
* Anfang der 80iger nur Starke Konsistenz bekannt
* Heute aber immer mehr im SInne der Scaleable und Availible Systems
* Einmal die Starke Konsistenz ist das bekannteste und der GOLD Standard
* Jede Request muss sofort zwischen Request und Response durchgeführt werden
* Jeder READ muss den tatsächlich letzen geschrieben Wert zurückliefern
* Auf der anderen Seite Weak and Eventual consistency
* Weak: Read gibt nicht die letzten geschriebenen Werte zurück auch die 
reihenfolge in der sie duchgeführt werden wird nicht garantiert
* Reads nacheinander müssen auch nicht immer die gleichen Werte zurückgeben
* Eventual Consistency ist da etwas stärker
* Wenn kein Update in einer gewissen Zeit passiert ist, liefern reads immer das
gleiche zurück
* Irgendwann hat man sicher konsistenten stand Wenn kein write
* Eventual Konsistency in Blck auf verteilste Systeme kann man über folgende
Eigenschaften definieren:
* Eventual delivery
* Convergence
* Termination \cite{consistenz}

* Base Systeme arbeiten lockern die starke Konsistenz auf und arbeiten mit der
eventual Consistency. Base steht deswegen auch für:
\cite{Cluster Based blabla}
* Base bedeutet Basically Availible Soft state eventual consistency
* Wo ACID Pessimistisch ist und Konsistenz am Ende fordert
* Ist Base optimistisch und will nur, dass Irgendwann eine Konsistenz eintritt
* Base geht davon aus, dass für den Nutzer vll Konsistenz gar nicht das 
wichtigste ist sondern eine extrem hohe verfügbarkeit
* Dabei schafft BASE raum für Skalierung und Performance Steigerung
* Kommunikation die in ACID Systemen zwingend nötig für Konsistenz sind
* Kann in Base vernachlässigt oder auf einen besser passenden Zeitpunkt 
verschoben werden \cite{Cluster}
## Funktionsweise
* Distributed File Systeme arbeiten eben nach genau diesem BASE prinzip
* Sie geben strikte Konsistenz für sehr gute Skalierbarkeit, Verfügabrkeit und
Kosten auf
* Deswegen werden DFS viel von Firmen eingesetzt um mit Big Data umzugehen
* Beispiele sind Google mit Google File System, Facebook mit Haystack, Ceph von 
Deutsche Telekom, Cern und Cisco und Hadoop Distributed File System von Yahoo
und zahlreichen anderen Firmen. \cite{ceph}

* Sie alle arbeiten auf Basis von Object Storage \cite{ceph}
* Im Gegensatz zu  Normale Filesysteme nutzen File Storage
* Files und Metadaten getrennt in Object Storage aber nicht
* Hier werden Files mit den ugehörigen Metadaten gemeinsam abgespeichert
* Der Nutzer greift meistens nur über eine sehr Abstrakte Schnitstelle wie http
auf die Daten zu.
* Nutzer eines Object stores arbeiten auf Objekten, lesen, schreiben löschen
* Geben aber nicht an, wie und wo es landen soll
* Das Speicher Management ist dabei die Aufgabe der sogenanten Object Storage
Devices (OSDs)
* Es müssen nur noch die OSDs verwaltet werden, die OSDs sind dann selbstständig
für ihre Dateien zuständig
* Dadurch das die jeweiligen OSDs die Dateien inklusive Metadaten verwalten,
lassen sich diese Systeme sehr gut skalieren
* haben deswegen hohe capacity
* Scalability
* Eventual consistency
* Web accessibility
* Weit geografisch verteilt \cite{Video oS}
**Gar nicht relevant vielleicht**
* Dann gibt es noch Block Storage, so funktionieren Datenbanken
* Hier werden Files in Datenblöcke zerteilt, welche indiziert werden
* Die einzelnen Blöcke kennen keine Metadaten sondern nur die Adresse ist 
bekannt
* Das ist gut für schnellen Zugriff auf Blöcke und random access
* Aber der Nutzer muss genau wissen welche Blöcke welche Bedeutung haben
* Block verwaltung auch wieder Aufwendig
**Bis hierhin**
* Object Storage systeme bestehen immer in irgendeiner Form aus OSD und aus
Metadata Servern für die OSDs
* Um einen bessern Einblick in die Funktionsweise der DFS zu gewähren sollen im
Folgenden zwei DFS grob vorgestellt werden. Zunächst das Google File System, 
und dann Facebooks haystack
* Google File System GFS wurde gebaut um den extrem schnell steigenden
Daten verarbeitungs bedürfnissen von Google Herr zu werden
* Hauptziel ist natürlich wie bei jedem DFS Scalability, availability, 
reliability und performance. Es wurde jedoch für die Bedürfnisse von Google 
angepasst
* Ausfälle sind die Regel und nicht die Ausnahme
* Einmal geschrieben werden Daten nur noch gelesen
* Man kann an Dateien appenden
* Es muss einige millionen von Files von 100 mb und größer speichern
* Es gibt sehr sehr große Dateien teilweise mehrere GB groß
* Demnach gibt es auch sehr große writes
* und auch große gestreamte reads
* Kleine reads und writes müssen nicht so effizient sein
* Das System muss mit sehr vielen Clients umgehen können
* UNd auch damit, wenn mehrere clients auf ads gleiche zugreifen wollen
* Zum Beispiel Append
* GFS bietet ähnliche Funktionen wie ein standard File System bildet aber nicht
alle Bedingungen des POSIX standards ab.
* Daten werden aber in Ordnern und pfaden abgelegt
* Es gibt Operationen wie open, close, read und write
* zusätzlich gibt es eben den snapshot und den atomic append
* Generell besteht das GFS aus zwei Bestandteilen
* Einem einzigen Master und mehreren Chunkservern
* Dateien werden in chunks verteilt 64 mb größe 
* Clients reden mit master und fragen danach wie file gelesen wird und
* reden dann direkt mit chunk servern um diese Daten zu erhalten
* Single Master vereinfacht das design und erlaubt ein hoch performantes
chunk placement
* Er speichert drei arten von metadata
	* namespace von file und chunks
	* mapping von chunk to file
	* location von jedem chunk und replica
* Das alles in memory und auch persistent
* speichert für jeden 64 mb chunk nur 64 byte metadata
* Zusätzlich speichert operation log alle durchgeführten änderungen an
metadata
* fungiert als timeline
* Werden über eindeutigen Zeitstempel indentifiziert
* mit ihm kann immer wieder ein konsistenter zustand im fehlerfall hergestellt
werden
* Deswegen muss hier auch immer alles sich drin stehen
* Hier wird wirklich gewartet bis request durch ist, bevor der client antwort
bekommt
* Damit der master aber nicht zum bottleneck wird, muss einiges getan werden
* Zum beispiel wird kein write und read durch den master direkt durcgeführt
* Ablauf bei einem READ
* Der Client weiß welchen Teil von Datei er lesen möchte
* daraus berechnet er den index des chunks größe ist ja bekannt
* Er Fragt den Master dann gemeinsam mit dem namen und dem chunk
* der master gibt ihm den ort des chunks zurück und den ort aller replicas
* der client fragt den chunkserver an am besten den nähsten 
* der chunkserver sendet daten
* Weitere reads der gleichen Datei erfordern keinen zugriff auf master mehr
* Chunk position ändern sich in der regel nicht
* Der client kann diese also einfach cachen
* Wie läuft ein write ab
* Um sicher zustellen, dass alle Replicas eines chunks irgendwann indetisch sind
ist es wichtig, dass änderungen auf den Daten zumindest in einer gleichen 
reihenfolge durchgeführt werden
* Dazu nutzt das google fs Leases
* Möchte ein client schreiben fordert er ein lease für einen chunk an
* wenn frei bekommt er es
* Leases dauern in der regel 60 sekunden
* zusätzlich zu lease gibts auch noch die ganzen replicas und ein primary chunk
* client schickt daten zu allen replicas chunkserver streamen die auch selbst
weiter
* reihenfolge ist egal
* Wenn alle die Daten empfangen haben sendet client an primaary die metadaten
für die gesendeten daten
* Primary kümmert sich dan darum, dass alle die metadaten haben
* erst dann werden die Daten auf allen replikas geschrieben
* Die seconderies sagen dem primary wenn sie fertig sind
* der primary sagt das dem client
* Wenn lease noch gültig ist, muss client master nicht mehr fragen auch das ist
für entkoppelung
* Besonderer fall wenn lease schon ausgegeben
* zweiter client will gleiche datei schreiben
* greift auf gleiches lease zu damit gleiche primary
* schickt daten zu replicas
* schickt operation zu primary
* primary kümmert sich darum, dass operation in richtiger reihenfolge bei allen
replikas ankommt
* erst dann wird geschrieben
* das sorgt dafür, dass selbst mehrere operationen nur einen zugriff auf 
platte erfordern
* und das die daten auf jeden fall irgendwann konsistent sind

* Wie breits erwähnt, werden alle daten chunks mehrfach abgelegt
* Im default wert sind das 3 ma pro chunk
* für maximale ausfallsicherheit werden diese chunks häufig sogar geopgraphisch
verteilt oder in anderen racks
* Das hat neben der Fehler toleranz auch noch den vorteil, dass clients
beim lesen immer den nähsten chunk lesen können
* Oder wenn mehrere den gleichen chunk lesen wollen sie nicht auf den 
tatsächlich selben zugreifen müssen
* dadurch werden hotspots vermieden
* Google FS hat auch einige prozess eum sicherzustellen, dass immer die gleiche
Anzahl an replixas existiert

* um das problem mit dem Single Master zu lösen gibt es auch immer einen shadow 
master, der immer nach einer kurzen Zeit die oerationen des masters übernimmt
* Inkonsistenzen herschen also bis ein lease abgelaufen ist und vielleicht
wenn ein master ausfällt bis der andere über log wieder auf richtigen stand ist

* Haystack 
* haystack wird bei facebook zum speichern von Fotos eingesetzt
* Handlet bei Facebook etwa 20 petabytes an daten
* und liefert über 1mio bilder pro sekunde zu den peaks aus
* Hauptziel ist auch hier metadaten so gering wie möglich um lookups in main
memory zu machen
* In den annahmen sehr ähnlich
* viele clients
* viele Daten
* daten werden einmal geschrieben und oft gelesen
* Fehler sind die Regel und nicht die ausnahme
* Jedoch werden metadaten für Daten noch weiter reduziert
* POSIX systeme halten für Ordner und pro File Metadaten
* Das lesen dieser metadaten kann zum flaschenhals werden
* Deswegen werden metadaten für mehrere files gebündelt
* Files werden in sogenannten Volumes aufbewart
* Ein volume ist quasi ein großes leeres file, welches nach und nach mit bildern
befüllt werden kann
* 10 tb kann zum beispiel in 100 100 gb volumes aufgeteilt werden

* Besteht generell aus drei elementen
* Auch hier ist metadata getrennt von tatsächlichen Daten
* Besteht aus directory, store, cache
* Haystack directory
* Directory verwaltet dann nur noch die volumes 
* Um replikation zu vereinfachen werden diese phyischen volumes auch nochmal
in logische Volumes gekapselt
* Dateien können so einfach zu einem logischen volume geschrieben werden
* Und werden dann zu allen darin enthaltenen volumes geschrieben
* Damit wird ausfallsicherheit erzeugt
* Das directory kümmert sich also um das mapping von logical zu physical volumes
* Leitet writes zu logical und reads zu physical volumes um
* Setzt volumes auf read only wenn sie zum Beispiel voll gelaufen sind

* haystack cache
* Cached die DAten, die haystack verlassen
* Entlastet maschinen, die gerade geschrieben worden sind
* Neue fotos werden häufiger angefragt als alte

* haystack store
* Lese anfragen erhalten vom master logical volume und die store machine auf der
sie laufen 
* Jede store maschine managed mehrere volumes
* Jedes volume kann millionen von Bildern halten
* Aufbau eines volumes ist superblock und needles
* Superblock enthält metadate für das volume file
* Zugriff erfolgt einfach über das volume un den offset innerhalb des volumes 
und größe
* Bild eines volumes
* Ein store hat immer alle keys für jedes volume im speicher
daher ist Zugriff schnell
* Diese Info kann immer durch ein volume file wiederhergestellt werden
* Alternativ könnte man ja auch einfach immer das gesamte ding lesen

* Wie läuft photo Read ab
* logical volume key und cookie schicken
* cookie ist random number um vor random zugriffen zu schützen
* directory gibt client konkrete url von store
* Client schickt anfrage an store
* store kann in memory nachschauen was er darüber weiß
* offset suchen und schickt bild zurücl
* Client kann auch wissen wo sich das direkt befindet
* Kann auch aus cache beantwortet werden

* wie läuft write ab
* Server fragt nach write enabled logical volume nach
* Bekommt das mit mapping zu physical volumes
* client schreibt synchron zu allen volumes
* bild einfügen upload
* Wird file mit mit gleicher id geschrieben, wird metadata eintrag im
store gändert und logical volume im directory
* Im gleichen volume gilt immer die datei mit dem größten offset als neuste

* Dadurch und duch fehler können natürlich auch immer wieder korrupte Needles
entstehen
* Deswegen wird hin und wieder aufgeräumt
* Needles in neues file kopieren und korrupte rauswerfen
* volume ersetzen
* Insgesamt braucht es für 4 bilder 40 bytes metadaten im gegensatz zu 536 in
gewöhnlichen file system
* Außerdem werden uploads zusammengeführt wenn möglich
* Disk kann besser große dateien schreiben

* unterschiede zu google fs
* Dateien werden nicht in chunks geteilt
* Mehrere bilder landen im gleichen file
* Dateien sind etwas kleiner als bei GFS
* Es gibt einen Cache zwischen directory und store
* noch weniger metadata angespeichert
* Im idealfall nur o(1) zugriffe auf die Platte vielleicht bei metadata neuer
Zugriff
* GFS hat größere reads
* Haystack hat keine ordner und pfade ist gar nicht POSIX komform

## Distributed File System gegen relationale Datenbanken
* Die stärken der DFS sind klar
* Skalierbarkeit, ausfallsicherheit kostennutzen
* Haystack zum beispiel mit o(1) zugriffen vielleciht sogar performance
* Trotzdem habe diese Systeme auch einige Nachteile und grenzen

* Konsistenz ist ein Problem
* Meistens nur mit relativ großen Files arbeiten
* Sie arbeiten mit files und nicht mit strukturierten Daten wie zum Beispiel
eine Datenbank
* Sie sind eigentlich gedacht um Videos, große Textdateien etc. zu speichern
* Kann so ein system überhaupt auf die Transaktionen einer Bank gebracht werden
* Und ist die konsistenz nicht ein Killer von all dem?

## Vorteile und Nachteile

* Viele auf große Files ausgelegt
* Immer sehr spezifischer Anwendungsfall
* Billig
* Gut Skalierbar
* Sehr gute Performance
* Können sehr viele Daten handeln
* Nehmen Komplexität vom Client
* Dynamisch skalierbar

## Anwendungsbereich
* Aufgrund der genannten Nachteile kommen nur Bereiche in Frage, die in den 
Händen der Banken liegen. 
* Eventuelle Konsistenz kann gegeben sein
* System muss natürlich zu einem vorhersehbaren Punkt konsistent sein
* Muss vom Kunden verziehn werden können


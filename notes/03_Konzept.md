Konzept
=======

* Die kleinen files sind schonmal ein Problem aber noch schlimmer scheint
die Konsistenz zu sein, gerade bei einem Buchungssystem unvorstellbar ohne
* Wenn ein Konto unter 0 geraten kann, weil man nicht anfragen kann, was der
Kontostand davor ist
* Der einfachste Schritt wäre demnach einfach ein read only DFS als kopie
um die Systeme der Banken noch etwas länger leben zu lassen
* Bei einem Read ist die konsistenz nicht ganz so wichtig
* Und keine Gefahr das Konto runter geht
* Das würde den Druck erstmal ein bisschen von den Systemen der Bank nehmen
* Langfristig ist es aber keine Lösung denn die Datenmenge wird für die
relationale Datenbank irgenwann nicht mehr handhabbar
* Deswegen soll hier ein Konzet entwickelt werden, das aufzeigt wie und wo ein
DFS eingesezt werden kann um die Buchungssysteme zu entlasten und neuen Raum für
Skalierung und Performance zu schaffen

## Brauchen wir die Konsistenz
* Um das Problem mit den kleinen Dateien müssen wir uns auf jeden Fall kümmern
Aber ist das mit der Konsistenz genauso? 
* Wir sprechen von Inkonsistenzzeiten von vielleicht 10 Sekunden bis zu einer 
Minute
* Brauchen denn alle Kontenarten überhaupt immer eine 100 prozentige Konsistenz
* Eigentlich doch nur alle Konten die regelmäßig Debit sowie Kredit empfangen
* Und ein vorgeschriebens Saldo nicht unterschreiten dürfen
* Zum Beispiel ein girokonto darf eigentlich nicht unter 0 gehen oder wenn
Dispo unter eine bestimmte Grenze 
* Problem ohne Konsistenz, Beim autorisieren der Bezahlung kann nicht
sichergestellt werden, ob tatsächlicher Betrag drin steht man weiß aber nicht 
wann Zahlungsanträge eingehen und kann sich von der Konsistenz her darauf
vorbereiten
* Gäbe es einen beliebigen Dispo, wäre das alles ok
* Das würde aber die Verantwortung in die Hände der Kunden legen
* Das sie nichts kaufen, was sie nicht bezahlen können, und Schulden in Kauf
nehmen
* Sicherheitskritische Probleme wie unberechtigte Zahlungsinitiationen sollen
im Rahmen dieser Arbeit nicht berücksichtigt werden
* Andere Konten, die kein problem mit einer Inkonsistenz hätten wären
* Konten auf die nur etwas überwiesen wird
* Oder nur eine einzige Abbuchung am Ende Kontrolliert!
* Und das bringt uns zu vielen Konten bei denen Banken die Verantwortung tragen
* Passivgeschäfte wären zum Beispiel die Termineinlage
* Da passiert zunächst nicht viel, es wird höchstens nochmal geld drauf gelegt
* Und die Abbuchung zur Verwendung liegt in den Händen der Bank
* Wenn aufgelöst, gibt es eine Abbuchung an den Kunden mit den Erträgen
* Der Zeitpunkt zudem es Knsistent sein muss ist also klar und damit kann
Kalkuliert werden
* Bild für Termineinlagen in Deutschland
* Auch Konten die Aktivgeschäfte abbilden brauchen nicht immer Konsistenz
* Ratenkredit
	* Girokonte erhält Geld das kann man nichtabbilden
	* Aber Konto bei Bank, bei dem Raten gezahlt werden, ist optimal
	* Da geht regelmäßig Geld drauf aber nichts runter 
	* Für jeden Kredit der gegeben wird kann ein Konto mit einem DFS abgebildet
	werden
* Es gibt also Konten, die mit einem DFS abbildbar wären
* DFS sol best möglichste Leistung für diese Kontoarten liefern
* Aber auch mit den Potential auch Skalierungsanforderungen für andere 
Kontoarten abzudecken, sofern man die Konsistenz in den Griff bekommt

## Annahmen 
* Um herauszufinden, was das System benötigt sollen nochmal Annahmen aufgezählt
werden
* Daher auch immer Verfügbar sein um alles abzubilden. System läuft nicht ist 
Geldverlust für Banken und Image schaden
* Das System muss mit sehr vielen Anfragen klar kommen von sehr vielen Clients
* Anzahl Konten, Anzahl Vertriebswege. Anzahl Transaktionen
* Wobei Deutlich mehr lese als schreibzugriffe
* Buchungen werden einmal geschrieben und nie wieder verändert
* Auch konkurrierende Zugriffe müssen berücksichtigt werden
* Wenn zwei Leute vom gleichen Konto abheben woll, Lastschrift zum Beispiel
oder Dauerauftrag
* Die Dateien müssen sehr sicher sein, dürfen nicht verloren gehen
Auch hier wieder imageschaden der bank
* Die Datenmenge einer einzelnen Buchung ist sehr klein
* Insgesamt muss das System aber sehr viele Daten speichern können wie anfangs 
gesehen 20Petabyte????
* Random seeks sind selten häufig werden Daten Zeitlich gesehen abgefragt
* Und viele Daten auf einmal ausgelesen
* Eher nicht eine konkrete Transaktion
* Also alle Transaktinen in einem Monat etc.
* Inkonsistenzen müssen sehr gering gehalten werden  
* Fehler sind an der Tagesordnung systeme dieser größenordnung haben ausfälle
* Sowohl GFS als auch Haystack erfüllen nicht alle Annahmen
* GFS bedient nur sehr große Dateien und hat teilweise lange inkonsistenzen
* braucht 64 byte für einen chunk im Hauptspeicher bei 16 gb hauptspeicher
des masters können so nur 250 Millionen Transaktionen abgefangen werden.
* geht man von einer transaktions größe von 1kb aus
* 255 byte User, 255 Byte schuldner, 128 Byte Iban Bic Schuldner, Gläubiger + 
255 Byte intended use, bytes für Betrag und Daten
* Das würde einem Speicher von 250 GB entsprechen
* Dateien in Haystack sind kleiner einge Kilobyte aber nutzer muss immer genau
wissen welche er haben will
* Er will nie mehrere Bilder nacheinander auslesen
* Das Streamen mehrerer Bilder wird also nicht unterstützt
* Der Cache ist aber fraglich weil Transaktionen ja immer nur von einem nutzer
eingesehen werden können
* Insgesamt liegt Haystack aber wegen der besseren Ausnutzung des Memorys
des masters und der kleineren Datei größe näher am Konzept
## Das DFS
* Was wir jetzt also bauen wollen ist ein System, das kleine Files unterstützt
und das auslesen von diesem am Block ermöglicht auch ohne die genaue ID zu 
kennen
* Auch Konsistenz soll angesprochen werden, und die Zeit für Inkonsistenz
soll möglichst gering sein
* Um das zu erreichen werden einige Ansätze von Haystack und GFS kombiniert 
werden
* Außerdem werden lösungsvorschöäge aus einigen Artikeln genutzt 
Daüfr gibt es mehrere Ansätze die in vielen Artikeln vorgestellt werden.
* \cite{improving, reducing, a novel approach, an optimization}
* Generell besteht das System auch aus einem Master und vielen OSDs
* Im ersten schritt wird betrachtet, dass logisch zusammenhängende Dateien auch
gemeinsam ausgelesen werden.
* Logisch zusammenhängend heißt gemeinsamer Papa
* In normaler Ordnerstuktur könnte das was sein wie Bilder oder Alben
* Die zusammen häufig aufgerufen werden
* So zusammenhängende Daten sollen in ein großes File gemerged werden
* Ähnlich wie volume bei Haystack oder chunk bei gfs
* Verwirrung vermeiden hier bucket genannt
* Bei uns macht eine grße von 16 mb sinn, da dateien ansich sehr klein sind
* bei einer größe von 1kb pro Transaktion könnten darin schon 16000
Transaktionen gespeichert werden um das in einem Jahr vollzubekommen
müsste man über 43 Buchungen am Tag machen
* Breite Masse abdecken amazon ist was anderes
* Zusätzlich muss master nur das große File verwalten und nicht jeden einzelnen
* Wenn eine Datei davon ausgelesen wird, werden die zusammenhängenden schon
mit in den Speicher geladen und können mit ausgeliefert werden
* Für viele Anfragen brauchen wir nur eine Disk operation
* Er muss nur wissen, welche Daten sich darin befinden können
* Nur wie soll der Master wissen, welche Dateien sich in einem solchen
Bucket befinden
* Eine streng aufsteiigende ID für jede hochgeladene Datei
* Die ID entspricht einem Timestamp der auf Nanosekunden genau ist 8 byte groß
* Jeder Bucket erhält als ID die älteste im Bucket enthaltene Datei
* Für logisch zusaenhängende Dateien wird immer zuerst ein Bucket aufgefüllt 
bevor ein neuer angelegt wird
* Bei einer anfrage mit einer gegebenen ID muss nur der Bucket gefunden werden
desse ID vor der angefragten ID liegt oder genau der angefragten ID entspricht
* Die Daten dieses Buckets können dann an den anfragenden gestreamt werden
* Damit aber nicht immer der gesamte Bucket gelesen werden muss um antwort zu 
liefern und auch nicht die gesamte Datenmenge übertragen wird
* Gibt es wie bei Haystack ein bucket index file
* Da wird nicht jeder Eintrag festgehalten sondern immer nur wenn eine gewisse
Füllmenge im Bucket erreicht wurde
* Das ermöglicht das ein OSD sehr viele Buckets halten kann und im Memory halten
kann
* 8 byte pro schlüssel plus 3 byte zum allokieren des offsets
* Größe muss nicht gespeichert werden, da jeder eintrag mit seiner id und 
seiner größe in der Datei abgelegt wird. 
* Es wird also von einem Checkpoint aus gestartet, id und größe gelesen,
bis zum nächsten checkpoint
* da wieder id und größe gelesen
* Bis man bei der gewünschten ID angekommen ist
* Es kann nicht beliebig gesprungen werden sondern nur von festen Checkpoints
angefangen werden
* Auch wenn das besser für eien READ einer einzelnen Datei ist, als wenn 
gesamter buccket gelesen werden muss immer noch scheiße
* Für das lesen größerer Blöcke ist das aber nicht so wichtig und ausreichend
* Bei den Transaktionen scheint eine Bucket größe von 16 mb ausreichend,
die 16 mal unterrteilt ist
* Es müssen maximal 1000 Einträge gelesen werden um auf genau den Punkt zu 
springen wo man hin will
* Nur wie findet man jetzt die logisch zusammenhängenden Files überhaupt raus
* Dazu dient der erste Teil der ID einer jeden transaktion
* Im fall von Buchungen wäre das BIC + IBAN
* Man kann sich das wie einen Ordner vorstellen
* Der erste Teil der ID bestimmt welche Dateien zusammengehören
* Alle IDs die die gleiche BIC und IBAn haben landen im gleichen Bucket
* Aber viel funktionalität von ordnern ist nicht gegeben
* Man kann nichts verschieben
* Es gibt keine weiterern unterordner
* Dadurch kann man sich auch wieder metadaten auf dem master sparen
* Eintrag darin das heißt er braucht 4 byte für adresse 3 mal + 8 byte id + 
53 byte für iban und bic 
* Dieses vorgehen wird auch namespace flattening genannt

* Das oben genannte war ads problem mit der memory consumption des masters
um alle files zu halten
* das sollte der ansatz lösen
* Auch die anfragen vieler einzelner files spielt keine rolle mehr,da sie ja
gebündelt wurden
* Um aber auch bei etas größeren reads, die über mehrere dateien gehen den 
master weiter zu entlasten wsoll das DFS auch noch ein prefetching machen
* Und zwar serverseitig
* Prefetching besagt eigentlich nur, dass Daten die vielleicht in der zukunft
gebraucht werden mal vorgeladen und gecached werden
* in der regel client seitig, das setzt aber vorraus, dass client weiß 
was der server bietet
* das ist ja hier eher nicht so
* Deswegen machen wir es serverseitig
* Und zwar das prefetching der information wo sich die anderen buckets für ein
Konto befinden
* Wenn der client eine ID anfragt, erhält er die adresse des OSD, welcher den
Bucket hält, der das speichert
* Wenn er aber über den bucket hinaus lesen möchte, muss er erneut anfragen
* Das kann man beheben, wenn man immer shon mehrere Bucket adressen mitschickt
bei anfrage
* Damit wird der master umgangen
* Die einträge können auch lange gecached werden, da sich Ordner ja nicht ändern können
 
Beispiel Write
* Write läuft ähnlich zu Write bei GFS ab
* Der Client fragt mit dem ersten Teil der ID also dem BIC und IBAN beim master
nach einem beschreibbaren Bucket
* Auch in diesem DFS gibt es von jedem Bucket in der regel 3 Replikas
* Einer davon wird Primary die anderen secondaries
* Er erhält auch ein Lease auf den primary
* Client sendet Daten zu allen Replikas die diese erst im hauotspeicher halten
* Und erst mit einer seperaten Write Request mit Ende des Leases schreiben
* Werden dann an Bucket angehängt
* Dabei immer erstmal 8 byte id, dann 2 bte größe und dann daten
* Und das inmemory mapping des bucket index files wird gegebenenfalls angepasst,
wenn Datengrenze überschritten
* Um Zeit für Leases so gering wie möglich zu halten, wird das hier auf 10 
Sekunden gesetzt aber Primary kann verlängern wenn das benötigt wird
* Daraus ergeben sich die gleichen vorteile wie bei GFS
* Die Reihenfolge ind er das applied wird ist sichergestellt
* Auch wenn einer zweiter client dazu kommt und vielleicht etwas schreiben 
möchte das ansich davor gekommen wäre gibt es keine Probleme
* Reihenfolge wird durch leases gewährleistet

Beispiel Read
* Bei Read gibt es fall wo entweder ein einzelner Dateneintrag angefragt wird
* Oder mehrere aneinander hängende
* Bei Einzelnen senden Client nur die ID an den Master
* Das Bedeutet auch er muss diese ganeu kennen
* Der Master schaut nach in welchem Bucket für die gegebene BIC und IBAN sich
die Daten befinden
* Er sendet die Locations aller Replikas an den Client zurück plus
die Locations von einigen andern buckets prefetching
* Der client fragt mit seiner id bei osd an
* der sucht in main memory nach nächstem checkpoint innerhalb des 
entsprechenden buckets 
* Wenn der gefunden sucht er von diesem checkpoint aus nach eintrag
* ließt immer id und größe bis er den richtigen eintrag gefunden hat
* den gibt er dann zurück

* Sollen mehrere gelesen werden, gibt client einfach einen timestamp von
und gegebenenfalls einen timestamp zu mit
* Der ablauf ist ähnlich
* Der Master schaut, wo diese Timestamps enthalten sein könnten
* sendet adresse für bucket plus mögliche weiterer mit
* anfrage an bucket
* bucket schaut in memory nach wo es weitergeht
* er fängt an zu lesen und gibt jetzt aber nicht nur den einen eintrag zurück
sondern ab diesem eintrag alle die passen
* Das ganze wird gestreamt, so das client acuh abbrechen kann wenn er genug hat
* Wenn chunk nicht alle Daten enthalten hat, kann client anfrage an anderen
bucket stellen, er hat ja durch das prefetching die adresse ja bereits

* Was erreichen wir durch dieses System
* Dieses DFS ermöglicht das ablegen sehr vieler kleiner Dateien
* Ein OSD benötigt zum abspeichern eines 16 mb blocks gerade einmal 176 bytes
* 11 bytes pro eintrag, 16 Einträge pro bucket ergibt 176 bytes
* Das heißt eine OSD mit 16 gb ram um die Daten zu verwalten kann alleine schon 
über 90 Millionen Buckets verwalten mit jeweils 16000 Einträgen
* Der Master braucht zum ablegen eines buckets mit all seinen replikas 
auch gerade einmal 73 bytes hauptspeicher
* Gehen wir auch wieder von 16 gb an Ram aus kommen wir so auf über 219 
millionen Buckets, das etwa 3.5 Billiarden Buchungen oder einem Speicher von 3.
5 Petabyte entspricht. Und das lässt sich durch das einfache aufstocken des 
hauptspeichers noch deutlich verbessern
* Lese zugriffe brauchen in der Regel nur einen einzigen Zugriff auf die 
Festplatte
* Auch das lesen mehrere Einträge zum Beispiel weil man alle Transaktionen eines
Monats brauchtsind auch meistens mit einem einzigen READ zu erledigen da sie
im Speicher zusammengeführt wurden
* Mit diesem System sind die Buchungen eines gesamten Lebens abzubilden und 
schnell auslesbar
* Skalierung erfolgt einfach über das hinzufügen weiterer osds
* Haben gleiche Vorteile wie GFS plus umgang mit kleinen files sowie haystack

* Inkonsistenzen können nur wie bei gfs auftreten für die Zeit eines leases
* Ist Konsistenz aber gewünscht, kann das erzielt werden indem leases für
gewissen zeitraum nicht vergeben werden
* Die Abbildung der im Kapitel ref{Kon} erarbeiteten Kontoarten wäre 
also über ein solches System problemlos möglich
* Aber was ein Problem ist, ist, dass zum feststellen des Kontostandes
* Immer alle Transaktionen gelesen werden müssen
* Wenn das sehr viele werden ist das ein problem
* Dieses Problem und das Problem der Inkonsistenz lässt sich im design 
des Buchungssystems besprochen

## Design des Buchungssystems
* Das hier entworfene Buchungssystem stellt die Schnittstelle für Anwendungen
* Zum schreiben und lesen von Buchungen
* Es Validiert sie auch
* Des weiterern ist es da einzige System, dass mit dem DFS kommuniziert
* Für viele Bereiche würde das in kapitel \ref{das dfs} besprochene DFS als 
Persistenzschicht des Buchungssystems reichen
* Bei Ratenkrediten oder Termineinlagen passieren nicht soviele Bewegungen, dass
das zusammenrechnen dieser zum erhalten des Kontostandes ein Problem wäre
* Kontobewegungen sind auch vorhersehbar und daher kann ein Konsistenter zustand
zu diesen Zeitpunkten erzeugt werden
* die würden sich also schonmal abdecken lassen
* Girokonten oder Konten mit sehr vielen Kontobewegungen sind aber nicht 
optimal damit abzudecken. Man kann nicht immer genauen Kontostand auslesen
und oder man muss immer alle Transaktionen dafür lesen
* Das ist auch ein Problem wenn man vor einer Überweisung prüfen möchte
was möglich ist
* Die einzige Information, die also konsistent sein müsste wäre der Kontostand
* Un um den abzulegen braucht man für jedes Konto nur einen einzigen Eintrag
der geändert wird bei jedem Write
* Um diesen abzulegen bietet sich der einsatz eines acid basierten Systems
also an
* Eine relationale Datenbank mit einer einzigen Tabelle
* Beim erstellen eines Kontos wird eintrag in Tabelle erstellt
* Speichert für jedes Konto den aktuellen Kontostand plus das Datum der letzten
Transaktion
* Dadurch, dass nur noch für jedes Konto ein eintrag da ist, ist es für einen
einziges Datenbanksystem handhabbar und Skalierung wird nicht benötigt
* Wenn bei jedem Write sichergestellt wird, dass er zuerst tabelle verändert
und dann den Eintrag in das DFS speichert, kann sichergestellt werden, dass 
Daten in tabelle immer korrekt sind
* Schlägt einer der beiden vorgänge fehl, wird der write nicht akzeptiert
* Das würde ermgöglichen Anfragen zu prüfen bevor sie durchgehen
* Über 0 oder schon unter null
* Man kann über das Datum feststellen ob die Daten die man vom DFS erhalten hat
Konsistent sind aber auf jeden fall schonmal richtigen Kontostand anzeigen
* Durch dieses System lassen sich eigentlich alle Kontoarten abbilden

* Wie läuft denn jetzt ein Write ab?
* Kommt beim Buchungssystem an
* Buchungssystem validiert, ob es Konto gibt
* Und ob Buchung preislich ok ist
* Wenn das ok, dann wird Eintrag in tabelle upgedatet
* Also auch letzten eintrag
* Man fürhrt mit der Buchung write auf DFS durch
* Schlägt Write Fehl wird Write als ungültig gesehen
* Genauso, wenn update fehlschllägt

* Wie läuft ein Read ab
* Client gibt an welchen Bereich er lesen will
* Buchungssystem fragt Datenbank an für Kontostand
* checkt ob Konto existiert
* Fragt DFS ab nach Transaktion
* Sendet Daen kombiniert zurück


## Skalierbarkeit 
* In wieweit bringt dieses Deign jetzt Vorteile für die Skalierung
* Buchungssystemen selbst ist keine eigene Datenbank zugeteilt
* Das heißt wenn mehrere Anfragen kommen, können auch einfach ein weiterer
Server für das Buchungssystem hochgefahren werden und Anfragen beantworten
* Alles in allen können die Buchungssystem Server sogar nach Last hochgefahren
und weggekürzt werden
* Um Die Datenbanken zu Skalieren kann man entweder Ansatz wählen 
weiterer DBs aufzusetzen, die alle das gleiche speichern
* Ist ja auch nur immer ein eintrag der pro anfrage gelesen werden muss
* Oder man nimmt Datenbanken, die jeweils einen Teil bereich zum beispiel BIC
Bereiche speichern
* Das DFS lässt sich wie bereits in vorangegagenen Kapiteln erwähnt einfach 
durch das hinzufügen neuer OSDs skalieren
* Davon bekommt der Nutzer auch gar nichts mit
* Durch das verwenden einer http schnittstelle
* Kann sogar bei drittanbieter liegen oder ausgetauscht werden
* Niemand bekommt es mit
* Schwachpunkt ist master aber der kann auch eifnach durch hinzufügen von 
Ram gestärkt werden
* Oder man nimmt auch hier ein zweites DFS das für einen anderen BIC bereich


 

 = bei 16 gb ram dafür kann er über 219 millionen
Buckets verwalten das entspricht über 3.5 billiarden Buchungen

 
 
* 16 MB größe einer Datei etwa 1kb größe ein Eintrag
* 16 GB Ram 8 byte großer Schlüssel es werden nur alle 1000 Einträge ein
Key gesetzt

* Der Key bestimmt den logischen zusammenhang von Dateien
* Dateien die logisch zusammenhängen werden in gleichen Chunk bzw Volume gepackt
* Komponenten
* Master
* OSDs

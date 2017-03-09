* Möglichkeiten Konzept auch anzuwenden für Girokonten
* 10 Skunden warten
	* Wäre das überhaupt ein Problem,wenn man sich überlegt wie schnell man bucht
* Anfrage für Konsistenz für jede Anfrage
	* Viel schlimmer als write muss read performant werden
	* Wegen PSD 2 etc.
* Beide Ideen haben den Nachteil, dass viele Anfragen nötig sind bei 
Konten mit vielen Transaktionen Amazon.

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
* Das Konzept soll sich zunächst nur um diese Anforderungen kümmern
* Wird aber dahingehend entworfen alles abzudecken

## Annahmen 
* Bevor das Konzeot entwickelt wird, müssen erst die Annahmen geklärt werden
* Das System muss mit sehr vielen Anfragen klar kommen
* Wurde ja oben gezeigt
* 
* Aus diesen Gründen eignet sich Haystack nicht und GFS auch nicht


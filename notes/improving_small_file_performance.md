Improving small file performance in object-based storage
========================================================
* Das Paper behandelt server-driven metadata prefetching, und namespace
flattening um besser mit kleinen files umgehen zu können
* IDs sorgen dafür, dass der Namespace auf die id gemapped wird
* Dadurch kann man id range ausnutzen

## Introduction
* direkter Client Zugriff bietet skalierbare Bandbreite
* Es müssen nicht alle Anfragen durch einen einzigen Client
* Die sind sehr gut für große Files haben aber Probleme mit kleinen
* Für jede Anfrage muss erst Metadaten Server gefragt werden ist Flaschenhals
* Eine Möglichkeit istdas mitliefern von Metadaten zusammenhängender Files und 
nicht nur vom angefragten. Damit muss client nicht jedesmal master fragen
* Dazu muss natürlich zusammenhang gewährleistet werden
* Schwierigkeit ist, den Schlüssel irgendwie so zu erzeugen, dass zusammenhänge
mit anderen Schlüsseln hergestellt werden können
* Position in ObjectID rein coden
* Erlaubt Metadata server loads bon 27-94 % kleiner zu machen
* System kann dann 1.4 - 17 mal so stark Skalieren
* Mit dem Prefetching können 5-34% aller master interactions vermieden werden

## Small files and object stores
### Problems for small files
* Object bases storage systems weisen jedem file ein ojekt zu oder mehreren
* Aber es gibt dabei zwei Probleme
	* namespace locality geht verloren
	* und die vielen Interaktionen mit master um positionen herauszufinden

* Per file metadata server interactions
	* Um ein file zu lesen, muss der client immer erstmal beim master nachfragen
	wo das ding liegt
	* Das kann schwere Last auf dem Master ezeugen
	* Und es kann zu Latenzen für den Client beim lesen kommen
	* Latenz wird so verdoppelt
	* Und der master muss soviele requests abkönnen wie alle Devices gemeinsam

* Loss of storage locality
	* Die dateien werden wie bei einem lokalen file system auf der Platte
	abgelegt
	* Die Performance bedingungen und ziele sind die gleichen wie im lokalen
	file system
	* Große objekte sollten möglichst nahe zusammen liegen und der inhalt sollte 
	sequentiell abgelegt werden
	* Bei kleinen files kann das nur über interobjekt lokalität realisiert werden
	* Objekte die wahrscheinlich gemeinsam angefragt werden sollen zusammen
	liegen
	* Das ist für OSDs aber schwer, da sie ja nichts zu der directory struktur
	wissen
	* Diese wird ja vom master gemanaged

### Solving the problems
* Server driven metadata prefetching und namespace flatteing kann sich um diese
Probleme kümmern ohne die architektur zu ändern

* Server driven metadata prefetching
	* Anstatt, dass nur die Daten für ein einziges Objekt vom Master zurückgeben
	werden,  sollen die Daten für alle Objekte die eine Verbindung dazu haben
	zurückgegeben werden
	* Der Client kann dadurch seinen Cache anreichern und muss nicht bei jeder
	Request den Client fragen
	* Der client kann zwar immer noch entscheiden, was er verwirft und was er 
	nutzt aber der master entscheidet, was er im vorraus verschickt
	* Normaler weise wird client site prefetching genutzt
	* Das ist aber in diesem Anwendungsfall nicht sinnvoll
	* 1.  Dafür muss der client aber genau wissen, was er will
	* Der Server weiß welche Daten existieren und kann so sinnvoller auswählen
	* Der Client müsste dafür die file informationen auch lokal abspeichern
	* Client prefetching ist das large file streaming
	* Beim Large File streaming würde das anders ablaufen
	* Da streamt man und fragt dann einfach nach dem nächsten Teil
	* 2. Die Tatsache, dass der Client immer optionen zum prefetchen bekommt
	erhöht, dass es auch wirklich genutzt wird
	* 3. der Master weiß welche metadaten günstig geprefetched werden können
	* Er kann berücksichtigen welche Daten schon im Cache sind
	* Um die last vom master zu nehmen müssen mehrere Metadaten geprefetched
	werden

* Namespace Flattening
	* Keine Ordner und Pfade mehr sondern ID
	* Aber auch keine einfach aufsteigende Nummer als Object id
	* Die Object ID soll auch die Locality innerhalb des Namespaces wiederspiegeln
	* Das encodieren des namespaces in die ID hat mehrere Vorteile
	* 1. Die Storage Devices können die ID nehmen als Hinweis auf die lokaltät
	* Das kann auch für sinnvolles cachen etc. genutzt werden
	* 2. Die Indexstruktur bei den OSDs packt ähnliche Object IDS auch zusammen
	* 3. Mehere Files die zusammengehören, können bei einem Teil einer ObjectID
	bezeichnet werden anstelle einer ganzen Liste id/id anstelle von 1. Id 2.ID..

### Related Work
* namespace locality ist ein guter indikator um die Beziehung zwischen Files
festzustellen
* Diese werden häufig ein einem gemeinsamen Bereich auf der Platte gelegt
* Auch gut für das gemeinsame Abspeichern von Daten ist es das Zugriffsverhalten
zu analysieren oder hinweise der Anwendung mitzukriegen
* Man kannd as auch kombinieren

## Improving small file efficiency
### Server driven metadata prefetching
* Erst Metadaten vom Master holen und dann mit dem OSD reden
* Hier wird server driven metadata prefetching betrieben

**Object metadata and capabilities**
* Metadata für jedes Objekt beinhaltet mapping und deskriptive Informationen
* capabilities sind Zugriffsrechte vom Master vergeben

* Mapping Information
	* Beschreibt den Ort eines bestimmten File, oder eben mehrere

* Descriptive Information
	* Jeder macht das unterschiedlich kann Länge des Objekts sein
	Oder Zugriffe etc. 

* Capabilities
	* Gibt an ob jemand wirklich Rechte hat etwas zu tun
	* Ein schlüssel ist ein geteiltes secret zwischen Storage device und 
	Master

**Changes in each component**
* Object Storage hat metadata server, clients und OSDs
* Wie ändern sich diese für das metadata prefetching

* Metadata Server
	* Normalerweise interagiert ein Client mit dem Metadata server für ein Objekt
	* Fürs Prefetching aber auch mit den Objekten bei denen er glaubt, dass sie 
	den Client interessieren könnten
	* Das Prefetching sollte sich in Grenzen halten, da kein Overhead entstehen
	sollte
	* Man könnte die Daten natürlich noch komprimieren um mehr liefern zu können
	* Generell sollten sie aber klein genug sein um auch zu rein zupassen

* Clients
	* Hauptunterschied zu vorher ist, dass Client mehr Infos erhält
	* Diese sollte der Client cachen
	* Client braucht zwei caches einmal was er vom Server vorgeschlagen bekommen
	hat
	* und was er selbst angefragt hat
	* Er muss sich genau überlegen, was sich lohnt zu speichern
	* Es wäre schlecht, wenn ein prefetch nicht mehr passt, weil nutzlose im cache
	sind
	* Der client muss also erkennen ob er die prefetched values brauchen kann

* Storage devices
	* Da gibt es keine änderungen
	* Außer, dass es bessere performance gibt
	* Weil die einzelnen Dateien näher beieinander stehen
	* wenn namespace lokalität zu access lokalität wird, dann ist das gut!

**Multi-object capabilities**
* Ist sicherheits bezogen
* Normalerweise für jedes file
* Diese müsste jedes mal generiert werden
* Deswegen besser wenn sie das für mehrere kann
* aber sie muss klein bleiben weil sie in jeder request drin sein muss

### Namespace flattening
* Ist eine Zuweisungs Stateggie, die die Ordnerstruktur in die Ohject ID codiert

**Fragmentation in the object ID space**
* Veränderungen des Namespace werden die lokalität zerstören, dadurch, dass die 
Object ID zum Star vergeben wird
* Der Namespace room muss auch eine passende größe vorweisen

**Namespace flattening algorithmus**
* Zwei Algorithmen für Namespaceflattening und Fragmentierung vorbeugen

* Child Closest
* Home mit 0x4 und user mit 0xa wird zu 0x4a File Number wächst dann von rechts
nach links also 0x8 anhängen wird zu 0x4a0 0008

* Cousin-Closest
* Das wächst aber von rechts nach links gleiches Beispiel wäre 0x0000 04a8

**Excessively deep and wide hierarchies**
* Problem ist, dass die IDS ausgehen
* 


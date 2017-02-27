Haystack
========
## Abstract
* Handelt bei Facebook gerade ungefähr 20 petabytes an daten
* Liefert über 1 Mio. Bilder pro Sekunde zu den Peaks aus
* Viel Billiger als NFS
* Reduzierung de Photo Metadata um alle Lookups in Main Memory zu machen

## Introduction
* Data is written once, offten read
* Nachteile eines normalen POSIX Filesystems sind Ordner und per file metadata
* Verschwendet SPeicherplatz aber macht es auch langsam wegen READ von Platte
* Das Lesen der Metadaten als Flaschenhals
* Hauptproblem bei NAS bzw. NFS

* Haystack kann Hoher Durchsatz bei niedriger Latenz
* Meistens nur eine Diskoperation per Read
* Alle Metadaten bleiben Im Speicher

* Haystack ist sehr Fehlertolerant
* In großen Systemen passieren Fehler jeden Tag
* Replikation auf Datencenter und Volume ebene

* Haystack ist sehr Kosten Effektv
* Besser zumindest als die vorherigen NFS basierten systeme
* 28 % kostet pro Terabyte weniger als NFS
* Kann 4x mehr Reads 

* Haystack ist einfach

## Background
* Caching macht nicht für alle Daten Sinn.

## Discussion
* Es ist fraglich wann ein neues System geschrieben werden soll und wann nicht
* Sie haben sich auch überlegt GFS nachzubauen
* Das ausliefern von Bildern hat jedoch das Problem, dass weder MySQL noch Nas
lösen kann
* Bestehende Speicher Systeme haben nicht das passende RAM to Plattenspeicher
verhältnis
* Es wird Speicher benötigt um die gesamten Metadaten aller files im Speicher
zu halten
* Inode (Recherchieren) Das beinhaltet die Daten zu Files

## Design und Implementierung
* Speichern mehrerer Fotos in einem großen File
* Application Metadata Was der Browser braucht im file zu erhalten
* Filesystem metadate auf dem Host um File zu finden

### Overview
* Haystack besteht aus 3 Kernelementen
	* HAystack store
		* Managed die Metadaten 
		* Kapselt den persistenten Speicher
		* Die Kapazität wird in Volumes aufgeteilt 10 tb in 100 100 gb volumes
		* Physical Volumes auf unterschiedlichen Maschinen kann zu logischen 
		Volumes zusammengefasst werden
		* Wenn Ein Bild zu einem logical volume geschrieben wird, wird es zu den
		physischen VOlumes darin geschrieben
		* Sorgt für Sicherheit bei Harddrive failure
	* Haystack Directory
		* Kennt das logische zu physikalische Volume mapping und die application
		metadata
		* Auch der freie Speicher der logischen Volumes ist bekannt
	* Haystack Cache
		* Funktioniert wie ein internes CDN
		* Schützt deen Store vor Anfragen nach den most popular daten
		* Bild Zeigt wie das funktioniert

### Haystack directory
* 4 Hauptfunktionen
* 1. Mapping von logical to physical volumes
* Wird genutzt wenn Webserver ein Bildhochladen oder wenn die URL gebaut wird
* 2. Balances writes zwischen logical volumes und reads zwischen physical volumes
* 3. Bestimmt von CDN oder Cache die Request handeln soll
* 4. Erkennt logical volumes wlehce nur Read only sind wenn sie z.B. voll sind

### Haystack Cache
* Erhält Anfragen von CDN und vom Browser direkt
* Fotos werden nur gecached wenn, wenn die Anfrage von Nutzer kommer und wenn,
die Anfrage von einem Write-enables Storage kommt.
* Gründe sind besserer Performance wenn entweder read oder write ausgeführt wird 
(!!!)
* Ansonsten würden die meisten Anfragen an die write enabled Maschinen kommen
(Bilder werden am meisten kurz nach Upload angefragt)

### Haystack Store
* Read anfragen fragen nach einem logischen VOlume und nach einer physical Store
mashine
* Jede Store mashine managed mehrere phsysical volumes
* Jedes Volume hält millionen Bilder
* Physical Volume kann man sich wie ein sehr großes FIle vorstellen
* Access erfolgt schnell über logical volume und den file offset innerhalb des 
physical volumes
* benötigt filename, offset und größe des fotos
* Diese Infro kommt aus der Store machine, denn sie hält das für jedes physical 
volume  im Speicher
* Physical Volume ist ein Superblock gefolgt von needles.
* Jede Needle ist ein Foto
* Needle hat auch wieder eine Struktur (Siehe Bild)
* ZUm Schnellen lesen hat jede Store Machine alle key für jedes Volume im speicher
* Auch der Offset und die größe in Bytes steht da drin
* Das kann immer durch das Volume file wieder hergestellt werden

**Photo Read**
* Zum Read wird die logical volume id, der key und der cookie geschickt
* cookie ist eine random nummer um zufällige reads zu verhindern
* Wenn die Anfrage rein kommt, schaut die Store machine erstmal nach, ob sie
die metdadaten dafür hat
* Dann wird das File mit dem entsprechenden offset im file gesucht
* Dann wird das Bild zurückgesendet

**Photo Write**
* Dazu wird die logical volume id, der key und der cookie benötigt, und die Daten
* Die files werden synchron zu dem physical volume hinzugefügt und updated die in
memory data
* Updates sind so nicht möglich
* Updates erfolgen indem man eine needle mit dem gleichen key erzeugt
* Wenn sich das logical volume geändert hat, wird das auch entsprechend im
Dictionary angepasst
* Ansonsten einfach zum physical volume hinzufügen
* Latest needle ist die mit dem größten offset

**Photo Delete**
* Setzen einer Delete flag im in -memory mapping und im volume file
* requests sehen, dass das file gelöscht ist

### The index File
* Index File sind für den reboot von maschinen gedacht
* Darin stehen die ganzen Daten für jedes Volume
* Das geht viel schneller, als alle Volumes einmal durchzuscannen
* Ist aufgebaut wie ein normales volume
* Kann aber zu inkonsistenzen führen
* Bei jedem Neustart wird nach neuen orphans gesucht
* Für Deletes files muss aber das ganze file gelesen werden

### Filesystem
* Am besten ein filesystem das nicht viel ram braucht um in großen Dateien zu 
suchen
* Jedes Store machine verwendet aktuell XFS
* Die Blockmaps sind klein genug um sie in Memory zu halten
* XFS kann preallocation
* Durch XFS können disoperations minimiert werden um ein Photo zu lesen

### Recovery from failures
* Haystack muss fehler tollerant sein!
* Hard drive fehler
* Misbehaiving RAID controlers
* Schlechte motherboards
* Zwei Techniken Detect and repair
* Regelmäßig wird health von Store machines getestet
* Connection und volume files
* Wenn das nicht klappt werden die logical volumes darauf als read only
gekennzeichnet
* Ansonsten können Neddles auch über ein replica wieder hergestellt werden

### Optimizations
**Compaction**
* Speicher von gelöschten und doppelten Bildern frei geben
* Dazu werden needles in ein neues File kopiert während alle doppelten, gelöschten
übersprungen werden, dann werden die in memory maps angepasst.

**Saving more memory**
* Statt delete flag einfach offset auf 0 setzen
* Cookies auch nicht im Speicher sondern erst nach READ checken 20 % besser!
* 10 bytes liegenim SPeicher pro Bild
* Für alle 4 Bilder 40 bytes im gegensatz xfsinode etwa 536 bytes

**Batch upload**
* Uploads zusammen führen wenn möglich!
* Disk sind schneller bei großen Daten


## Evaluation
* Statistiken zu Reads and writes
* Randomio zum testen der Reads
* 




The Google File System
======================
* Fehler tolerant auch wenn es auf commodity hardware läuft
* Hohe performance für viele Clients
* Generell wie alle DFS aber ihre Idee wurde von ihrer Anwendung und technischen
Umgebung geleitet

## Introduction
* Designed um den steigenden daten verarbeitungs ansprüchen gerecht zu werden
* Performance, scalability,, reliability und availability sind goals
* Fehler tolerant gegen menschliche Fehler, software bugs, os bugs, disk, speicher
motherboard, networking und power errors
* Die Dateien sind sehr groß. Mehrere GB groß
* Hauptsächlich append , anstelle von modify
* Einmal geschrieben sind die Dateien nur noch read
* atomic append


## Design Overview

### Assumptions
* Das System ist in commodity Hardware realisiert -> dieser versagt häufig
* Speichert einige millionen files mit 100 mb oder größer
* Kleine files sind nicht das ziel
* Zwei unterschiedliche Read arten
	* große streaming reads
	* und kleine random reads
* Es wird auch viele große sequential writes geben (append)
* KLeine writes müssen nicht effizient sein
* Das system muss damit umgehen könne, dass mehrere Clients auf ein file appenden
* Minimale synchronisation!
* Große Bandbreite ist wichtiger als geringe Latenz

### Interface
* Implementiert nicht ein standard API sowie POSIX
* Dateien sind hierarchisch angeordnet (Ordner und Pfade)
* Sie können, create, delete, open, close, read, write
* zusätzlich noch snapshot und append

### Architecture
* GFS besteht nur aus einem einzigen Master und mehreren chunkservern
* Dateien werden ind eine feste chunk größe zerlegt
* Jeder chunk ist unveränderbar und durch einen 64 bit chunk handle unique (Vom
Master erhalten)
* Chunkservers speichern die chunks als Linux files.
* Jeder chunk wird auf mehrere server gesenden (default 3)
* Der master kennt alle file system metadata
	* namespace, access control, wo liegen die chunks
	Kümmert sich auch um garbage collection und so weiter
* Chunkserver macht heartbeat mit allen chunkservern
* Die Clients reden mit dem master und den chunkservern
* Clients holen sich metadata vom master und reden dann mit den chunkservern
* sie müssen sich ohne POSIX in den vnode layer einhängen
* Es wird nicht gecached

### Single Master
* Vereinfacht das Design und sorgt für ein sehr sophisticated chunk placement
* Es muss jedoch minimal in reads und writes involiert sein -> kein bottleneck
* Master erlaubt keinen read oder write
* Beispiel Ablauf Bild auf seite 3
* Der Client cached die Daten die er bekommt (Entlastung des Masters)

### Chunk Size
* Ist bei ihnen 64 mb groß das ist größer als üblich
* Jedes chunk replica ist ein einfaches linux file
* Lazy space allocation ???? Maximal lange warten bevor man Disk speicher 
allokiert
* große Chunks erfordern nur einen einzigen Read bzw. interaktion mit master 
* Anwendungen lesen und schreiben große Files meistens sequentiell
* Je größer die chunks desto weniger muss der master wissen
* Problem der großen chunks ist, dass sie eine mögliche read last nur unter 
wenigen chunks aufteilen können

### Metadata
* master speichern drei haupttypen von metadaten
	* namespace vom file und den chunks
	* mapping von chunks to file
	* locatoin von jedem chunk replica
* Alle in memory und auch persistent
* damit kann stand immer wieder hergestellt werden! ggf. nochmal nachlesen!
* Chunk locations müssen nicht gespeichert werden, kann chunkserver einfach
fragen
* master speichert weniger als 64 byte metdadata für einen 64 mb junk
* Das aufstocken von ram ist ein kleiner preis für ein so simples system

### Chunk locations
* über heartbeat bleibt das immer up to date

### Operation log
* Beinhaltet einen geschichtlichen EIntrag für kritische Metadaten änderungen
* Es fungiert als timeline welche die Angabe zu concurrend Anfragen macht
* Alle werden mit einem Timestamp versehen
* Das ist der einzige Ort, der log muss immer consistent sein
* Deswegen wird hier auch sicher gestellt, dass das File wirklich geschrieben wird
bevor der client eine rückmeldung bekommt
* Es wird sogar gewartet, bis die replikas ersteltt wurden
* Der Master bündelt den log throughput bevor er verarbeitet wird
* Der State wird durch das replay des Logs wiederhergestellt
* Der Log wird regelmäßig gesnapshoted um startup times zu reduzieren
* Checkpoint ist B-Baum weil dann schneller in Memory! (Lesen wenn brauchen!)


### Consistency Model
* GFS hat relaxed consistency model
### Gurantees by GFS
* File namespace veränderungen sind atomic und nur vom Master durchgführt
* Dazu dient namespace locking
* operation log definiert die total order dafür
* AUch der append ist atomic
* Erkennt daten inkonsistenz beim handshake

### Implications for Applications
* Alles sind appends
* appends sind deutlich weniger anfällig für Fehler als random writes

## System Interactions
* Master ist nur minmal involviert

### Leases and Mutation Order
* Mutation ändert den inhalt der metadaten oder des chunkks und allen replicas
* leases sind da um die mutation order beizubehalten
* Lease hat einen timeout von 60 Sekunden
* Das primary chunk kann aber auch Lease verlängern
* Der Heartbeat gibt auskunft ob ein Lease jemals wieder aufgelöst wird

**Ablauf beim Lease ergreifen**
1. Client fragt master welcher chunkserver gerade ein lease auf einen chunk hat
Hat niemand ein Lease, so wird eines ausgegeben
2. Master gibt primary und replicas zurück, der client cached das!
3. Client schickt daten zu allen replicas
4. Wenn alle angekommen sind sendet client eine write anfrage zum primary
Diese request identifiziert die Daten, welche vorher gepushed worden sind
Der Primary generiert serial numbers für alle mutations
5. Primary sagt allen replicas bescheid, dass was geschrieben werden soll
6. Die secondaries sagen dem primary, wenn sie fertig sind!
7. Der Primary antwortet dem Client
Alle Fehler und fehler in den Replicas werden übertragen
Im Fehlerfall versucht es der Client einfach nochmal

* Wenn die der Write zu groß ist, bricht der gfs client code ihn mehrere
writes auf
* Alle laufen dann ab wie der oben beschriebene flow

### Data Flow
* Decoupeling data flow von kontroll fluss um Netzwerk sinnvoll zu nutzen
* Daten werden immer linear gepushed um die Bandbreite voll auszunutzen
* Jede Maschine sendet die Daten weiter zu ihrer nächsten Maschine
* Wenn ein Chunkserver ein bisschen Daten erhält, streamed er diese gleich weiter

### Atomic Record Appends
* Läuft generell ab wie der normale Flow außer einer ausnahme
* Es muss gecheckt werden, ob ein neuer chunk gebraucht wird
* Wenn das der fall ist, wird der chunk voll gemacht und dem client mitgeteilt
, dass er nochmal anfragen muss mit dem nächsten chunk
* Append Daten müssen kleiner als 1/4 sein um Fragmentierung zu minimieren
* Wenn ein append fehlschlägt, wird das nochmal probiert. Es kann zu 
inkonsistenzen kommen. Aber nicht so wild denn einmal wird atomic garantiert

### Snapshot
* Macht eine Kopie von einem File
* Snapshots revokes alle outstanding leases
* Dadurch müssen alle kommenden writes erstmal über den master

## Master Operations
* Macht alles wie bereits erwähnt und balanced auch die Load zwischen den chunks
servern
* Er Reclaimed auch unused Storage

### Namespace Management und Locking
* Viele Master OPerations sind Zeitaufwendig wie zum Beispiel der Snapshot
* Damit damit nicht das ganze System lahmgelegt wird, werden locks über namespaces
genutzt
* GFS hat keine data Struktur pro Ordner
* Namespaces werden als Lookup tabelle representiert
* Full Pathnames to metadata
* Read locks an allen beteiligten Pfaden (Ordner umbenennung verbieten)
* Und ein krasses lock auf der tatsächlichen Datei

### Replica Placement
* Daten verteilung, im Sinne der Skalierung verlässlichkeit und Verfügbar keit ist
sehr schwer
* Man will auch die Bandbreite optimal ausnutzen
* Replicas müssen nicht nur über maschinen sondern auch über Racks verteilt werden
* Wenn ein ganzes rack kaputt geht und um bandbreite mehrerer Racks zu nutzen

### Creation, Re-replication, Rebalancing
* Wenn der master einen chunk erzeugt überegt er sich wo er die zunächst leeren
replikas hinlegt
* Replicas werden nach unterschiedlichen kriterien verteilt
	* Alle Chunkserver mit unterdurchschnittlicher speicher auslastung
	* Es ist wichtig wie häufig auf dem chunkserver neue chunks abgelegt werden
	* Es muss über racks verteilt sein
* Wenn replicas verloren gehen (corruption) Dann gibt es rereplication
* Replication wird priorisiert nach 
	* Wieviele replicatoin fehlen bis zum default
	* Chunks von lebenden files sind auch wichtiger
	* Und wenn ein chunk client performance blockt
* Master sagt einem chunk server, dass er was von einem chunkserver kopieren soll
* Auch heir gelten wieder die entsprechenden richtlinen zu location wahl
* Hin und wieder werden die replicas auch umkopiert für ein rebalancing

### Garbage Collection
* Wenn File gelöscht wird, wird es nicht sofort entfernt
* Auf Chunk unf File Ebene

#### Mechanisum
* Wird gehandelt wie alle anderen events auch
* ABer File wird nur als gelöscht markiert
* HIdden Name und Datum des Löschens
* Bei einem Scan werden diese hidden Files entfernt
* Orphans werden durch Heartbeats gekennzeichnet und gelöscht

### Stale Replica Detection
* Master hat Versionen der einzelnen chunks
* Immer wenn ein neues Lease gegranted wird, die Version wird erhöht
* Die Replcias übernehmen dieser Version
* Niedrigere Version heißt Stale Replica, muss weg
* Höhere heißt Lease hat nicht geklappt, die neue muss genommen werden
* Client bekommt die Versionsnummer wenn er etwas lesen will
* Damit fragt er chunkserver an
* Bei anderer Version wird propagiert

## Fault Tolerance and Diagnosis
* Es gibt ständige Hardware fehler
* Es gibt auch immer einen shadow master
	* ließt replica des logs
	* und führt die gleichen changes durch

### Data Integrity
* Jeder Chunkserver verwendet checksumming um corruption zu bemerken
* Wird nur zum detecten von corruption, es ist eigentlich kein Problem
* Die einzelnen chunks müssen nicht hunder prozentig gleich sein, deswegen ist 
checksum sinnvoll
* chunks sind in 64 kb bläcke aufgeteilt jeder hat 32 bit checksum, diese ligen 
auch in memory
* Bei jedem Read wird erst zurückgeben ob checksum passt
* Chunkserver checken auch selbstständig hin und wieder checksums

### Diagnostic Tools
* Sie nutzen sehr diagnostisches logging

## Measurements
* Daten bei 16 chunkserver und 16 clients und ein master mit zwei replicas

### Reads
* N clients lesen gleichseitig
* jeder liest 4 MB random und das ganze 356 mal jeder insgesamt 1gb

### Record Append
* N clients append gleichzeitig auf ein einziges file

## Related Work
* Sie haben nur einen master und keine distributed algorithms to maintain
consistency
* Der Master hat viele Vorteile Chunk placement und replication



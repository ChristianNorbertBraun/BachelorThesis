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
* 



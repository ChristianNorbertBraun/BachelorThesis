Ceph: A Scalable, High-Performance Distributed File System
==========================================================
## Introduction
* In einem dirstributed file system wird object based gearbeitet
* Die Festplatte wird ersetzt mit Ibject Storage devices
	* CPU, netzwerk und cache und Festplatten in RAID
	* OSDs ersetzen block level interfaces bei denen byte ranges geschrieben 
	werden
* Object Base ist auf ein Video bezogen Metadata gebunden an Datei
* Block level interfaces ist auf byte ebene Also direkt auf der harddrive
* Metadata Server interagiert mit clients für rename etc.
* Der Datenfluss läuft aber direkt mit den OSDs
* Das verbessert die gesamte Skalierbarkeit
* Systeme die so funktionieren haben immer noch das Problem, dass die Metadaten
an einem Ort liegen und deswegen Skalierung begrenzt ist
* Dumme OCDs begrenzen auch die skalierbarkeit und performance
* Ceph ist besser, es kann alles was oben beschrieben wurde ohne nachteile 
(quasi)
* Ceph baut auf der These auf, dass Systeme mit vielen Daten sehr dynamisch sind
* Sie Wachsen nach und nach und Nodes fallen regelmäßig aus
* Ceph entkoppelt daten und metadaaten operationen indem File Allocation Tables
durch generating functions ersetzt werden
* So werden die OSDs intelligenter
* Ceph nutzt einen hoch anpassbaren metadata cluster -> Bessere Skalierbarkeit

## System Overview
* Ceph besteht aus 3 Hauptkomponenten
* Der Client mit einem fast POSIX konformen File Interface für die Hosts
* Ein Cluster von OSDs welche die Daten und Metadaten speichern
* Und ein Metadata Server Cluster der den Namespace handelt und sich um 
Sicherheit kümmert und Konsistenz und Kohärenz

* Ceph ist fast POSIX weil es das interface ein bisschen erweitert und teilweise
die Konsistenz ein bisschen auflockert
* Höchstes Ziel der Architektur ist die Skalierbarkeit
* hundres of petabytes and beyond
* Auch Performance und reliability sind sehr wichtig
* Skalierbarkeit wird in einige Richtungen betrachtet
* Die Gesamtkapazität und der Durchsatz des Systems
* Performance ist bezogen auf ein file oder ein directory und Clients

**Drei design features um das alles zu erreichen**
* Decoupled Data and Metadata
	* Maximale trennung von metadaten management vom speichern des files
	* Metadata operations open, rename werden vom metadata server cluster
	übernommen
	* File IO Reads gehen direkt an die OSDs
	* Anstelle von kurzer per file bliock list, hat Ceph sowas gar nicht
	* Stattdesen werden die daten zu vorhersagbar benamten objects hinzugefügt
	* CRUSH verteilt diese Objekte dann zu den Storage devices
	* Eine Abstraktion wieder mehr!
	* Dadurch muss man keine distribute Object List haben
	* Jeder kann die Position der Objects vorhersagen
	* Reduziert die zu speichernde Metadata
* Dynnamic Distributed Metadata Management
	* Metdadata operations sind die hälfte der Arbei von Typischen Filesystemen
	* Deswegen ist effektives Metadata Management kritisch!
	* Nutzt Dynamic Subtree Partitioning um die file system hierarchy auf die
	MDSs zu verteilen
* Reliable Autonomic Distributed Object Storage
	* Große Systeme sind dynamisch
	* Dateien werden verschoben, es gibt Ausfälle etc. Daten wachsen heran
	* Replication, Fehlererkennung und Fehlerbehebung sind aufgaben der OSDs

## Client Operation
* Der Ceph client läuft auf jedem host und stellt ein file system interface 
für die Anwendung zur Verfügung
* Zugriff entweder über direkten Link oder über FUSE
* Jeder Client hat seinen eigenen file data cache

### File IO and Capabilities
* Wenn ein File geöffnet wird, wird eine Anfrage an den MDS cluster gesendet
* Wenn das File existiert und man rechte hat, darf man darauf zugreifen
* Man bekommt auch info darüber wie die Files auf die Objekte aufgeteilt sind

### Client Synchronization
* POSIX will, dass ein READ alle vorherigen Writes berücksichtigt
* Außerdem muss ein write atomic sein
* Wenn ein file von mehreren clients geschrieben oder geschrieben und gelesen
werden will, entzieht der MDS erstmal alle rechte um sicherzustellen, dass
die Zugriffe Synchron sind
* Das wird gewährleistet dadurch, dass jeder Zugriff Blockt, bis der OSD in
bearbeitet
* Writes werden über exclusive locks realisiert auf Object Ebene
* Das Blocken ist Blöd, aber auch die Konsistenz Aufgabe ist blöd
* Deswgen hat man sich Erweiterungen für POSIX I/O ausgedacht von der 
high performance computing community HPC
* Zum beispiel ein lacy open der die Konsistenz für shared writes entspannt
* Zum Beispiel wenn an unterschiedlichen Stellen im File geschrieben wird

### Namespace Operations
* Interaktion mit dem Namespace ist aufgabe des metadata server clusters
* readdir und readstat und updates werden synchron angewendet für die Konsistenz
* Es werden keine Locks oder leases ausgestellt
* Ceph versucht auch sinnvoll im vorfeld daten zu cachen
* Bei einem readdir gefolgt von einem stats werden die gecacheden daten 
zurückgegeben

## Dynamically Distributed Metadata
* Metadata Scaling ist ein Problem!
* Auch wenn die generelle Speicherung leicht ist, ist das verwalten der 
Metadaten häufig aufwendig 
* Ceph hat sehr wenig metadata eigentlich nur die File names und inodes
* Es wird keine file allocation metadate gebraucht
* die Objektnamen werden generiert und via CRUSH auf die OSDs verteilt

### Metadata Storage
* MDS will generell alle Anfragen über den cache antworten, aber Änderungen an
den Metadaten müssen persistent gespeichert werden
* Dazu gibt es lazily flushed journals, welche jedem MDS das streamen der 
updates zu den OSDs erlaubt
* Ein anderer MDS kann das Journal scannen und den Stand des MDS 
wiederherstellen

### Dynamic Subtree Partitioning
* Die Metdadaten werden dynamisch über die Knoten verteilt
* Das ganze basiert auch auf basis der Anfragen an die entsprechenden 
directories also lastbasiert

### Traffic Control
*  Ceph verteilt die Metadaten so, dass hotspots mehr verteilt sind

## Distributed Object Storage
* Die Clients und die metadada server sehen den object storage cluster als einen
einzelnen Object Storage
* Cephs Reliable Autonomic Distributed Object Store (RADOS)
	* Kümmert sich um das lineare Scaling

### Data Distribution with CRUSH
* Macht sinnvolles Verteilen der Daten
* Auch das neue verordnen von Dateien macht CRUSH
* Zuerst werden Objekte in Placement Groups eingeordnet über Hashfunktion
* Jede OSD ist in etwa 100 PGs
* Die Wahl der OSDs wird über CRUSH getätigt
* Um was wieder zu finden braucht man nur die placement group und die osd cluster
map
* CRUSH löst distribution und location problem

* Ceph skaliert fast linear mit der Anzahl der OSDs

* Kernfeatures von CEPH gibts noch in der Conclusion

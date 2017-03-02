Object Storage: The Future Building Block for Storage Systems
=============================================================
* Object Storage wurde Anfag der 1990iger vorgestellt

## Object Store in a Nutshell
* OSD ermöglicht die Erstellung selbstständiger, geteilter und sicherer speicher
Netzwerke
* Speicher managmenet ist aufgabe des OSDs
* Ein oBject store hebt den level der Abstraktin von den heutigen block devices
* Anstelle eines arrays von unabhängigen Blöcken adressiert bei einem index
Tritt ein Object store als eine reihe von Objekten auf
* Ein Objekt ist ein Speicher behälter für daten und metadaten
* In einer object store umgebung wird der Speicher vom object store und nicht
von einer höher gelegten Anwendung bestimmt
* Die Nutzer eines Object Stores arbeiten auf dem Objekt. Lesen, Schreiben,
Löschen aber geben nicht an wo es landen soll

### Main benefits of Object Storage
* Ermöglicht sicheren, unvermittelten, geteilten Zugriff auf Speicher
* Das verschieben des Speicher allokierung und Zugriffskontrolle auf das Speicher
Gerät entfernt Druck von einem zentralen File server
* Ein Objekt ist ein Speicherbehälter für Kunden Daten und Attribute
* In einem Objekt store sind Metadaten ein wesentlicher Teil des Objekts
welcher gemeinsam mit den Daten gespeichert wird
* In der zukunft sollen Object Stores noch mehr können wie anfertigen von
Collections von Objekten, klonen, multi objekt operationen

### Difficulties with Paradigm Shift
* Object Storage benötigt einen Paradigmen Wechsel
* Eine sehr Grundlegende API im System Stack wird geändert
* Hosts greifen nicht mehr länger auf blöcke auf disks zu
* Sie beziehen sich auf object ids und offsets innerhalb von Objekten
* Desweiteren wird Speicherhandling nicht mehr vom Host gemacht
* Die Benefits von Object Storage sind eigentlich nur in hoch verteilten und 
hoch skalierbaren Speicher relevant

## Object storage in the Industry
* Am Anfang war die Anpassung an Object Storage von der Industrie her langsam
* Beispiele für Object Storage Systems sind PanFS und Lustre
	* Alle aufgebaut nach dem Schema network, metadata server, object storage
	clients


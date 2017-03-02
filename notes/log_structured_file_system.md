The Design and Implementation of a Log-Structured File System
=============================================================

* Ein Log structured file system schreibt alle modifikationen nacheinander
in einer log ähnlichen Struktur auf die Platte
* Der Log wird geindexed um schnell wieder was lesen zu können
* Zusätzlich wird der Log in Segmente geteilt
* Die Segmente können dann auch rearanged werden um fragmentation vorzubeugen

* Das Log File System nutzt 70% der Disk Bandbreite zum schreiben
* Unix File Systeme nutzen nur 10%

## Introduction
* CPU Speeds ist stärker gewachsen als Disk Speed 
* Das Log File System soll die Disk sehr viel Effizienter nutzen
* Log Structured File Systems gehen davon aus, das die Dateien im Hauptspeicher
gecached werden
* Disk Traffic sind im Endeffekt dann nur noch writes 
* Jedes Event wird sequentiell auf die Platte geschrieben
* Das erhöht die write performance
* Keine Seeks mehr benötigt
* Viele File Systems benutzten log nur für temporäre Daten
* Der Rest sind random access storage 
* Damit das funktioniert, muss immer viel Platz vorhanden sein
* Das ist eine große Herausforderung
* Dafür gibt es hier den Segment Cleaner
* Teilen von häufig ändernden und alten Daten
* UNIX 5 -10% weil der Rest für das Suchen gebraucht wird

## Design of file systems of the 1990s

### Technology
* Drei Dinge sind wichtig für ein File System: Prozessoren, Disks, Speicher
* Prozessoren werden immer schneller und entwickeln sich schneller weiter als
die anderen Beiden Punkte
* Disks werden immer Billiger aber nicht schneller
* Main Memory wird immer größer

### Workloads
* Besonders schwer ist der Umgang mit kleinen Files 
* Kleine Files führen häufig zu kleinen Random IOs
* Und einen Metadata overhead
* Log File System kümmert sich um small File Access

### Problems with existing file systems
* Information wird über die Ganze Disk verteilt
* die inodes sind separat von den files
* Man braucht mindestens 5 einzelnen disk ios mit einem seek um eine neue Datei
anzulegen
* Zwei für die file attribute, einen für die file daten, einen für den ordner,
einen für die ordner daten
* Bei kleinen files werden so nur 5% der Disk Bandwith genutzt rest ist suchen
* Außerdem schreiben aktuelle File Systems synchron
* Anwendung muss auf write warten
* Statt, dass sie weiter arbeiten kann und der write im hintergrund passiert
* Metadata ist ein großes Problem bei kleinen Files
* Die Anwendung muss von der Geschwindig keit der CPU profitieren können
* Wenn man auf die Disk wartet geht das nicht

## Log structured file systems
* Die Idee ist mehrere writes zu buffern und dann auf einmal auf die Platte zu
schreiben, nacheinander
* Dabei werden die vielen kleinen random access synchronen writes zu einem
einzigen sequenzielen asynchronen writes die die Disk Bandbreite ausnutzen

### File location and reading
* Der Log ist mit inidzes gespickt um random access zu erlauben
* Für jedes file gibt es einen inote mit den file attributen z.B. block position
* inodes werden aber im log abgelegt und nicht an einem festen ort
* Sie können über eine inode map referenziert werden
* Die most recent inodes werden immer im cache gehalten

### Free space management: Segments
* Wenn fragmentiert, dann kann man nur umkopieren
* Die Disk wird in Segmente aufgeteilt und diese werden befüllt
* Segmente müssen so groß sein, dass es sich für den Seek lohnt
* Im Sinne der Disk Bandwidth

### Segment cleaning mechanism
* Wenn mehrere Segmente voll laufen,
* Zusammen kopieren der live data
* Überführen in weniger Segments
* Die übrigen Segmente können wieder als clean markiert werden

### Segment cleaning policies
* Wann soll der cleaner laufen? Nachts, wenn Speicher knapp wird, regelmäßig?
* Wieviele Segmente soll er auf einmal cleanen, ansich je mehr desto besser
* Welche Segmente müssen gecleaned werden die am meisten fragmentierten?






* Kurz und Knapp:
Nimm viele kleine writes und pack sie erst im memory zusammen bevor du sie auf
Platte schreibst. Die Schwierigkeit entsteht durch das managen der 
Speicherblöcke. Es muss immer genug SPeicher da sein und am besten möglichst
defragmentiert

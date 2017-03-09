An optimized approach for storing and accessing small files on cloud storage
============================================================================
* Hadoop ist scheiße für viele kleine Files
* File grouping und prefetching ist auch eine möglichkeit

## Introduction
* Probleme von small files
* Der Hauptspeicher des Namenodes wird extrem beansprucht
* Wenn zusammenhänge zwischen Files nicht deutlich gemacht werden, treten hohe
Zugriffslatenzen auf
* Es gibt filemerging, prefetching, filegrouping

### Small file problem of HDFS
* Leistung von HDFS sinkt, wenn sehr viele kleine files abgelegt werden
* -> Es wird viel main memory gebraucht + ernsthafte zugriffs latenzen
* Bild Download von unterschiedlichen Dateigrößen beim gleichen Throuhgput
320 MB
* Um 5120 64 kb downzuloaden braucht es 4 1/4 mal so lange wie 5 64 mb files
* Und es braucht auch viel Speicher im Main Memory auch bild
* Ist wie GFS deswegen vergleichbar siehe website

## Small file tuning schemes for HDFS

* File merging and prefetching für struktur ähnliche Dateien
* File grouping und prefetching für logisch ähnliche files

### Classifications of files
* Strukturverwandte files
	* Ist eigentlich ein großes File aufgeteilt in viele kleine
	* Zum Beispiel weil es halt so üblich ist
	* Teile einer Landkarte
* Logischverwandte files
	* Sind ansich unabhängig aber aufgrund einer Regel oder Beziehungen zueinander
	können sie zusammengefasst werden 
* Unabhängige files
	* Kann man nicht definieren wozu die gehören

### File merging and prefetching scheme für strukturverwandte Daten
* Dateien sie eigentlich eh zusammengehören in ein großes mergen!
* Das reduziert die memory consumption von namenode
* Der Client soll die zusammen mergen(Schlecht für mich)
**Local file index**
* Lesen der Teildaten über ein index file wo offset und länge der gemergten drin 
stehen

**File merging strategy**
* Das macht der Client
* Der Namenode hat nur die Daten für dieses eine File
* Index File wird da irgendwie reingepackt

**Three-level prefetching and caching strategy**
* Prefetching versteckt sichtbare Dis IO kosten und verbessert die Antwortzeit
durch ausnutzen von zugriffsnähe

### File grouping and prefetching scheme for logically related small files
* Die Idee ist logisch voneinander abhängige Dateien in eine Datei zu packen
* Logische Units ist über dem File Level. Es geht um gleiche Kurse etc.

**Hinweis**
* Hat auch einige Daten dazu wie sich die Performacne verbessert hat
* Ist aber nur zum Teil auf mein Konzept anwendbar
* Ich würde wahrscheinlich metadata prefetching machen

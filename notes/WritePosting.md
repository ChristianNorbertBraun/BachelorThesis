Write Posting
=============
Ablauf um ein Posting  zu schreiben
1. **Client sendet Posting an Backend**
	* Posting wird geprüft (Kredit und Debit müssen gleich sein. Alle Felder müssen ausgefüllt sein
1. **Backend teilt Posting auf und speichert es in Journal**
	* Das Posting wird einmal für den Debitor sowie den Kreditor in das Journal geschrieben
	* Zusätzlich werden update Aufträge für das Konten des Kreditors wie den Debitor geschrieben
	* Jeder Eintrag im Jorunal erhält eine Priorität. Die höchste Priorität wird zuerst ausgeführt.
	* Wird ein neues Posting in das Journal eingetragen erhält es immer eine höhere Priorität als die dazugehörigen update Aufträge
1. **Posting für den Kreditor wird unter der Kontonummer des Kreditors abgespeichert**
	* Die Kontonummer stellt den Ordner da, das Posting wird nach dem entsprechenden Posting benannt.
1. **Posting für den Debitor wird unter der Kontonummer des Debitors abgespeichert**
	* Analog zu Kreditor Posting
1. **Update Aufträge für die Kontonummern des Kreditors und Debitors werden ausgeführt**
1.	* Durch den update Auftrag wird zunächst das aktuelle Kontofile ausgelesen
	* Aus dem Kontofile wird das letzte gelesene Posting extrahiert
1. **Mit dem Datum des letzten Postings wird nach neuen Postings unter der entsprechenden Kontonummer gesucht**
1.	* Alle Postings nach dem Timestamp des letzten Postings werden gelesen und die Kontodatei mit dem neuen letzten Posting überschrieben

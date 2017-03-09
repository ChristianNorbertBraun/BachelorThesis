educing File System Latency using a Predictive Approach
=======================================================

### Prefetching
* Einfachste ist, wenn der Nutzer das System informiert, was er demnächst noch
lesen will
* Dadurch kann das system diese zugriffe optimieren
* Aber ads hat auch einige Probleme
* Die Anwendung muss in der Lage sein mitzuteilen was sie will
* Der Entwickler muss genau wissen, wie die Dateien angesprochen werden
* Außerdem braucht das syste ein bisschen vorlauf um sicherzustellen, dass die
Datei auch wirklich geladen ist, wenn sie angefragt wird

## Automatic Prefetching
* Da bestimmt das OS welche Files als nächstes gebraucht werden
* Sie versuchen von vergangenen Verhalten der Nutzer auf das kommende zu schlißen
* Hat einige Vorteile
* Die Anwendungen müssen nicht umgeschrieben werden
* Dadurch haben alle die Vorteile des prefetchings

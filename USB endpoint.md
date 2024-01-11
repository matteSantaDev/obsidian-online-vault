[[buffer]] di un device USB
endpoint è un termine che si riferisce all'hardware, indipendentemente sal Sist. Op. host.

L'host può inviare e ricevere dati dal buffer.

L'endpoint può essere:
- CONTROL, bidirezionale, l'host può inviare dati all'endpoint e ricevere info da esso in un trasferimento.
- DATA, per trasferire i dati, unidirezionali.

???
**BULK ENDPOINTS**
I bulk endpoint differiscono dalle rest endpoint perchè combinano molteplici chiamate dello stesso tipo in un array e le eseguono come una singola richiesta.
L'endpoint handler splitta gli array in singole entità e le scrive come messaggi separati nella [[message queue]].

**[[REST]] ENDPOINT**
Endpoint di molteplici risorse REST
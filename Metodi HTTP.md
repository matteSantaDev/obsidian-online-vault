#http #best-practices 

----

#### GET
- Utilizzato per richiedere e recuperare risorse dal server.
- Non modifica lo stato del server o delle risorse.
- I parametri possono essere passati attraverso l'URL, quindi non è adatto per inviare dati sensibili come password o informazioni riservate.
- I dati ottenuto con questo metodo sono facilmente memorizzati nella cache del browser e possono essere segnalati nei log del server, poiché i parametri sono visibili nell'URL.

#### POST
- Utilizzato per inviare dati al server per creare una nuova risorsa.
- Modifica lo stato del server o delle risorse.
- I dati vengono solitamente inviati nel corpo della richiesta, quindi è consigliato l'utilizzo quando si devono mandare *id* e *uuid*.

#### PUT
- Utilizzato per aggiornare una risorsa esistente o crearne una nuova se non esiste.
- Invia i dati per sostituire completamente la risorsa esistente.
- Se la risorsa non esiste, può creare una nuova risorsa.
- I dati inviati sono passati nel corpo della richiesta HTTP e non nell'url.

#### PATCH
- Utilizzato per applicare modifiche parziali a una risorsa esistente.
- Invia solo le parti dei dati che devono essere aggiornate.
- I dati sono trasferiti nel corpo della richiesta HTTP e non nell'url.

#### HEAD
- Simile a GET, ma richiede solo l'intestazione della risorsa senza restituire il corpo della risorsa.
- Utile per ottenere informazioni sull'intestazione della risorsa senza scaricarne il contenuto.

#### DELETE
- Utilizzato per rimuovere una risorsa dal server.
- Elimina la risorsa specificata.

----

## Caratteriatiche dei metodi HTTP
### Safe
Un metodo è safe se non altera lo stato del server. Ciò significa che un metodo può essere considerato safe solo se porta ad un operazione _read-only_.
I metodi safe sono:
- GET
- HEAD
- OPTIONS
Tutti i metodi **safe** sono **idempotenti**, ma non viceversa. Ad esempio PUT e DELETE sono entrambi idempotenti ma non safe.

Anche se questi metodi portano ad operazioni read-only, i server possono alterarne lo stato (ad esempio facendo log o tenendo statistiche). La cosa principale però è che ==il client chiamando un metodo **safe** non richiede direttamente un cambiamento dello stato del server.== E quindi non creerà un carico o un onere superfluo per il server. I browser possono chiamare metodi sicuri senza timore di causare danni al server; questo consente loro di eseguire attività come il prefetching senza rischi. Anche i web crawler dipendono dalla chiamata di metodi sicuri.
I metodi sicuri non devono necessariamente servire solo file statici; un server può generare una risposta a un metodo sicuro on-the-fly, purché lo script generatore garantisca la sicurezza: non dovrebbe attivare effetti esterni, come avviare un ordine in un sito di e-commerce.
È responsabilità dell'applicazione sul server implementare correttamente la semantica sicura, il server web stesso, che sia Apache, Nginx o IIS, non può imporla da solo. In particolare, un'applicazione non dovrebbe permettere alle richieste GET di modificare il suo stato.

### Idempotent
Un metodo HTTP è idempotente se l'effetto desiderato sul server effettuando una singola richiesta è lo stesso dell'effetto generato da più richieste identiche.
Ciò non significa che non ci siano effetti collaterali specifici: ad esempio il server può loggare ogni richiesta con il tempo ricevuto.

L'**idempotency** si applica solo agli effetti generati dal client (una POST che invia dati al server o un DELETE che elimina risorse dal server).

I metodi che hanno questa proprietà sono:
- GET (safe)
- HEAD (safe)
- OPTIONS (safe)
- PUT
- DELETE
- TRACE
==Il metodo POST non gode della proprietà di **idempotency**.

Per essere idempotente, si considera solo lo stato del server. La risposta restituita da ogni richiesta può essere diversa: ad esempio, la prima chiamata di un DELETE probabilmente restituirà un codice `200`, mentre le successive probabilmente restituiranno un `404`. Un'altra implicazione dell'**idempotenza** del DELETE è che gli sviluppatori non dovrebbero implementare API RESTful con una funzionalità di eliminazione dell'ultima voce utilizzando il metodo DELETE.

Si noti che l'**idempotenza** di un metodo non è garantita dal server e alcune applicazioni potrebbero violarne erroneamente il vincolo.

### Cacheable
Una risposta HTTP è **cacheable** se può essere salvata nella cache (salvata per essere recuperata in un secondo momento).

I metodi che hanno questa proprietà sono:
- GET
- HEAD
- POST (solo se viene indicata la freschezza e l'intestazione Content-Location è impostata, ma questo è raramente implementato)
- PATCH  (solo se viene indicata la freschezza e l'intestazione Content-Location è impostata, ma questo è raramente implementato)

PUT e DELETE non sono salvabili nella cache.

Anche i codici di risposta sono cacheable, nello specifico i seguenti:
- 200
- 203
- 204
- 206
- 300
- 301
- 404
- 405
- 410
- 414
- 501
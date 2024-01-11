#myhound #best-practices 

---

Sono file di testo che conservano una piccola porzione di dati, come username e password, che sono usati per identificare il computer quando lo si usa nella rete. Cookies specifici sono usati per identificare gli utenti e fornire loro un esperienza di navigazione migliore.

I dati salvati in un cookie sono creati dal server alla connessione. I dati sono etichettati con un ID univoco associato all'user e al computer. Quando il cookie viene scambiato dal computer al network server, il server legge l'id e restituisce le informazioni relative.
Solitamente quando si parla di cookies ci si riferisce agli http cookies.

SESSIONE: tempo trascorso su un sito.

I cookies sono usati per identificare un utente quando visita un sito. Il server, che salva i dati, invia un breve flusso di dati identificativi al browser sotto forma di cookies. I cookies sono processati e da questi sono estratte info in formato name-value.
Il browser salva i cookies localmente; quanto viene visitato un sito il browser consulta i dati cookie e li ritorna al server. Quest'ultimo ripristina i dati sul sito dalle ultime sessioni.

I cookies sono usati per:
- session managment: permettono di riconoscere users e ripristinare le informazioni del profilo.
- personalization: consentono la personalizzazione delle ads presenti nel sito.
- tracking: permettono gestire i suggerimenti fatti da un sito.

### Tipologie di cookies
- **session cookies**: usati solo durante la navigazione in un sito. Sono salvati nella random accesss memory e mai scritti nell'hard drive. Quando la sessione termina i cookies vengono eliminati. Vengono usati anche quando si utilizza il bottone per tornare indietro del browser.
- **persistent cookies**: rimangono nel computer indefinitamente, anche se molti includono l'expiration date, raggiunta la quale vengono eliminati.



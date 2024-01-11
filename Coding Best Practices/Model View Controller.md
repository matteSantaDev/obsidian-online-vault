#best-practices 

___

Pattern architetturale diffuso nello sviluppo di sistemi software, in particolare in ambito della programmazione orientata agli oggetti e in applicazioni web, in grado di separare la logica di presentazione dei dati dalla logica di business.

Il componente centrale del MVC, **il modello**, cattura il comportamento dell'applicazione in termini di dominio del problema, indipendentemente dall'interfaccia utente (view). Il modello gestisce direttamente i dati, la logica e le regole dell'applicazione. 
Una **vista** può essere una qualsiasi rappresentazione in output di informazioni, come un grafico o un diagramma. Sono possibili viste multiple delle stesse informazioni, come ad esempio un grafico a barre per la gestione e la vista tabellare per l'amministrazione.
La terza parte, **il controller**, accetta l'input e lo converte in comandi per il modello e/o la vista.

Il pattern è basato sulla separazione dei compiti fra componenti software che interpretano tre ruoli principali:
- **model**: fornisce i metodi per accedere ai dati utili all'applicazione.
- **view**: visualizza i dati contenuti nei model e si occupa dell'interazione con utenti e agenti.
- **controller**: riceve i comandi dell'utente (in genere attraverso la view) e li attua modificando lo stato degli altri 2 componenti.

Questo schema impone anche la tradizionale separazione tra _logica applicativa_ (logica di business, a carico del controller e del model), e l'_interfaccia utente_ a carico della view. I dettagli dell'interazione tra questi oggetti dipendono molto dalle tecnologie usate (linguaggio di programmazione, eventuali libreirie, middleware..). Quasi sempre la relazione tra **view** e **model** è descrivibile anche come istanza del pattern [[Observer]]. A volte, quando è necessario cambiare il comportamento standard dell'applicazione a seconda delle circostanze, il _controller_ implementa anche il pattern [[Strategy]].

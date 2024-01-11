#JavaScript #best-practices 

----

### Usare codice modulare
Usare moduli piccoli che si occupano di una singola funzionalità. Seguire il principio di [[SOLID#Principio Single-Responsability|singola responsabilità]].
Minimizzare l'uso delle variabili globali poiché possono portare a un codice fortemente accoppiato e rendere difficile identificare le dipendenze. Meglio [[Principi programmazione ad oggetti o Object Oriented Programming (OOD)#Encapsulation|incapsulare]] le variabili all'interno dei moduli ed esporre solo le interfacce necessarie.
Se il file o modulo diventa eccessivamente lungo slittare il codice in file separati.

### Focalizzarsi sulla leggibilità del codice
Non è necessario inserire commenti se i nomi a variabili, classi e funzioni sono autoesplicativi, il codice così è anche più chiaro.
I commenti poi se non sono aggiornati possono ingannare il lettore.


### Separare la logica business dalle routes API
Non scrivere le funzioni logiche dentro le funzioni che gestiscono le routes.
Questo crea numerosi blocchi monolitici che non sono ingestibili, difficili da leggere e da decomporre.
Per questo è necessario ricorrere a:

### MVC pattern
Si consiglia di adottare lo schema [[Model View Controller|model view controller]]:
- **model**: componenti che rappresentano i dati, le interazioni con il database e la business logic dell'applicazione. Responsabile per l'implementazione delle regole di business "core" e della logica dell'app. Riguarda anche le operazioni [[CRUD]], la validazione dei dati e il mantenimento dell'integrità.
- **view**: riguarda il presentare i dati all'user e gestire i componenti dell'interfaccia utente (UI). Riceve i dati dal **model** mediante il **controller** e fa il render degli elementi grafici per poter garantire all'utente di visualizzare e interagire con i dati.
- **controller**: è il componente intermediario che riceve gli input dell'utente, aggiorna ii dati del **model** se necessario e coordina le interazioni tra **model** e **view**, assicurandosi che quest'ultimo mostri i dati all'user.


### Usare layer aggiuntivi (oltre ai tre MVC) per servizi e accesso dati
Possono essere aggiunti specifici layers (oltre a quelli del paradigma MVC) per gestire la logica di business e l'accesso dati.
Il **livello di servizio** è uno strato aggiuntivo introdotto per incapsulare la logica di business delle applicazioni. È una collezione di classi, metodi e funzioni che rappresentano le operazioni e i flussi di lavoro principali dell'applicazione. Il **controller** delega compiti complessi di logica di business al **layer di servizio**, e a sua volta lo strato di servizio interagisce con il modello per eseguire operazioni relative ai dati (**data access layer**).

Il **data access layer** viene integrato nell'architettura MVC come un layer di aggiuntivo responsabile delle interazioni con il database.

![[Pasted image 20231206145421.png]]


### Gestire file di configurazione separatamente
Organizzare i file di configurazione in una directory dedicata aiuta a centralizzare differenti file di configurazione riguardanti:
- database
- server
- logging
- caching
- environment
- localizzazione
Inoltre avendo una cartella dedicata ai file di configurazione è possibile circoscrivere le informazioni sensibili in un unico punto del progetto. In tale modo, se è usato un sistema di version control, è più facile definire regole per ignorare i dati importanti e non condividerli nelle repositories comuni.


### Usare il principio di [[SOLID#Principio di Dependency Injection (Invertion)|dependency injection]]
Adottando la dependency injection nel codice del server i vantaggi sono:
- **Test unitari semplificati**: Inietta le dipendenze direttamente nei moduli per test unitari più facili e completi.
- **Ridotta accoppiamento dei moduli**: Evita un accoppiamento stretto tra i moduli, migliorando la manutenibilità e la riutilizzabilità del codice.
- **Flusso Git accelerato**: Definisce interfacce stabili per le dipendenze, riducendo conflitti e agevolando lo sviluppo collaborativo.
- **Scalabilità migliorata**: Aggiungi o modifica componenti senza cambiamenti estesi nel codice esistente, facilitando l'estensione e l'adattamento.
- **Grande riutilizzabilità del codice**: Decupla i componenti e inietta le dipendenze per riutilizzare moduli in diverse parti dell'applicazione o in altri progetti.

Senza applicare dependency injection:
```js
import client from "../config/database.js"

export default async function getUserInfo(username) {
	try {
		const query = "SELECT * FROM info WHERE username = ?"
		const [rows] = await client.query(query, [username]);
	} catch (error) {
		console.log(error)
	}
}
```
Applicando dependency injection:
```js
export default async function getUserInfo(client, username) {
	try {
		const query = "SELECT * FROM info WHERE username = ?"
		const [rows] = await client.query(query, [username]);
	} catch (error) {
		console.log(error)
	}
}
```


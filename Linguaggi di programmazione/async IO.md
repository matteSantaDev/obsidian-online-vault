#python #tetrapak 

Correlato [[Avanzamento progetto TetraPak]]

---
## PREMESSA

**Parallelismo**: consiste nell'eseguire molteplici operazioni allo stesso tempo. Il **multiprocessing** è un metodo di implementare il parallelismo, e consiste nel distribuire i task da eseguire sulle unita di processamento del computer (CPU, o core).

**Concurrency**: termine leggermente più ampio, che significa che molteplici task hanno la capacità di eseguire in maniera sovrapposta. Per questo motivo si può dire che la concorrenza non implica parallelismo.

**Threading**: modello di esecuzione concorrente in cui molteplici thread sono eseguiti a turno per portare avanti ognuno il proprio task. 

Mentre in un _CPU-bound task_ i core del computer eseguono continuamente, un _IO-bound job_ è caratterizzato dal susseguirsi di tempi di attesa per ottenere input/output.

**async IO** usa un design single-process, single threaded. È usato il _cooperative multitasking_.
async IO rende l'esecuzione concorrente, nonostante si usi un unico thread in un singolo processo. Le coroutines (central feature di async IO) possono essere schedulate in modo concorrente, ma non sono eseguite contemporaneamente.

Routine asincrone possono essere messe in pausa mentre attendono l'ultimo risultato/output e lasciano nel frattempo altre routine eseguire.

![[Pasted image 20221104145431.png]]

---

## DEFINIZIONE COROUTINE

**Coroutine**: funzione che può sospendere la propria esecuzione prima di raggiungere il _return_. Essa può passare indirettamente il controllo ad un altra coroutine per un certo intervallo di tempo.

==_async_== introduce una coroutine nativa o un generatore asincrono. 
==_await_== sospende l'esecuzione della coroutine, fino a quando riceve il risultato della funzione considerata. Nell'attesa viene eseguita un altra funzione.
Può essere usata solo su oggetti "awaitable", che sono altre coroutine o oggetti che hanno definito al loro interno un metodo __await()__ che ritorna un iteratore.

![[Pasted image 20221104161238.png]]




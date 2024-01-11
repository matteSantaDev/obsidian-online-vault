#informatica #tetrapak 

---

Un loader è la parte di un sistema operativo responsabile del caricamento di programmi e librerie. È una delle fasi essenziali nel processo di avvio di un programma poiché inserisce i programmi in memoria e li prepara per l’esecuzione da parte del sistema operativo.

Loader funziona leggendo il contenuto del file eseguibile e quindi memorizzando queste istruzioni nella RAM, così come tutti gli elementi della libreria che devono essere in memoria per l’esecuzione del programma. Questo è il motivo per cui viene visualizzata una schermata iniziale subito prima dell’avvio della maggior parte dei programmi, che spesso mostra ciò che accade in background, ovvero ciò che il loader sta attualmente caricando in memoria.

Il caricamento comporta la lettura del contenuto del file eseguibile che contiene le istruzioni del programma e quindi l’esecuzione di altre attività preparatorie necessarie per preparare l’eseguibile per l’esecuzione, il tutto richiede da pochi secondi a minuti a seconda delle dimensioni del il programma che richiede di essere eseguito.

Le responsabilità di un loader includono:

-   Collega il punto di partenza del programma e collega qualsiasi altra libreria richiesta.
-   Avvia i registri.
-   Convalida il programma per requisiti di memoria, autorizzazioni, ecc.
-   Copiare i file necessari come l’immagine del programma o le librerie richieste dal disco alla memoria.
-   Salta al punto di inizio del programma in memoria.

**Infine, il loader può essere di tre tipi:**

-   Carico assoluto
-   Caricamento riposizionabile
-   Caricamento dinamico in fase di esecuzione
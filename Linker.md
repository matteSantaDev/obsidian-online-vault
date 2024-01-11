#informatica #tetrapak 

---

Programma che prende uno o più oggetti generati da un [[Compilatore|compilatore]] o un assemblatore e li combina in un unico programma eseguibile.
I programmi per computer sono generalmente costituiti da più moduli che possono separare file oggetto, ciascuno dei quali è un programma per computer compilato. Il programma nel suo complesso si riferisce a questi file compilati separatamente utilizzando simboli. 
Il linker combina questi file separati in un unico programma unificato, risolvendo i riferimenti simbolici man mano che procede.
Il collegamento viene eseguito sia in fase di compilazione quando il codice sorgente viene tradotto in codice macchina che in tempo di caricamento, quando il programma viene caricato in memoria dal loader. Il collegamento viene eseguito nell’ultimo passaggio della compilazione di un programma.

Il collegamento può essere di due tipi:

-   **Collegamento statico**
-   **Collegamento dinamico**

**Il collegamento dinamico** viene eseguito durante il tempo di esecuzione. Questo collegamento viene eseguito inserendo il nome di una libreria condivisibile nell’immagine eseguibile. C’è più possibilità di errori e guasti. Richiede meno spazio di memoria poiché più programmi possono condividere una singola copia della libreria.   

**Il** **collegamento** **statico** viene eseguito durante la compilazione del programma sorgente. Prende la raccolta di file oggetto riposizionabili e argomenti della riga di comando e genera file oggetto completamente collegati che possono essere caricati ed eseguiti. Le due funzioni principali del collegamento statico includono:

-   Trasferimento del codice e della sezione dati e modifica dei riferimenti ai simboli nella posizione di memoria trasferita.
-   Associazione di ogni riferimento di simbolo con esattamente una definizione di simbolo. Ogni simbolo ha un’attività predefinita.

![[Pasted image 20230215104840.png]]

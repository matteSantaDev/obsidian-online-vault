#informatica 

Errore di tipo [[buffer overflow]] che avviene quando un programma prova a usare uno spazio di memoria dello [[local stack memory | stack]] più grande di quello effettivamente allocato a quello stesso stack.
Lo stack è un buffer di memoria di dimensione fissa, nel quale sono salvate variabili locali di funzioni e che ritorna indirizzi di memoria durante l'esecuzione del programma.

Il call stack ha una struttura LIFO (Last In First Out). ==Ad ogni funzione è assegnata una struttura stack per salvare variabili locali e memory address.== Lo stack mantiene la sua struttura, rimanendo salvato in memoria, fino a che la funzione non ha finito di eseguire. Dopo questo la memoria è liberata per lasciare spazio ad altri stack frames.

### CAUSE 
- utilizzo di funzioni ricorsive. Ogni volta che la funzione chiama se stessa occupa un ulteriore spazio della stack memory. Se la funzione è eseguita troppe volte può finire tutta la memoria disponibile, causando uno stack overflow.
- assegnazioni di una quantità di dati eccessiva a variabili presenti nello stack frame. Gli array sono variabili paraticolarmente suscettibili ad errori di stack overflow, specialmente se non è stata implementata alcuna logica per prevenire che l'eccesso di dati sia scritto sull'array.



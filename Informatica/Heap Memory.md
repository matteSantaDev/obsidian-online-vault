#memoria #informatica 

Anche chiamata _memoria dinamica_ è un alternativa alla [[local stack memory]]. Essa è una porzione di memoria allocata dinamicamente, e che rimane occupata fino a che è deallocata o il programma termina.

La memoria locale è automatica; le variabili sono allocate automaticamente quando una funzione è chiamata e sono deallocate automaticamente quando la "funzione esce".

La memoria heap differisce da quella locale perchè con essa il programmatore richiede esplicitamente che spazi di memoria siano allocati. Il blocco di memoria deve essere di una specifica dimensione, solitamente determinata in modo automatico dalla dimensione dell'oggetto che viene creato.

In C e C++ un oggetto salvato nella memoria heap viene eliminato ( e lo spazio di memoria deallocato) solo quando il programmatore richiede in modo esplicito che tale spazio di memoria sia liberato.

**memory leak**: eliminare tutti i riferimenti ad una posizione di memoria senza deallocarla. Molti programmi hanno questo bug che causa il crash se il programma è usato per un tempo prolungato.

Java elimina questo tipo di errori gestendo la deallocazione della memoria heap in modo automatico, usando la funzione di _garbage collection_. Il lato negativo è la lentezza del processo che avviene in maniera inaspettata.
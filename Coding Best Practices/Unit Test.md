#best-practices 

---

Attività di collaudo di singole unità di un software. 
Per unità si intende il minimo componente di un programma dotato di un funzionamento autonomo. A seconda del paradigma di programmazione e del linguaggio può corrispondere a una _funzione_ (nella programmazione procedurale), a una singola _classe_ o ad un singolo _metodo_ (nella programmazione ad oggetti).

Viene fatto in modo automatico sfruttando librerie per l'unit testing.

Vantaggi:
- sempifica le modifiche, verificando che ogni volta le varie unità continuino a funzionare.
- semplifica l'integrazione di uovi moduli. Necessari però sempre anche test umani oltre che le procedure automatizzate.
- I test sono documentazione intrinseca dell'unità, perché incorporano le caratteristiche critiche per il successo di un unità di codice. Tali caratteristiche indicano l'uso appropriato dell'unità e i comportamenti errati che devono essere identificati nel suo funzionamento. Pertanto lo unit testing documenta tali caratteristiche, sebbene in molti ambienti questi non possono costituire la sola documentazione necessaria. In compenso, la tradizionale documentazione diventa spesso obsoleta a causa di successive modifiche del codice non documentate.
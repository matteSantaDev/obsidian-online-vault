#C 

---

contiene l'indirizzo di una variablile. Strumento fondamentale per il [[Memory management del linguaggio C]].

Quando una variabile è dichiarata, un blocco di byte viene riservato nella memoria. Il pointer di una variabile è l'indirizzo/address del primo di questi byte.

I pointer possono essere creati per ogni tipo di variabile: primitive (come **int**, **char**, **double**), custom data type creato con uno **struct**, o anche un altro pointer.

![[Pasted image 20221005155832.png]]
![[Pasted image 20221005155805.png]]

* usato per dichiarare un pointer
**&** è il **reference operator**, e permette di ottenere l'indirizzo di memoria (non il contenuto dell'indirizzo di memoria)





![[Pasted image 20221005171154.png]]
_ptr_ salva l'address della variabile _x_, il valore ottenuto dereferenziando _ptr_ è il valore di _x_. 
È possibile anche dereferenziare un pointer, e usare il suo contenuto.

\*\ ptr è il valore che è salvato nello spazio di memoria di ptr




POINTER ARITHMETICS
I pointer sono speciali tipi di variabili (è uno speciale tipo di intero), per questo motivo è possibile effettuare operazioni aritmetiche su di esse. Le uniche che si possono effettuare sono **addizione** e **sottrazione**. 

Si può sommare solo un pointer ad un intero, mai più pointer tra loro.
![[Pasted image 20221005173629.png]]
sommando un numero _x_ ad un pointer il nuovo indirizzo sarà spostato di 

>>           _x_ * (dimensione del tipo di dato in byte)

Accedere agli array usando l'aritmetica sui pointers è una pratica pericolosa perchè accedendo a address al di fuori dei limiti dell'array si corrompono i dati salvati (nel caso di scrittura), se invece si effettua una lettura saranno ritornati valori random.

![[Pasted image 20221005175508.png]]
esempio in cui dereferenzio il pointer per accedere ad ogni valore nell'array, poi incremento il pointer per accedere al valore di una posizione successiva dell'array.



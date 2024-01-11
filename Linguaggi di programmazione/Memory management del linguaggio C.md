#memoria #C

La memoria è una risorsa finita e costosa, se un programma ne utilizza molta avrà basse performance. Per questo motivo è necessario usarne il meno possibile.

In un computer la memoria fisica è un componente hardware dotato di complessi circuiti elettrici che sono usati per memorizzare informazioni. Tali componenti sono la [[RAM]] (memory stick) o la [[ROM]] (hard disk).

![[Pasted image 20221006093927.png]]
Schema con cui si rappresenta la RAM. L'unità fondamentale della memoria è il byte. Parlando di RAM solitamnete si parla nell'ordine dei gigabyte (GB).
Ogni byte ha il proprio indirizzo  o _memory address_ usato per salvare i dati. Gli address sono enumerati usando il sistema esadecimale. 

VARIABILI
Una variabile occupa un certo blocco di byte nella memoria.
int -> 4 byte
float -> 8 byte
double -> 8 byte
char -> 1 byte

>> sizeof()

mostra la memoria occupata da una variabile .



MEMORY ALLOCATION
La memoria usata da un programma è organizzata in 2 principali categorie: **[[local stack memory|stack]]** e **[[Heap Memory|heap]]**.

Lo **stack** è una sezione di memoria che è altamente ordinata e i dati salvati sulla memoria stack sono disponibili solo in un determinato ambito.
La **heap** è un sezione di memoria non ordinata come lo stack.

Quando si crea una variabile nel modo tradizionale si alloca staticamente una porzione di memoria nello stack. Quello spazio di memoria è poi deallocato automaticamente dal programma una volta che la variabile non serve più.

Usando la memoria heap è possibile usare quanta memoria disponibile si vuole, e essa rimane allocata fintanto che non si dealloca esplicitamente. Questa modalità infatti è chiamata _allocazione dinamica della memoria_. Se ci si scorda di deallocare la memoria è possibile creare dei memory leaks.


Per allocare dinamicamente la memoria C fornisce alcune funzioni, presenti nella libreria _stdlib_.
	- **malloc()**: Use this function to reserve as many bytes as you want on the heap
	- **calloc()**: Use this function to reserve memory for some number of `int`s, `double`s, or any other data type
	- **realloc()**: Use this function to expand or contract a block of reserved memory (reserved by either `malloc()` or `calloc()`).
	- **free()**: Use this function to release previously allocated memory.

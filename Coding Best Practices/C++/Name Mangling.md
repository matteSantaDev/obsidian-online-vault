#cpp #best-practices 

---

Algoritmo che il compilatore usa per generare un nome univoco per le funzioni (anche quelle definite con lo stesso nome, ma con parametri differenti). È il motivo per cui è possibile implementare il principio del polimorfismo.
Il name mangling consente al linker di eseguire la chiamata alla funzione corretta.
L'algoritmo di name mangling varia in ogni compilatore, ma è sempre basato sul **tipo** e sul **numero** di argomenti della funzione.

A causa della differenza tra gli algoritmi di name mangling, non è possibile chiamare funzioni C++ da C.

### Extern C
Se si compila il codice C++ con **extern C**, applicabile a funzioni e variabili globali, è usato per sopprimere il name mangling applicato alle funzioni (o variabili ??).
Avendo una serie di funzioni overloaded, può essere applicato ad una sola di queste, ma **applicando Extern C su una funzione C++ è possibile rendere invocabile tale funzione da C o anche da altri linguaggi**.
Deve essere applicata sia su dichiarazione, sia su definizione.
```cpp
//in .h file
extern "C" <function declaration>;

//in .cpp file
extern "C" <function definition> {

}
```

Per vedere i nomi "mangled" generati per le funzioni o le variabili globali si possono fare i seguenti passaggi in VisualStudio:
- `Project`
- `Function Overloading Properties...`
- Expand `Linker`
- `Debugging`
- Scegliere opzione `Yes(/MAP)` in `Generate Map File` option
- fare rebuild del programma
- aprire la directory del programma
- aprire la cartella `Debug`
- aprire il file con l'estensione `.map`

Il risultato è che il nome della funzione non viene magled, e può essere chiamata anche da C o da altri linguaggi.

Si può applicare Extern C anche a più funzioni alla volta:
```cpp
extern "C" {
	<function definition> {
	
	}
}
```

Se la dichiarazione ha già specificato l'uso di extern C, non è necessario esplicitarlo nella definizione.



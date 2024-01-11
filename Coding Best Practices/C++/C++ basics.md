#best-practices #cpp 

---

### Caratteristiche generali
Linguaggio general purpose. Adatto alla [[Principi programmazione ad oggetti o Object Oriented Programming (OOD)|programmazione ad oggetti]], è stato creato da Bjarne Stroustrup. Simile a C, ma ha più features.
In C++11 implementa smart pointers (pointers a indirizzi di memoria che si liberano in modo automatico).
È usato per stabilità, compatibilità e performance, funziona in tutti i sistemi operativi e non necessita di un separato runtime per funzionare. Il memory footprint per questo motivo è molto basso e può operare senza problemi in ambienti vincolati (constrained environments), in cui ad esempio c'è poca memoria o una CPU molto lenta.
Un codice scritto in C++ funziona con modifiche minime in molte piattaforme differenti.

___
### Visual Studio Project Structure
![[Pasted image 20231130094727.png]]
Quando si crea un nuovo progetto in Visual Studio in `Templates/Visual C++` seleziono `Win32`, e poi `Win32 Console Application`.
Per mostrare `Solution Explorer` attivare la checkbox in `View`.
Per aggiungere file al progetto vuoto, nell'interfaccia `Solution Explorer` schiaccio destro su `First Program` e seleziono `Add -> New Item`.

---
### Script base
```cpp
#include <iostream> //per abilitare comunicazione 

int main() {
	using namespace std;
	cout << "hello" << "C++" << endl;
	return 0;
}
```
Il main viene invocato automaticamente durante il run time.
È necessario usare il namespace per sfruttare il `cout`.
`endl` è un manipolatore che inserisce una riga.
Faccio build.
In `Debug` faccio `Start Without Debugging`.

Si può fare anche in questo modo:
```cpp
int main() {
	using namespace std;
	std::cout << "hello" << "C++" << std::endl;
	return 0;
}
```

___
### C++ Build
Processo che viene eseguito dal compilatore per convertire il programma in un file eseguibile.

##### Steps
1) Preprocessing
	- processati gli statements che iniziano con `#`.
	- se sono stati inclusi header file nel codice sorgente, il preprocesser sostituisce l'`include` con il contenuto dell'header.
	- i macros sono espansi.
2) Compilazione
	- il compilatore controlla che non ci siano errori di sintassi
	- viene convertito il codice in object code, e viene generato un file con estensione `.obj`; in alcune piattaforme l'estensione è omessa.
3) Linking
	- l'object code è linkato alle standard libraries.

Se non ci sono errori durante il linking viene generato un file eseguibile, eseguibile dalla command line o dall'explorer.

___
### Tipi primitivi
- **Aritmetici**
	- *integral*
		- `bool`
		- `char` / `wchar_t` / `char16_t` / `char32_t`
		- `short`
		- `long`
	- *floating point*
		- `float`
		- `double`
- **Void**

**Modifiers**: tipi che consentono di modificare il comportamento dei tipi primitivi. Possono essere:
- `signed` (solo con *integral*)
- `unsigned` (solo con *integral*)
- `short`
- `long`
==Non tutti i tipi possono essere modificati con i modifiers==.

**Qualifiers**:
- `const`
- `volatile`
- `static`

Tutti i tipi occupano spazio nella memoria, ma lo spazio occupato può variare dalla piattaforma.

![[Pasted image 20231130144433.png]]
![[Pasted image 20231130144448.png]]
![[Pasted image 20231130144514.png]]
Macros con valori massimi e minimi dei vari tipi. È necessario includere `<climits>` o `<limits.h>`.
![[Pasted image 20231130144531.png]]
In `<cfloat>` o in `<float.h>` invece sono presenti i range massimi e minimi dei valori di float.

___
### Dichiarazione delle variabili
Viene specificato il tipo e poi il nome della variabile. La variabile è anche chiamata **identifier**.
Si possono dichiarare differenti variabili sulla stessa linea.
Si può fare l'inizializzazione delle variabili solo quando sono state dichiarate. 
In C++ una variabile dichiarata ma non inizializzata genera un errore.
```cpp
//Schalar types
int i = 39;
char ch = 'a';
float f = 1.234f;
bool flag = true; //true o 1, flase o 0
double d = 521.349;- void AddVal(int *a, int *b, int *result); //Add two numbers and return the sum through the third pointer argument

//Vector types
int arc[5];
int arr1[5] {1,2,3,4,5};
```

---
### Inizializzazione delle variabili

In C++11 è stata adottata la *uniformed initialization*, ovvero è possibile inizializzare una variabile di qualsiasi tipo con `{}`.

Possono essere usate 3 tipologie di inizializzazione:
- *value initialization* `obj{};`
- *direct initialization* `obj{v};`
- *copy initialization* `obj = v;`

Inizializzazione standardizzata con le `{}` per tutti i tipi, che porta diversi vantaggi:
- sintassi comune per i diversi tipi.
- forza l'inizializzazione sia di scalari, sia di array.
- previene i bug se si inizializza un tipo con un valore di un altro tipo. O anche [[Narrowing Convertions]].
```cpp
//premature types
int a2 = 0;               //copy initialization
int a3(5);                //direct initialization
int b1{};                 //value initialization
int b2();       //WARNING INIZIALIZZO UNA FUNZIONE     most vexing patse
int b3{6};                //direct uniformed initialization

std::string s1;
std::string s2("C++");

//array types
char d1[8] = {'/\0'};
char d1[8] = {'a','d','f'};      //aggregate initialization
char d1[8] = {'dwfwf'};
char d1[8] = {};                 //inizializzo al valore di default
char d1[8]{};                    //value uniformed initialization
char d1[8]{"Hello"};             //value uniformed initialization
```
Evitare l'inizializzazione per copia, sia per i premature types, sia per gli array. Usare invece l'inizializzazione uniformata con le `{}`.

Si può usare l'inizializzazione standard anche quando si crea una variabile nella memoria heap.
```cpp
int *p1 = new int{};
int *p2 = new char[8]{};
int *p2 = new char[8]{"Hello"};
```

---
### Console I/O
`std::basic_ostream` è responsabile per l'output sulla console, mentre `std::basic_istream` è responsabile per l'input.
Queste classi sono definite anche come tipi `std::ostream` e `std::istream`.
`std::cout` è un oggetto della classe `std::ostream`, mentre `std::cin` è un oggetto di `std::istream`.

Per leggere dalla console si usa il simbolo `>>`
Per scrivere sulla console si usa il simbolo `<<`

Questi oggetti sono definiti nell'header `<iostream`.

```cpp
#include <iostream>

int main() {
	using namespace std;

	//scrivo in console
	cout << "hello worls\n" << 45 << 8.2f << endl;

	//leggo dalla console
	int age;
	cout << "Tell me your age:";
	cin >> age;
	cout << "Your age is :" << age << endl;

	//leggo una parola dalla console
	char buff[512];
	cout << "Whats your name?:";
	cin >> buff;
	cin.getline(buff, 64, ''\n')
	cout << "Your name is " << buff << endl;
	
	return 0;
}
```

----

### Funzioni
Set di brani di codice all'interno delle parentesi `{}`, anche chiamato corpo della funzione.
Le funzioni esistono per ridurre la ripetizione di codice e incrementare la riusabilità e la modularità, riducendo la leggibilità e la complessità. Con le funzioni è possibile creare moduli riutilizzabili di codice.
Non ci sono limiti al numero di volte in cui si chiama una funzione.

Una funzione può ricevere parametri in ingresso (usabili al suo interno) e può ritornare un unico valore (variabile di un tipo).
```
<return type> <nome funzione>(<parametri) {
	<body
}
```
Se la funzione ha parametri in input devono essere inseriti ogni volta che la funzione è invocata.

Nel seguete esempio il compilatore darebbe errore perchè la funzione non è stata definita quando è invocata (l'errpre è causato nello specifico dla fatto che il nome della funzione non è associato ad un type).
```cpp
int main() {
	int result = Add(x,y);
}

int Add(int x, int y) {
	int sum = x + y;
	std::cout << "Sum is:" << sum << std::endl;
	return sum;
}
```

Per consentire la compilazione è necessario dichiarare la funzione prima del brano di codice che invoca la funzione.
```cpp
//dichiarazione della funzione, parte che si mette in un file header
int Add(int x, int y);

//parte del codice in cui è invocata
int main() {}

//definizione, questa parte si porta in un file cpp
int Add(int x, int y) {}
```
Si includono solo header files, mai source file `.cpp`, perché il file avrà molteplici definizioni di funzione.

Un altro modo per far compilare correttamente è posizionare la definizione della funzione prima nel brano in cui si invoca la funzione. Soluzione usata solo per funzioni **template** e **inline** (==non per funzioni comuni==.

La struttura file usata è:
- Header Files
- Source Files

----

### Function overloading
Se differenti funzioni hanno lo stesso comportamento (sommano dei numeri), dovrebbero avere tutte lo stesso nome. In modo quindi di ridurre la complessità si assegna a funzioni differenti che fanno la stessa cosa (con parametri differenti, in numero o tipo) lo stesso nome. Definizione del [[Principi programmazione ad oggetti o Object Oriented Programming (OOD)#Polymorphism|polimorfismo]] statico.
```cpp
#include <iostream>

int Add(int a, int b) {
	return a + b;
}
int Add(double a, double b) {
	return a + b;
}

int main() {
	using namespace std;
	int result1 = Add(3, 5);
	cout << result << end1;
	double result2 = Add(3.1, 6.2);
	return 0
}
```
Il function overload funziona sempre anche quando gli input sono puntatori o reference.
```cpp
void Print(int *x) {

}

void Print(const int* x) {

}
```

Usando funzioni con  lo stesso nome è necessario che il compilatore distingua le funzioni identificativi univoci ([[Name Mangling]])

---

### Pointer
Un pointer tiene in memoria l'indirizzo di memoria di un'altra variabile.
Viene usato per accedere indirettamente ad una variabile.
Non è necessario che sia inizializzato nella dichiarazione, ma è una buona pratica.
```cpp
int * ptr;
int *p1, *p2, *p3, x;    //l'ultima è una variabile non un pointer
```

### Address
Per ottenere l'indirizzo di una variabile (che voglio salvare in un pointer) uso l'operatore `&`.
Quando l'operatore `&` è applicato a qualsiasi variabile si ottiene l'indirizzo di memoria dove è salvata la variabile.

```cpp
usinf namespace std;
int x = 10;
cout << &x;

int *ptr = &x;
cout << ptr;
```
Quando si stampano `x` e `ptr`, il risultato in entrambe i casi è che viene stampato l'indirizzo di memoria. 
Il type della variabile deve coincidere con il type del pointer.
Quando si vuole creare un pointer che punta a variabili di tipi diversi si usa il tipo `void`.

==NB== Quando un pointer è dichiarato ma non inizializzato contiene un indirizzo di memoria random.

### Dereference
Dopo aver assegnato una variabile ad un pointer è possibile accedere al valore nell'indirizzo di memoria salvato nel pointer usando il *dereference operator* ( `*` ). Esso consente la lettura o la scrittura indiretta della variabile tramite il pointer.
```cpp
int x = 10;
int *ptr = &x;
*ptr = 5;           //assegno il valore 5 all'address di x
int y = *ptr;       //leggo un valore dall'address di x
```

Se il ponter non viene inizializzato si può inizializzare a `NULL`. Nella maggior parte dei compilatori è equivalente a 0, e viene usato per inizializzare i pointers.
In C++11 è stato introdotto il nuovo tipo `nullptr`, che è più sicuro di `NULL`. Quindi è sempre meglio evitare l'uso di `NULL`.
==NB== non si può LEGGERE o SCRIVERE da/in un pointer nullo (anche se il pointer può essere inizializzato a nullo).
_____
### Reference Type
Definisce un nome alternativo della variabile (o alias); viene creato con l'operatore `&` durante la dichiarazione e necessita sempre di un inizializzatore (_referent_), il quale deve essere una variabile.
La reference può essere usata per modificare la variabile indirettamente, come un puntatore.
La reference ==NON è una nuova variabile, ma è solo un altro nome per la variabile.==
![[Pasted image 20231211152237.png]]
```cpp
int x = 10;
int &ref = x;      //x è il referente, ref è la reference
```
Se cambio il valore di `x` viene modificata anche la reference `ref`. Gli address di memoria a cui puntano il referente e la reference sono gli stessi.

##### DIFFERENZE REFERENCE VS POINTER
Reference:
- necessita sempre di un inizializer.
- l'inizializzatore deve essere una variabile.
- non può avere valore `nullptr`.
- legato al valore del referente.
- non occupa memoria perché ha lo stesso address del referente.
- per accedere al valore non è necessario dereferenziarlo.

Pointer:
- l'initializer è opzionale.
- l'inizializzatore non deve essere necessariamente una variabile.
- può essere `nullptr`.
- può puntare ad altre variabili.
- ha il proprio storage, per questo ha un address differente.
- è necessario usare l'operatore `*` (dereference) per accedere al valore salvato.

##### Come decidere se usare Pointers o Reference
**Pointers**
```cpp
void Swap(int *x, int *y) {
	int temp = *x;
	*x = *y;
	*y = temp;
}

void Swap(int &x, int &y) {
	int temp = x;
	x = y;
	y = temp;
}

int main() {
	using namespace std;
	int a = 5, b= 10;
	
	Swap(&a, &b);    //pointer
	Swap(a, b);    //reference
	
	count << "a:" << a << "\n";
	count << "b:" << b << "\n";
	return 0;
}
```
Se usassi solo le variabili verrebbero invertiti solo i valori delle copie locali create all'interno della funzione, mentre usando i puntatori si invertono i valori delle variabili reali.
La stessa cosa si può fare passando il reference type, con il vantaggio che non si devono dereferenziare i valori nella funzione.
Le reference non potendo essere `nullptr` non fanno compilare il codice, mentre i pointer possono essere `nullptr`. Questo in alcune situazioni porta a crush del programma perché magari si cerca di accedere ad un pointer nullo. Quindi ==quando si usano i pointer è necessario controllare se il valore è NULLO==.
____
### `const` Qualifier
Parola chiave che rende una variabile costante, in tale modo il valore non può essere modificato. Tentativi di modifica causano errore durante la compilazione.
La keyword `const`è qualificata alla dichiarazione, ma necessita dell'inizializzazione.

Si usano i macros, ma dato che non viene definito il tipo è meglio usare le costanti. I macros infatti non sono **type safe** e non hanno uno **scope**.

`const` sono spesso usati con le reference, quando viene passato un parametro in una funzione.
```cpp
const float pi = 3.141f;
```

Se un puntatore punta ad una variabile costante, allora anche il puntatore dovrà essere costante.
```cpp
const int CHUNK_SIZE = 512;
const int *ptr = &CHUNCK_SIZE;
*ptr = 1;      //ERRORE perché è const
```
Si può rendere anche costante il solo pointer, in questo modo si impedisce di modificare l'indirizzo.
```cpp
const int *const ptr = &CHUCK_SIZE;
```

Si possono rendere costanti anche le reference
```cpp
const int &ref = 5;
```
Così facendo è possibile fare:
```cpp
void Print(const int &ref) {
	using namespace std;
	cout << ref << end1;
}

int main() {
	using namespace std;
	int x = 5;
	Print(1);
	return 0;
}
```









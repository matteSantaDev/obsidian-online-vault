#best-practices #cpp

---
#### Sommare vettore in modo conciso
```cpp
#include <vector>
#include <numeric>

int sum(std::vector<int> nums) {
  return std::accumulate(nums.begin(), nums.end(), 0);
}
```
accumulate prende come prmo parametro l'elemento da cui deve cominciare (in questo caso il primo del vettore), come secondo parametro l'ultimo elemento che considera per la somma (in questo caso l'ultimo). Il terzo parametro è il valore a cui viene sommata la somma dei numeri eseguita.


#### Rimozione degli spazi in una stringa
```cpp
#include <iostream>
#include <string>
#include <algorithm>

x.erase(std::remove(x.begin(), x.end(), ' '), x.end());
```


- `std::remove(x.begin(), x.end(), ' ')` sposta tutti i caratteri spazio nella parte finale del vettore e restituisce un iteratore che indica la nuova fine del vettore dopo la rimozione degli spazi.
- `x.erase(...)` utilizza questo iteratore restituito da `std::remove` per eliminare gli spazi dalla stringa.

Quindi, dopo l'esecuzione di questa riga di codice, la stringa `x` conterrà il testo originale senza spazi. È un modo conciso ed efficiente per rimuovere gli spazi da una stringa in C++.

#### Substring

```cpp
#include <string.h>

string r = s1.substr(3, 2);
```

Primo parametro è la **posizione** da cui inizia il substring, secondo parametro è la **lunghezza** della sottostringa.

#### Algoritmo di Brian Kernighan (conta dei bit a 1 in un numero binario)
```cpp
unsigned int countBits(unsigned long long n){ 
	int count = 0;
	while (n) { 
		n &= (n-1);
		++count;
	}
	return count;
}
```

Il presupposto dell'algoritmo è che i numeri successivi siano differenti per un bit unico messo ad 1.
![[Pasted image 20231004102737.png]]
Con l'operazione `n &= (n-1)` faccio una sorta di moltiplicazione di due numeri binari. Infatti se in una determinata posizione ci sono due 1, il valore nel numero finale rimane 1. Mentre se c'è almeno uno 0, il risultato finale è 0.

Nel codice sopra quindi conto ogni operazione di annullamento di un 1, che equivale a contare gli 1 del numero binario ma con meno cicli totali.

---


#### Cambio ordine ad elementi di un vector
```cpp
swap(values[i], values[j]);
```

#### Algoritmo di sostituzione delle posizioni in un vector
Per ottimizzare l'algoritmo si usano 2 cicli for. Il primo è completo, mentre il secondo cicla in modo decrescente, partendo dal fondo e finendo all'indice a cui è arrivato il primo ciclo.
Dato che il secondo ciclo deve essere eseguito più volte, l'inizio viene spostato verso "sinistra" abbassando l'indice di partenza del ciclo decrescente.


N.B. si può inserire una condizione secondaria di fine del ciclo for:
```cpp
for(int j=k; j>i && values[i]>0; j--) {
    ...
}
```

#### Concatenare vector

```cpp
vector1.insert( vector1.end(), vector2.begin(), vector2.end() );
```
---
#### Ottenere i numeri che compongono un intero

I numeri si ottengono come il resto della divisione per 10.
```cpp

int numero = 12345; // L'intero da cui vogliamo ottenere le cifre
int cifra;

while (numero > 0) {
	cifra = numero % 10; // Otteniamo la cifra meno significativa
	std::cout << cifra << " "; // Stampa la cifra
	numero /= 10; // Rimuoviamo la cifra meno significativa
}
```

---

### Algoritmo di Floyd (lepre - tartaruga)
Determina se c'è un ciclo in una linked list o in una lista.
Se la linked list ha questa struttura:
![[Pasted image 20231006142702.png]]
un modo per determinare la presenza di cicli è con l'algoritmo di Floyd. Se si hanno 2 pointers, muovendone uno a velocità doppia rispetto all'altro (uno avanza di una posizione alla volta, l'altro di 2 posizioni alla volta), se c'è un ciclo i 2 i 2 pointers si incontreranno sullo stesso elemento. Se non si intersecano mai non c'è il ciclo.

Dopo che i 2 puntatori si sono incontrati, il puntatore **turtle** viene spostato in avanti di un passo alla volta. Un contatore **count** tiene traccia dei numeri di passi. Si esegue un secondo `while` fino a che i puntatori **turtle** e **rabbit** si incontrano nuovamente (in questo caso viene spostato un solo contatore). Il numero di passi effettuati è la lunghezza del ciclo.
```cpp
int getLoopSize(Node* startNode)
{
  Node* turtle = startNode;
  Node* rabbit = startNode->getNext();
  while(turtle != rabbit) {
    turtle = turtle->getNext();
    rabbit = rabbit->getNext()->getNext();
  }
  turtle = turtle->getNext();
  int count = 1;
  while(turtle != rabbit) {
    turtle = turtle->getNext();
    count++;
  }
  return count;
}
```


---

### Leggere ottetti da numero in 32 bit
```cpp
std::string uint32_to_ip(uint32_t ip)
{
  return 
    std::to_string(ip>>24 & 255) + "." + 
    std::to_string(ip>>16 & 255) + "." +
    std::to_string(ip>>8 & 255) + "." +
    std::to_string(ip & 255);
}
```
Per ottenere un ottetto devo shiftare di tanti byute quanto l'ottetto di sta da "sinistra".


---

### Conversione da `std::string` a `int`
```cpp
int number = std::stoi(str);
```


---

### Distruttore


---

### Versionamento

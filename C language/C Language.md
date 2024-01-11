
----

### auto Keyword
È usato per consentire il salvataggio automatico di una variabile. Elemento del linguaggio rimasto da linguaggi pre-C.

```c++
auto <identifier> = <initializer>;

auto i = 5;
auto sum = i + j;

auto sum = i + 4.3f;
```
Se ci sono più tipi, auto assegna il tipo dell'ultimo elemento presente nell'espressione.
Se la variabile è inizializzata con il valore ritornato da una funzione, auto assume il tipo del return.

Si possono usare i qualifier `static` e `const`, ma quando la variabile.
==Evitare di usare `auto` con indirizzi o reference.==


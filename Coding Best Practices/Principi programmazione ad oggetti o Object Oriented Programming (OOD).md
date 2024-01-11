#best-practices 

___

![[Pasted image 20231005110419.png]]

### Encapsulation
Meccanismo con cui si nascondono implementazione dati restringendo l'accesso ai metodi pubblici. Le variabili di istanza sono mantenute private e vengono creati metodi di accesso pubblici per raggiungere questo obiettivo.
Un esempio è nascondere gli attributi `name` e la data di nascita `dob` nella classe `Employee`, creando metodo pubblici per accedere ai valori, ma non alle variabili.

```cpp
public class Employee {  
    private String name;  
    private Date dob;    
    public String getName() {  
        return name;  
    }    
    public void setName(String name) {  
        this.name = name;  
    }    
    public Date getDob() {  
        return dob;  
    }    
    public void setDob(Date dob) {  
        this.dob = dob;  
    }  
}
```
Le keyword `private`, `public` e `protected` sono **access specifiers**, i quali definiscono come i membri di una classe (metodi ed attributi) possono essere acceduti. In C++ ci sono tre access specifiers:
- `public` : i membri sono accessibili dall'esterno della classe.
- `private` : i membri non possono essere acceduti o visti dall'esterno della classe (cosa che succede anche se non si specifica l'access specifier. Di default in C++ i metodi e gli attributi di una classe sono `private`)
- `protected` : i membri di una classe non sono visibili ne accessibili da una classe, ma possono essere acceduti da classi figlie.
```c++
// Base class  
class Vehicle {  
  public:  
    string brand = "Ford";  
    void honk() {  
      cout << "Tuut, tuut! \n" ;  
    }  
};  
  
// Derived class  
class Car: public Vehicle {  
  public:  
    string model = "Mustang";  
};  
  
int main() {  
  Car myCar;  
  myCar.honk();  
  cout << myCar.brand + " " + myCar.model;  
  return 0;  
}
```
### Data Abstraction
Concetto o idea non associato ad una particolare istanza. Usando una classe astratta o un interfaccia si esprime l'intento della classe piuttosto che quella dell'effettiva implementazione.
Una classe non divrebbe conoscere i dettagli interni ad un altra classe al fine di usare quest'ultima; dovrebbe essere necessario solo conoscere l'interfaccia.

### Polymorphism
Un nome, molte forme. Può essere di due tipi:
- statico: implementato il "**method overloading**", ovvero la possibilità in una classe di definire metodi con lo stesso nome, ma con **firme** diverse. La firma di un metodo include il suo nome e il tipo, l'ordine e il numero dei suoi parametri. In altre parole, metodi con nomi identici ma firme diverse possono essere sovraccaricati nella stessa classe. 
   Il compilatore distingue tra i metodi sovraccaricati in base alla loro firma, consentendo di chiamare il metodo appropriato in base ai tipi o al numero di argomenti passati.
   Esempio in java:
```java
public class Calcolatrice {
    int somma(int a, int b) {
        return a + b;
    }

    double somma(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calcolatrice calcolatrice = new Calcolatrice();

        int risultatoInt = calcolatrice.somma(5, 10);
        System.out.println("Risultato somma int: " + risultatoInt);

        double risultatoDouble = calcolatrice.somma(3.5, 2.5);
        System.out.println("Risultato somma double: " + risultatoDouble);
    }
}
```
  Nell'esempio sopra, la classe `Calcolatrice` ha due metodi `somma()`, uno che accetta due interi e uno che accetta due double. Poiché hanno firme diverse, è possibile chiamarli in base al tipo degli argomenti passati. Questo è un esempio di sovraccarico di metodi.


- dinamico: sfrutta il "**method overriding**".
   L'override di un metodo è un concetto nell'ambito della programmazione orientata agli oggetti che si verifica quando una classe figlia (o derivata) fornisce una nuova implementazione per un metodo già definito nella classe genitore (o base). La nuova implementazione nella classe figlia deve avere la stessa firma (cioè lo stesso nome del metodo, lo stesso numero e tipo di parametri) del metodo nella classe genitore.

   L'override di un metodo consente alla classe figlia di personalizzare il comportamento del metodo ereditato dalla classe genitore. Quando si chiama il metodo sulla classe figlia, verrà eseguita la versione del metodo definita nella classe figlia anziché quella nella classe genitore.

   Ecco un esempio in Java:

```java
class Veicolo {
    void accelerare() {
        System.out.println("Il veicolo sta accelerando.");
    }
}

class Auto extends Veicolo {
    @Override
    void accelerare() {
        System.out.println("L'auto sta accelerando.");
    }
}

public class Main {
    public static void main(String[] args) {
        Veicolo veicolo = new Auto();
        veicolo.accelerare(); // Stampa: "L'auto sta accelerando."
    }
}
```

Nell'esempio sopra, la classe `Auto` eredita dalla classe `Veicolo` e ridefinisce il metodo `accelerare()`. Quando chiamiamo `accelerare()` su un oggetto di tipo `Auto`, viene eseguita la versione del metodo definita nella classe `Auto`, mostrando il principio dell'override del metodo.

In questo modo si può scrivere un codice che funziona nella superclass e funzionerà per ogni tipo di sottoclasse.

### Inheritance
L'ereditarietà esprime una relazione "è-un" e/o "ha-un" tra due oggetti. Utilizzando l'ereditarietà, nelle classi derivate possiamo riutilizzare il codice delle superclassi esistenti. In Java, il concetto di "è-un" si basa sull'ereditarietà di classe (utilizzando "extends") o sull'implementazione di interfacce (utilizzando "implements").

Ad esempio, FileInputStream "è-un" InputStream che legge da un file.

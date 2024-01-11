Robert C. Martin
#best-practices 

___

Primi 5 principi dell'[[Principi programmazione ad oggetti o Object Oriented Programming (OOD) | Object Oriented Programming]],  che servono per consentire lo sviluppo di software che sia **mantenibile** ed **estendibile** nel caso il progetto si espanda.
Adottare queste pratiche può anche contribuire a evitare problemi di codice, rifattorizzare il codice e sviluppo software Agile o adattivo.

SOLID sta per:
- S - Principio di Singola Responsabilità
- O - Principio Aperto-Chiuso
- L - Principio di Sostituzione di Liskov
- I - Principio di Segregazione delle Interfacce
- D - Principio di Inversione delle Dipendenze

### Principio Single-Responsability
Una classe dovrebbe avere solo un motivo per cambiare, ciò significa che la classe dovrebbe svolgere solo un compito. La classe ha un unico compito; in questo modo se si decide di modificare la struttura del progetto o aggiungere cose nuove è possibile aggiungere un modulo nuovo che può integrarsi con quelli esistenti senza che questi ultimi debbano essere modificati.

### Principio Open-Closed (OCP)
Le classi, quindi gli oggetti o le entità, devono essere aperte all'estensione, ma chiuse alle modifiche. Deve essere possibile integrare nuovi componenti con quelli esistenti e non modificare quelli esistenti.

### Principio di Sostituzione di Liskov
Se q(x) è una proprietà dimostrabile sugli oggetti di x di tipo T, allora q(y) dovrebbe essere dimostrabile sugli oggetti di y di tipo S, dove S è un sottotipo di T.

Ciò significa che ogni sottoclasse o classe derivata dovrebbe essere sostituibile dalla loro classe base o classe genitore.

### Principio di Interface Segregation
Un cliente non dovrebbe mai essere costretto a implementare un'interfaccia che non utilizza, né i clienti dovrebbero essere costretti a dipendere da metodi che non utilizzano.

### Principio di Dependency Injection (Invertion)
Le entità devono dipendere dalle astrazioni, non dalle concretezze. Questo afferma che il modulo di alto livello non deve dipendere dal modulo di basso livello, ma entrambi devono dipendere dalle astrazioni.

La **Dependency Injection** (DI) è un principio di progettazione e un design pattern utilizzato principalmente per gestire le dipendenze tra le diverse parti di un'applicazione. In termini semplici, consiste nel passare gli oggetti o le dipendenze di cui una classe ha bisogno invece di crearle all'interno della stessa classe.
In pratica, questo significa che, anziché creare direttamente le dipendenze all'interno di una classe, queste vengono "iniettate" o fornite a quella classe da un'entità esterna. Questo rende il codice più flessibile, manutenibile e facilita i test.

Ci sono principalmente tre tipi di dependency injection:
1. **Constructor Injection (Iniezione tramite costruttore):** Le dipendenze vengono passate come parametri al costruttore della classe. Questo tipo di DI rende esplicite le dipendenze richieste dalla classe e assicura che siano disponibili quando un'istanza della classe viene creata.
```javascript
// Esempio in JavaScript usando il costruttore

// Dipendenza esterna
class Database {
  query(data) {
    console.log(`Eseguendo la query: ${data}`);
  }
}

// Classe che richiede la dipendenza tramite costruttore
class DataManager {
  constructor(database) {
    this.database = database;
  }

  performTask(task) {
    this.database.query(`Eseguendo il task: ${task}`);
  }
}

// Utilizzo della classe DataManager con Dependency Injection tramite costruttore
const db = new Database();
const dataManager = new DataManager(db);
dataManager.performTask('Aggiornamento dati');
```

```cpp
#include <iostream>
#include <memory>

// Classe dipendente
class Dependency {
public:
    void doSomething() {
        std::cout << "Doing something in Dependency" << std::endl;
    }
};

// Classe che accetta la dipendenza tramite il costruttore (Constructor Injection)
class MyClass {
private:
    std::shared_ptr<Dependency> dependency;

public:
    MyClass(std::shared_ptr<Dependency> dep) : dependency(dep) {}

    void useDependency() {
        dependency->doSomething();
    }
};

int main() {
    std::shared_ptr<Dependency> dep = std::make_shared<Dependency>();
    MyClass myClass(dep);
    myClass.useDependency();

    return 0;
}
```

2. **Setter Injection (Iniezione tramite setter):** Le dipendenze vengono fornite attraverso i metodi setter della classe. Questo approccio consente di cambiare le dipendenze anche dopo la creazione dell'istanza della classe, ma può rendere meno evidenti le dipendenze richieste.
```js
// Esempio in JavaScript usando i setter

// Classe che richiede la dipendenza tramite setter
class Logger {
  setOutput(output) {
    this.output = output;
  }

  log(message) {
    this.output(`Log: ${message}`);
  }
}

// Utilizzo della classe Logger con Dependency Injection tramite setter
const logger = new Logger();
logger.setOutput(console.log);
logger.log('Un messaggio di log');
```

```cpp
#include <iostream>
#include <memory>

// Classe dipendente
class Dependency {
public:
    void doSomething() {
        std::cout << "Doing something in Dependency" << std::endl;
    }
};

// Classe che accetta la dipendenza tramite un metodo setter (Setter Injection)
class MyClass {
private:
    std::shared_ptr<Dependency> dependency;

public:
    void setDependency(std::shared_ptr<Dependency> dep) {
        dependency = dep;
    }

    void useDependency() {
        if (dependency)
            dependency->doSomething();
        else
            std::cout << "Dependency not set" << std::endl;
    }
};

int main() {
    std::shared_ptr<Dependency> dep = std::make_shared<Dependency>();
    MyClass myClass;
    myClass.setDependency(dep);
    myClass.useDependency();

    return 0;
}
```

3. **Method Injection (Iniezione tramite metodo):** Le dipendenze vengono passate come parametri ai metodi della classe ogni volta che vengono chiamati. Questo tipo di DI è più dinamico e consente di fornire le dipendenze solo quando necessario.
```js
// Esempio in JavaScript usando i metodi

// Classe con un metodo che richiede la dipendenza tramite metodo
class Authenticator {
  authenticate(user, password, authFunction) {
    // Simulazione di autenticazione
    const isAuthenticated = user === 'utente' && password === 'password';
    authFunction(isAuthenticated);
  }
}

// Utilizzo della classe Authenticator con Dependency Injection tramite metodo
const authenticator = new Authenticator();
authenticator.authenticate('utente', 'password', isAuthenticated => {
  console.log(`L'utente è autenticato: ${isAuthenticated}`);
});
```

```cpp
#include <iostream>
#include <memory>

// Interfaccia per la dipendenza
class DependencyInterface {
public:
    virtual void doSomething() = 0;
    virtual ~DependencyInterface() = default;
};

// Classe con l'implementazione della dipendenza
class Dependency : public DependencyInterface {
public:
    void doSomething() override {
        std::cout << "Doing something in Dependency" << std::endl;
    }
};

// Classe che accetta l'interfaccia come dipendenza (Interface Injection)
class MyClass {
private:
    std::shared_ptr<DependencyInterface> dependency;

public:
    void setDependency(std::shared_ptr<DependencyInterface> dep) {
        dependency = dep;
    }

    void useDependency() {
        if (dependency)
            dependency->doSomething();
        else
            std::cout << "Dependency not set" << std::endl;
    }
};

int main() {
    std::shared_ptr<Dependency> dep = std::make_shared<Dependency>();
    MyClass myClass;
    myClass.setDependency(dep);
    myClass.useDependency();

    return 0;
}
```

L'utilizzo della Dependency Injection rende il codice più modulare, facilita la sostituzione delle dipendenze con implementazioni alternative (utile per i test), riduce l'accoppiamento tra le classi e rende più facile il riuso del codice. Inoltre, aiuta a migliorare la leggibilità e la manutenibilità del codice, consentendo una migliore separazione delle responsabilità tra le diverse parti dell'applicazione.



Es.
Sono in classe robot.
Un oggetto della classe robot viene inizializzato con la Factory, una classe che consente di leggere i parametri con cui inizializzare l'oggetto robot da file di configurazione yaml, json o altro.
L'oggetto di robot ha come parametro di input ha solo la tipologia del robot.
L'oggetto della classe factory ha nel proprio costruttore una funzione che cerca il file di config. giusto (in base alla tipologia) ed estrae i dati. In output ha un oggetto.
Da ciò si può inizializzare i parametri (anche privati) dell'oggetto della classe robot.
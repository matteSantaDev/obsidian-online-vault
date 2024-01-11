#JavaScript #best-practices 

---

### Module Design Pattern
Qualsiasi numero di unità distinte ma interrelate con cui un programma può essere costruito, o struttura nella quale si può suddividere una attività complessa per analizzarla.

Questo design pattern segue il principio dell'incapsulazione, grazie al quale è possibile esporre solo gli elementi che devono essere accessibili all'esterno del modulo. Tutto il resto può essere nascosto. 

Dato che in JavaScript non c'è un modo esplicito per implementare variabili e funzioni private, si sfrutta lo scope delle variabili.

```javascript
const CarModule = () => {
  let milesDriven = 0;
  let speed = 0;

  const accelerate = (amount) => {
    speed += amount;
    milesDriven += speed;
  }

  const getMilesDriven = () => milesDriven;

  // Using the "return" keyword, you can control what gets
  // exposed and what gets hidden. In this case, we expose
  // only the accelerate() and getMilesDriven() function.
  return {
    accelerate,
    getMilesDriven
  }
};

const testCarModule = CarModule();
testCarModule.accelerate(5);
testCarModule.accelerate(4);
console.log(testCarModule.getMilesDriven());
```

In questo caso la variabile globale di fatto è `speed`, perchè è accessibile solo all'interno dello scope della classe.


### Metodo alternativo
Con la keyword `function` è possibile usare `this`, che è vincolata alla funzione stessa; mentre se è usata la freccia => non è possibile alcun utilizzo della parola `this`.
```javascript
function CarModule() {
  let milesDriven = 0;
  let speed = 0;

  // In this case, we instead use the "this" keyword,
  // which refers to CarModule
  this.accelerate = (amount) => {
    speed += amount;
    milesDriven += speed;
  }

  this.getMilesDriven = () => milesDriven;
}

const testCarModule = new CarModule();
testCarModule.accelerate(5);
testCarModule.accelerate(4);
console.log(testCarModule.getMilesDriven());
```


### Utilizzo delle classi
Le variabili `milesDriven` sono definite nel costruttore, anche se portebbero essere definite al suo esterno. Questo però comporte che tali variabili siano accessibili all'esterno della classe. 
```javascript
class CarModule {
  /*
    milesDriven = 0;
    speed = 0;
  */
  constructor() {
    this.milesDriven = 0;
    this.speed = 0;
  }
  accelerate(amount) {
    this.speed += amount;
    this.milesDriven += this.speed;
  }
  getMilesDriven() {
    return this.milesDriven;
  }
}

const testCarModule = new CarModule();
testCarModule.accelerate(5);
testCarModule.accelerate(4);
console.log(testCarModule.getMilesDriven());
```

##### Workaround con underscore
Un metodo usato per comunicare quali sono variabili private della classe (che non dovrebbero essere modificate) è l'utilizzo di nomi che iniziano con un underscore `_`.
Tuttavia è un metodo che non rende effetivamente le variabili private, ma comunica a chi lavora sul codice che tali variabili non dovrebbero essere utilizzate al di fuori dell'oggetto.
```javascript
// This is the new constructor for the class. Note that it could
// also be expressed as the following outside of constructor().
/*
  _milesDriven = 0;
  _speed = 0;
*/
constructor() {
  this._milesDriven = 0;
  this._speed = 0;
}
```

##### Inserire tutto dentro il costruttore
Inserendo tutte le definizioni delle variabili e dei metodi nel costruttore si rende impossibile accedervi dall'esterno.
```javascript
class CarModule {
  constructor() {
    let milesDriven = 0;
    let speed = 0;

    this.accelerate = (amount) => {
      speed += amount;
      milesDriven += speed;
    }

    this.getMilesDriven = () => milesDriven;
  }
}

const testCarModule = new CarModule();
testCarModule.accelerate(5);
testCarModule.accelerate(4);
console.log(testCarModule.getMilesDriven());
console.log(testCarModule.speed); // undefined -- We have true variable privacy now.
```
Le variabili non sono direttamente accessibili, ma il problema è che il codice non è bello. Per questo si ricorre al metodo successivo.

##### Utilizzo di `WeakMap()`
Se le mappe possono prendere qualsiasi valore come chiave, le WeakMap prendono solo oggetti e eliminano le chiavi quando la chiave dell'oggetto è eliminata.
Non è possibile ciclare sulle WeakMap, ciò significa che è necessario avere la chiave per poter accedere al valore.
```javascript
class CarModule {
  constructor() {
    this.data = new WeakMap();
    this.data.set(this, {
      milesDriven: 0,
      speed: 0
    });
    this.getMilesDriven = () => this.data.get(this).milesDriven;
  }

  accelerate(amount) {
    // In this version, we instead create a WeakMap and
    // use the "this" keyword as a key, which is not likely
    // to be used accidentally as a key to the WeakMap.
    const data = this.data.get(this);
    const speed = data.speed + amount;
    const milesDriven = data.milesDriven + data.speed;
    this.data.set({ speed, milesDriven });
  }

}

const testCarModule = new CarModule();
testCarModule.accelerate(5);
testCarModule.accelerate(4);
console.log(testCarModule.getMilesDriven());
console.log(testCarModule.data); //=> WeakMap { [items unknown] } -- This data cannot be accessed easily from the outside!
```
Le variabili però non sono propriamente private perchè comunque possono essere accedute anche dall'esterno dello scope dell'oggetto.
Soluzione non ottimale perchè introduce molta complessità e non è elegante.

##### Utilizzo di `Symbol`
I `Symbol` sono istanze che si comportano come un valore unico che non sarà mai uguale a qualcos'altro, eccetto la sua stessa istanza.
```javascript
class CarModule {
  constructor() {
    this.speedKey = Symbol("speedKey");
    this.milesDrivenKey = Symbol("milesDrivenKey");
    this[this.speedKey] = 0;
    this[this.milesDrivenKey] = 0;
  }

  accelerate(amount) {
    // It's virtually impossible for this data to be
    // accidentally accessed. By no means is it private,
    // but it's well out of the way of anyone who would
    // be implementing this module.
    this[this.speedKey] += amount;
    this[this.milesDrivenKey] += this[this.speedKey];
  }

  getMilesDriven() {
    return this[this.milesDrivenKey];
  }
}

const testCarModule = new CarModule();
testCarModule.accelerate(5);
testCarModule.accelerate(4);
console.log(testCarModule.getMilesDriven());
console.log(testCarModule.speed); // => undefined -- we would need to access the internal keys to access the variable.

Like the underscore solution, this method more or less relies on naming conventions to prevent confusion.
```

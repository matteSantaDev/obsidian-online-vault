#JavaScript #css #best-practices 

---
### Principali problemi
##### Troppe interazioni con l'host
Ogni interazione con l'oggetto host o il browser dell'utente aumenta l'imprevedibilità e contribuisce al ritardo delle prestazioni. Questo problema si manifesta spesso come un rendering lento degli oggetti [[Document Object Mode|DOM]]. Ovviamente non puoi evitare tali interazioni, ma puoi mantenerle al minimo.
##### Troppe dipendenze
Se le dipendenze JavaScript sono numerose e mal gestite, allora le prestazioni della tua applicazione ne risentiranno. Gli utenti dovranno attendere più a lungo per il rendering degli oggetti, il che è particolarmente fastidioso per gli utenti mobili con larghezza di banda limitata.
##### Scarso event handling
L'uso corretto dei gestori di eventi può migliorare le prestazioni riducendo la profondità dello stack di chiamate; tuttavia, se non ne tieni traccia, possono eseguirsi ripetutamente senza che tu ne sia a conoscenza.
##### Iterazioni inefficienti
Poiché le iterazioni richiedono molto tempo di elaborazione, rappresentano un ottimo punto di partenza per ottimizzare il tuo codice. Rimuovere cicli o chiamate superflue all'interno dei cicli velocizzerà le prestazioni dello script JavaScript.
##### Codice non organizzato
La natura flessibile di JavaScript è un vantaggio e un punto debole. Puoi fare molto con solo un piccolo sottoinsieme di costrutti lessicali, ma una mancanza di organizzazione nel tuo codice può comportare un'allocazione inadeguata delle risorse. Familiarizzarsi con gli standard ECMA può aiutarti a costruire JavaScript più conciso.


### Accorgimenti per migliorare le prestazioni di JavaScript
##### 1) Usare HTTP/2
Migliorala la velocità del sito. Permette che ci siano molteplici richieste e risposte contemporaneamente.

##### 2) Usare pointer references ✅
Salvare variabili globali per salvarsi valori del DOM usati frequentemente.
Puoi ridurre i viaggi di attraversamento del DOM memorizzando puntatori di riferimento per gli oggetti del browser durante l'istanziazione. 
Se non ti aspetti che il tuo DOM cambi, memorizzare un riferimento al DOM o agli oggetti jQuery necessari per creare la tua pagina può aiutare a velocizzare tutto il processo. In alternativa, se devi iterare all'interno di una funzione, ma non hai memorizzato un riferimento, puoi creare una variabile locale con un riferimento all'oggetto.
```js
// Storing references to DOM elements or jQuery objects during instantiation
const container = document.getElementById('container');
const changeTextButton = document.getElementById('changeTextButton');

// Using the stored references for manipulation
changeTextButton.addEventListener('click', function() {
  const paragraph = container.querySelector('p');
  paragraph.textContent = 'Text changed successfully!';
});

// Making a local variable with a reference to the object for iteration
const listItems = container.getElementsByTagName('li');
for (let i = 0; i < listItems.length; i++) {
  // Some operation on list items
  listItems[i].style.color = 'blue';
}

```

##### 3) Ridurre la dimensione dell'html ✅
La dimensione dell'html determina il tempo per interrogare e modificare gli oggetti del DOM. Se si riduce la dimensione dell'HTML si può raddoppiare la velocità del DOM.
Un esempio può essere eliminare `<div>` e `<span>` non necessari.

##### 4) Usare `.getElementById()` ✅
Invece di usare JQuery usare la funzione che permette di ottenere gli elementi del DOM dall'id.

```javascript
// With jQuery
var button = jQuery('body div.window > div.minimize-button:nth-child(3)')[0];

// With document.getElementById()
var button = document.getElementById('window-minimize-button');
```

##### 5) Raggruppare le modifiche apportate al DOM
Ogni volta si apportano modifiche al DOM, è meglio raggrupparle per evitare un rendering dello schermo ripetuto.
Se si sta apportando modifiche allo stile, cercare di eseguire tutte le modifiche in una volta sola anziché applicare modifiche a ciascuno stile singolarmente.

##### 6) Bufferizzare il DOM
Se ci sono `<div>` scorrevoli, si può utilizzare un buffer per rimuovere gli elementi dal DOM che al momento non sono visibili all'interno della finestre di visualizzazione. Questa tecnica aiuta a risparmiare sia sull'uso della memoria che sull'attraversamento del DOM.

##### 7) Comprimere file
Usando metodi di compressione come [Gzip](https://www.keycdn.com/support/enable-gzip-compression) or [Brotli](https://www.keycdn.com/support/brotli-compression) si riduce la dimensione dei file JavaScript che così possono essere scaricati più velocemente, migliorando la performance.

##### 8) Limitare le dipendenze delle librerie
Le dipendenze delle librerie contribuiscono notevolmente ai tempi di caricamento. Un modo per ridurre la dipendenza da librerie esterne è fare affidamento maggiormente sulla tecnologia in-browser.
Inoltre, se hai bisogno di selettori CSS complessi, prova ad utilizzare Sizzle.js al posto di jQuery. Se hai librerie che contengono solo una funzionalità, ha più senso aggiungere quella funzionalità separatamente.

Esempio:
Supponiamo di voler cambiare lo stile di un elemento HTML usando un selettore CSS complesso. In questo esempio, useremo Sizzle.js al posto di jQuery per ottenere lo stesso risultato.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Limiting Library Dependencies Example</title>
  <style>
    .highlight {
      color: red;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <div id="container">
    <p>This is a sample paragraph for demonstration purposes.</p>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/sizzle/2.3.3/sizzle.min.js"></script>
  <script src="script.js"></script>
</body>
</html>

```

Nel file `script.js`:

```js
// Utilizzo di Sizzle.js per selezionare l'elemento e applicare lo stile
const paragraph = Sizzle('div#container p')[0];
if (paragraph) {
  paragraph.classList.add('highlight');
}

```

Nell'esempio è stato utilizzato Sizzle.js per selezionare l'elemento `<p>` all'interno del contenitore e abbiamo applicato lo stile definito dalla classe CSS 'highlight'. Questo ci consente di evitare di caricare l'intera libreria jQuery solo per questa operazione specifica, limitando così le dipendenze dalle librerie esterne.

##### 9) MINIFICARE, Ridurre le dimensioni del codice
Raggruppando i componenti dell'applicazione in file \*.js e passandoli attraverso un programma di minificazione JavaScript renderai il tuo codice più snello.

**NB**. Minificare il codice è diverso che refattorizzarlo per renderlo più comprensibile. L'unica finalità è ottimizzarlo ridurro il tempo di download. Quindi si eliminano:
- spazi
- commenti
- punto e virgole
- block delimiters
[code minification tools](https://www.keycdn.com/support/how-to-minify-css-js-and-html)

##### 10) Aggiungere gestori di dipendenze post-caricamento
Aggiungere un gestore di dipendenze, come RequireJS o webpack, allo script di caricamento consente all'utente di visualizzare il layout della tua applicazione prima che diventi funzionale. Questo può avere un enorme impatto positivo sulle conversioni per i visitatori che accedono per la prima volta. 
Assicurati semplicemente che il tuo gestore delle dipendenze sia configurato per tenere traccia delle dipendenze già caricate, altrimenti potrebbe verificarsi il caricamento duplicato delle stesse librerie. Cerca sempre di caricare il minimo assoluto necessario affinché l'utente possa visualizzare l'applicazione.

In questo esempio, stiamo gestendo il caricamento di due moduli JavaScript separati, e li utilizziamo per aggiungere del testo a una pagina HTML:

Supponiamo di avere due file JavaScript `module1.js` e `module2.js`:

```js
// File: module1.js
define(function() {
  return {
    setText: function(elementId, text) {
      document.getElementById(elementId).innerText = text;
    }
  };
});
```
```js
// File: module2.js
define(function() {
  return {
    setText: function(elementId, text) {
      document.getElementById(elementId).innerText = text;
    }
  };
});

```

Nel nostro file HTML, carichiamo RequireJS e i nostri moduli, e poi li utilizziamo per modificare la pagina:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Post-load Dependency Example</title>
  <script data-main="main" src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.6/require.min.js"></script>
</head>
<body>
  <div id="content1">Placeholder text for module 1</div>
  <div id="content2">Placeholder text for module 2</div>

  <script>
    require(['module1', 'module2'], function(module1, module2) {
      module1.setText('content1', 'Text set by Module 1');
      module2.setText('content2', 'Text set by Module 2');
    });
  </script>
</body>
</html>
```
In questo esempio, stiamo utilizzando RequireJS per caricare i moduli `module1` e `module2` in modo asincrono. Una volta caricati, vengono utilizzati per impostare il testo in due elementi diversi sulla pagina. L'utilizzo di RequireJS in questo modo ci consente di gestire facilmente le dipendenze post-caricamento in modo efficiente.

Un altro esempio potrebbe riguardare il caricamento dei dati di una tabella in un file ejs.
1. Organizza la struttura del tuo progetto in base alle convenzioni di RequireJS. Assicurati di avere un file `app.js` principale che configuri RequireJS e carichi i tuoi moduli.
    
2. Crea un modulo JavaScript che carichi i dati della tabella. Ad esempio, potresti avere un file `tableDataModule.js` con un codice simile al seguente: ```
```js
// File: tableDataModule.js
define(function() {
  // Simulazione dei dati della tabella
  const tableData = [
    { name: 'John Doe', age: 30, email: 'john@example.com' },
    { name: 'Jane Smith', age: 25, email: 'jane@example.com' },
    // Altri dati della tabella...
  ];

  return tableData;
});
```
3. Nel tuo file EJS, puoi includere il seguente codice per utilizzare RequireJS e ottenere i dati della tabella:
``` html
<!DOCTYPE html>
<html>
<head>
  <title>Caricamento dati tabella con RequireJS in EJS</title>
  <script data-main="app" src="path_to_require_js/require.js"></script>
</head>
<body>
  <table>
    <thead>
      <tr>
        <th>Name</th>
        <th>Age</th>
        <th>Email</th>
      </tr>
    </thead>
    <tbody id="tableBody">
      <!-- Dati della tabella verranno inseriti qui -->
    </tbody>
  </table>

  <script>
    require(['tableDataModule'], function(tableData) {
      const tableBody = document.getElementById('tableBody');
      tableData.forEach(function(rowData) {
        const row = document.createElement('tr');
        Object.values(rowData).forEach(function(value) {
          const cell = document.createElement('td');
          cell.appendChild(document.createTextNode(value));
          row.appendChild(cell);
        });
        tableBody.appendChild(row);
      });
    });
  </script>
</body>
</html>

```

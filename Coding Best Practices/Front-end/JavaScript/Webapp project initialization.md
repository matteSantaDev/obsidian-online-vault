#JavaScript #best-practices 

---

### Setting iniziale
- `npm init -y`
- `npm install express`
- `npm i save --save-dev nodemon`
	in package.json modificare "scripts" con valore `"devStart": "nodemon server.js"`

- creo file `server.js`
- se scrivo `npm run devStart` viene eseguito sempre il file del server. Ogni volta che modifico qualcosa del server lo riavvia automaticamente.

### Inizializzazione server
- import di moduli express per server
```js
const express = require('express')
const app = express()

app.set('view engine', 'ejs')
```

- impostare porta
```js
app.listen(8080)
```
- definizione routes (dopo gli import ma prima della definizione della porta)
	È possibile usare differenti metodi per gestire richieste differenti:
	- get
	- post
	- put
	- delete
	- patch
```js
app.get('/', (req, res, next) => {
	...
	res.send('Hi')  //fornisce risposta generica, non molto usato, va bene per testo
	res.sendStatus(500)  //invia stato
	res.status(500).send("hi down")  //invia stato più messaggio
	res.status(500).json({message: "Error"})  //invia stato più json
	res.download('<path al file che si vuole inviare>')
})
```

- rendering di una pagina ejs/html
```js
app.get('/', (req, res, next) => {
	res.render('index', {text: "world"})
})
```
di default tutti i file grafici sono dentro la cartella `views`.
N.B. Necessario impostare la view engine (ejs, pug, ecc.) con `app.set('view engine', 'ejs')`. Prima di inserire la riga di codice scrivere nel terminale:
```
npm i ejs
```
Alla fine fare il restart del server con:
```
npm run devStart
```

- accedere a variabili da pagina ejs
```
	<%= %> per mettere valore sulla pagina
	<%- %> per accedere alla variabile passata, in <script>
```
Per evitare di rompere tutto se la variabile non è definita usare:
```
Hello <%= locals.text || 'Default' %>
```


### Come inizializzare il router
Se si hanno tante routes, magari legate allo stesso ambito, il file server diventerebbe molto lungo.
```js
app.get("/users", (req, res) =>{
	res.send("user list")
})

app.get("/users/new", (req, res) =>{
	res.send("users new form")
})
```
Invece di tenere questo codice nel file `server.js`, nella cartella `routes` creo il file `users.js`. Dentro a questo file devo:
```js
const express = require('express')
const router = express.Router()

router.get("/", (req, res) =>{
	res.send("user list")
})

router.get("/new", (req, res) =>{
	res.send("users new form")
})

module.export = router
```
In file `server.js`
```js
...
app.get(...)

const userRouter = require('./routes/users')
app.use('/users', userRouter)  //mount, associa la route da cui deve partire al router

app.listen(8080)

```
`router` ha tutti i metodi di app (get, post, delete...).
Altra differenza è che il router partirà sempre da `/users/`; quindi tolgo `/users/` dalla route dei get.

### Gestione routing advanced
Esempi funzionano sia nel file server sia nel router.
Se si vuole creare un nuovo user:
```js
router.post('/', (req,res) => {
	res.send(`User new form`)
})
```
Se voglio creare invece un nuovo user:
```js
router.get('/:id', (req,res) => {
	req.params.id  //per accedere ai parametri passati nella route
	res.send(`User with id ${req.params.id}`)
})
```
N.B. Importante mettere le route che si modificano dinamicamente dopo tutte le altre routes perché i nomi stessi delle routes potrebbero essere interpretati come variabili. La routes viene processata comparandola alle routes presenti nel file dall'alto al basso.
IMPORTANTE ==Le route che si modificano dinamenicamente != dai parametri passati nell'url==


Invece di avere più metodi sulla stessa route:
```js
router.put('/:id', (req,res) => {
	res.send(`User with id ${req.params.id}`)
})
router.delete('/:id', (req,res) => {
	res.send(`User with id ${req.params.id}`)
})
```
posso concatenarli così:
```js
router
	.route("/:id")
	.get((req, res) => {
		res.send(`User with id ${req.params.id}`)
	})
	.put((req,res) => {
		res.send(`User with id ${req.params.id}`)
	})
	.delete((req,res) => {
		res.send(`User with id ${req.params.id}`)
	})
}

router.params("id", (req, res, next, id) => {
	console.log(id)
	next()
})
```
`router.params` è una funzione che chiama una funzione ogni volta che c'è una route che ha un parametro id. La funzione `params` è middleware perché esegue tra la richiesta e la risposta.
Inoltre esegue per prima, motivo per cui bisogna specificare il successivo pezzo di middleware che viene eseguito.

Esempio di cosa può fare il middleware:
```js
router
	.route("/:id")
	.get((req, res) => {
		res.send(`User with id ${req.params.user.name}`)
	})
...
})

const user = [{name:"sally"},{name:"kyle"}]
router.params("id", (req, res, next, id) => {
	req.user = user[id]
	next()
})
```
Ricevo l'id dell'user, con il middleware inserisco nell'oggetto richiesta anche il nome pescato dal db. Le funzioni quindi che gestiscono le routes avranno il dato disponibile.

### Creare funzione di middleware
- Definisco una funzione che opera da middleware:

```js
app.set("view engine", "ejs")  //non centra

app.use(logger)

...
app.use("/users", userRouter)  //non centra

function logger(req, res, next) {
	console.log(req.originalUrl)
	next()
}
```
   Se voglio avere una funzione di middleware che esegue prima di ogni altra funzione deve essere definita in alto.

- Se si vuole far funzionare la funzione di middleware solo su una get:
```js
app.get("/", logger, logger (req,res) => {
	res.render("index", {text:"hello"})
})
```
   Si possono passare molteplici funzioni di middleware che eseguono in sequenza.

- Si può usare la funzione di middleware anche nel router:
```js
const router = express.Router()  //non centra

router.use(logger)

...

function logger(req, res, next) {
	console.log(req.originalUrl)
	next()
}
```

Il motivo principale per cui si usano le funzioni di middleware è gestire il routing di pagine statiche e tradurre le informazioni che giungono dal client.
- per fare il render di pagine statiche che non cambiano mai solitamente si evita di usare una route specifica e si gestisce il render con una funzione di middleware.
  Creo una cartella `pubblic` con dentro i file statici.
  Ora nel server:
```js
const app = express()  //non centra

app.use(express.static("public"))

app.set("view engine", "ejs")  //non centra
```
- per fare il parsing dei dati inviati al server da forms o json requests.
  Nell'esempio creo una nuova pagina `/user/new` in cui inserisco un form che fa partire una post con i dati del nome e cognome.
```js
router.post("/", (req, res) => {
	console.log(res.body.firstName)
	res.send("hi")
})
```
  Facendo solo così da errore perché di default express non permette di accedere al body. Quindi devo aggiungere:
```js
const app = express()  //non centra

app.use(express.static("public"))  //non centra
app.use(express.urlencoded({ extended:true }))

app.set("view engine", "ejs")  //non centra
```

  In uno scenario reale devo processare i dati arrivati dal form. Quando arrivano alla post e sono validi (ora si assume lo siano) uso `.redirect()` per indirizzare l'user su una route differente.

  Se voglio fare la stessa cosa ma mi arrivano dati formato json devo inserire:
```js
const app = express()  //non centra

app.use(express.static("public"))  //non centra
app.use(express.json())

app.set("view engine", "ejs")  //non centra
```

- se passo parametro `name` nell'url lo accedo così:
```js
router.get("/", (req, res) => {
	req.query.name
})
```

##### Esempio funzione di middleware
```js
function auth(req, res, next) {
	if(req.query.admin === 'true') {
		req.admin = true
		next()
	} else {
		res.send('No auth')
	}
}
```
Questa funzione è passata nella get che fa il render della pagina, se nell'url della pagina è passato il parametro `admin=true` allora viene eseguito il codice dentro la get, se no è inviato un messaggio e eventualmente effettuato il re-indirizzamento ad un'altra pagina.
Per passare valori dalla funzione di middleware alla get è necessario inserire variabili dentro al dizionario di `req` o `res`.
N.B. ==`next()` è diverso da `return`==


Se si vuole che il logger esegua le proprie routine dopo ogni funzione è necessario posizionare `app.use(logger)` all'inizio e chiamare `next()` come prima operazione dentro a logger.
```js
function(req, res, next) {
	next()
	... 
	//tutto il codice viene eseguito dopo le varie funzioni
}
```

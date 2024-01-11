#JavaScript #best-practices #code-example

https://www.youtube.com/watch?v=-RCnNyD0L-s -> 23:00

---

### Struttura progetto
- pagina register
- pagina di login (con form per email e password)
- pagina di benvenuto per l'utente (con bottone di logout)

---

### Setting iniziale
```
npm i express ejs
npm i --save-dev nodemon dotenv
```
`dotenv` consente di avere variabili globali che sono slavate nel server.
Viene creato un _gitignore_ per escludere dai commit sia `.env`, sia `node_modules`.

---
### Implementazione pagine

Create routes per *login* e *register* con 2 get in `server.js`.
Necessario usare
```js
app.set('view-engine','ejs')  //non centra
app.use(express.urlencoded({extended: false}))
```
per leggere i dati mandati dai form al server.
##### Pagina Register
inserito form associato a metodo post. La funzione post legata alla route di register è definita nel server.
```js
app.post('/register', (req, res) => {
	req.body.name
	req.body.email
	req.body.password
})
```

```html
<h1>Register<h1>
<form action="/register" method"POST>
	<div>
		<label for="name">Name</label>
		<input type="text" id="name" name="name"
		required>
	</div>
	<div>
		<label for="email">Email</label>
		<input type="email" id="email" name="email"
		required>
	</div>
	<div>
		<label for="password">Password</label>
		<input type="password" id="password" name="password"
		required>
	</div>
	<button type="submit">Register</button>
</form>
<a href="/login">login</a>
```

Per garantire la sicurezza della password
```
npm i bcrypt
```

```js
const bcrypt = require('bcrypt')
```

```js
app.post('/register', async (req, res) => {  //dato che è asincrona uso try catch
	try {
		//genero una password, 10 è il numreo di volte che viene generata la pw
		const hashedPassword = bcrypt.hash(req.body.password, 10)
		user.push({
			id: Date.now().toString()
			name: req.body.name,
			email: req.body.email,
			password: hashedPassword
		})
		res.redirect('/login')
	} catch {
		res.redirect('/register')
	}
	console.log(users)
})
```

`users` è un dizionario globale nel file server.

##### Pagina Login
Installo libreria per autenticazione
```
npm i passport passport-local express-session express-flash
```
`express-session` per la cache e `express-flash` per mostrare messaggi sull'interfaccia.

Metto tutto ciò che riguarda la configurazione in un file js separato dal file `server.js`, che si chiamerà `passport.config.js`. 
```js
const LocalStrategy = require('passport-local').Strategy
const bcrypt = require('bcrypt')

function initialize(passport) {
	const authenticaUser = (email, password, done) => {
		const user = getUserByEmail(email)
		if(user == null) {
			return done(null, false, {message: 'no user with that email'})
		}
		try {
			if(await bcrypt.compare(password, user.password)) {
				return done(null, user)
			} else {
				return done(null, false, {message: 'password incorrect'})
			}
		} catch(e) {
			return done(e)
		}
	}
	passport.use(new LocalStrategy({ usernameField: 'email'}), authenticaUser)
	passport.serializeUser((user, done) => { })
	passport.deserializeUser((user, done) => { })
}

module.exports = initialize
```

In `server.js`
```js
const flash = req

const passport = require('passport')
const initializePassport = require('./passport-config')
initializePassport(
	passport, 
	email => user.find(user => user.email === email)
})
```
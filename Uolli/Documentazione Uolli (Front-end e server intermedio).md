#uolli

---

### Struttura progetto

![[Uolli/Diagram.svg]]
Lo scopo di questa applicazione è fornire un interfaccia all'utente per il controllo del robot.

---
### Requisiti
- Versione di Node `16.17.1`
- Pacchetti all'interno di `package.json`
- File `.env`

### Requisiti di Sviluppo
- `eslint`
- `webpack`
- `webpack-cli`


Per installare tutti i pacchetti in modalità di sviluppo utilizziamo nel terminale:
```cmd
npm i <nome del pacchetto>
```
in produzione:
```cmd
npm i --omit=dev
```


Procedura di aggiornamento per Node 16:
1) `npm cache clean -f`
2) `npm install -g n`
3) `sudo n 16.13`
Potrebbe essere necessario riavviare il sistema affinché la versione corretta venga utilizzata.

File .env:
```env
NODE_ENV = development
PORT = 443
DATABASE_HOST = localhost
DATABASE_USER = root
DATABASE_PASSWORD = Robish_2021
DATABASE_NAME = alarms
NODE_TLS_REJECT_UNAUTHORIZED = 0
DATABASE_URL="mysql://root:Robish_2021@localhost/alarms?schema=public"
```
Si consiglia di modificare `NODE_ENV` in `production` quando l'applicazione viene rilasciata.

**ATTENZIONE**: il seguente parametro:
```env
NODE_TLS_REJECT_UNAUTHORIZED = 0
```
è utilizzato perché il server del robot non dispone di un certificato SSL valido; questo deve essere rimosso una volta che è stato aggiunto un certificato SSL valido.

---

### Generazione di file pubblici da src
Per l'uso in release o produzione, utilizzare il seguente comando:
```cmd
npm run releaseBuild
```

Per compilare ogni volta che vengono apportate modifiche (durante lo sviluppo), è possibile utilizzare il seguente comando:
```cmd
npm run build
```

Questi comandi permettono di generare i file pubblici a partire dalla directory src. Il comando `npm run releaseBuild` è destinato all'uso durante il rilascio o la produzione dell'applicazione, mentre `npm run build` è utile durante lo sviluppo per ricompilare i file ogni volta che vengono apportate modifiche alla sorgente.

---
### Avvio dell'app dalla cartella (opzionale)
Se la porta è 443, utilizzare il seguente comando per avviare l'applicazione:
```cmd
sudo node server.js
```

Altrimenti, utilizzare il seguente comando per avviare l'applicazione:
```cmd
node server.js
```
Da eseguire nella stessa cartella in cui si trova il file `server.js`.

___
### Gestione del servizio
Nel caso in cui l'applicazione venga avviata tramite un servizio, utilizzare i seguenti comandi:

```cmd
sudo systemctl enable webapp.service
sudo systemctl start webapp.service
sudo systemctl restart webapp.service
sudo systemctl stop webapp.service
sudo systemctl status webapp.service
```

Posizione del servizio:
```path
/etc/systemd/system/webapp.service
```
Questi comandi consentono di avviare, arrestare, riavviare e gestire il servizio relativo all'applicazione.

___
### Struttura dell'app

```
├── app
│   ├── configs
│   ├── controllers
│   │   ├── errors
│   │   ├── robot
│   ├── middlewares
│   ├── models
│   ├── routes
│   ├── services
│   ├── ssl
│   └── utils
├── locales
├── package.json
├── package-lock.json
├── prisma
├── public
├── server.js
├── src
│   ├── 3rdparty
│   │   ├── bootstrap
│   │   ├── croppie
│   │   ├── fontawesome
│   │   ├── jquery
│   │   ├── select2
│   │   ├── socket
│   │   ├── toastr
│   │   └── zxcvbn
│   ├── css
│   ├── images
│   ├── modules
│   └── pages
├── views
│   ├── pages
│   ├── partials
│   └── svg
└── webpack.config.js
```

- `app` [file del server intermedio]
	- `configs`: file di configurazione per le pagine
	- `controllers`: _request handlers_, files in cui sono definite le funzioni usate per gestire le risposte del server intermedio.
	- `middlewares`: files in cui sono definite le funzioni di middleware, funzioni express generiche come `authentication`.
	- `models`: query al database.
	- `routes`: files in cui il server intermedio gestisce le chiamate del client alle varie routes.
	- `services`: files in cui sono definite le funzioni usate per la comunicazione via HTTP con server C++.
	- `utils`: funzioni riutilizzabili.
- `locales`: dizionari contenenti la traduzione italiana e inglese dei termini (titoli, descrizioni, testo) usata negli elementi delle pagine.
- `prisma`: schema del database.
- `public`: cartella con file generati da `src`.
- `src` [file del Front-end]
	- `3rdparty`: librerie esterne.
	- `css`: file di configurazione degli elementi grafici nelle pagine.
	- `images`: immagini usate nelle pagine.
	- `modules` [logica per la gestione delle pagine]
		- `alarms`: files in cui sono contenuti i metodi con cui sono mostrati gli allarmi nell'interfaccia e inviati gli acknowledge.
		- `api`: files in cui sono scritte le richieste dall'interfaccia al server intermedio.
		- `buttons`: files contenenti le funzioni chiamate agli eventi generati dai bottoni.
		- `checkbox`: files contenenti le funzioni chiamate agli eventi generati dalle checkbox.
		- `config`: files contenenti funzioni che operano modifiche all'interfaccia in funzione dei dati (stati del robot) ricevuti dal server intermedio.
		- `debug`: metodi che mostrano lo stato del robot sull'interfaccia.
		- `forms`: files contenenti le funzioni chiamate agli eventi generati dai form.
		- `robot`: gestione invio dei comandi di controllo del robot verso il server intermedio.
		- `utils`: funzioni riutilizzabili.
	- `pages`: files JavaScript che contengono il codice richiamato nelle pagine ejs.
- `views` [template per la generazione delle pagine]
	- `pages`: files ejs delle interfacce grafiche di cui è composto il progetto. Se la pagina richiede l'utilizzo di funzioni, viene associata al file JavaScript necessario della cartella `src/pages`.
	- `partials`: files ejs che contengono un singolo elemento grafico utilizzato nelle pagine, come bottoni, modals, tabelle, ecc.
	- `svg`: icone SVG per i pulsanti usati nelle pagine.
- `server.js`: file del server.
- `webpack.config.js`: impostazioni del compilatore Webpack.

___
### Compilazione
La posizione predefinita di installazione è impostata per default su `/opt/myhound`, tramite il valore predefinito di `CMAKE_INSTALL_PREFIX`. È possibile impostarla diversamente, se desiderato.

Opzione 1: Per generare un pacchetto **deb**, eseguire questi comandi:
```bash
cmake -Bbuild -S.
cmake --build build
(cd build && cpack)
```

Opzione 2: Generatore in una sola riga.

==**ATTENZIONE:** Si noti che questo comando RIMUOVERÀ LA CARTELLA BUILD in anticipo==
```bash
rm -r build; cmake -Bbuild -S.; cmake --build build; (cd build && cpack)
```


___

### Trasferimento dei pacchetti deb sul computer del robot
```bash
rsync -avP ./build/*deb uolli@192.168.0.140:/tmp/
```

Per estrarre un pacchetto deb:
```bash
sudo dpkg -i uolli_hmi_2.0.0-pre.78.g3b14ff1_amd64.debv
```





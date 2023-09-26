Interfaccia *can configuration* che si trova all'interno del *configuration editor* quando si esegue una modifica ad una configurazione esistente.

Processo analizzato e modificato per risolvere issue [MYHND-1191].

---


La pagina che contiene tutto il codice dell'interfaccia e i partials è **configuration_model_editor.ejs**.

Con ejs è stata scritta in maniera che si adatti dinamicamente in modo da diventare un editor dei topic, delle configurazioni ecc. Ora considero solo il caso in cui la si utilizzi per la modifica delle configurazioni (*config\==\='can'*). Quindi viene incluso il file **can.ejs**, che contiene i partials **can_drive.ejs** e **can_line.ejs**.

### Come arrivano i dati della configurazione della can alla pagina can.ejs

I dati sono passati da **configuration_model_editor.ejs** a **can.ejs** nel dizionario *data*, passato come parametro dell'include. Il dizionario *data* arriva a **configuration_model_editor.ejs** come un parametro "globale" del dizionario *pageConfiguration*, questo passato alla pagina quando ne viene fatto il render in **model_route.js**. 

Poi dato che per ogni can line deve essere fatto l'include del file **can_line.ejs**, mentre per ogni can drive deve essere incluso il file **can_drive.ejs**, i dati sono anche in questo caso passati come parametri dell'include.

### Struttura dati passata a can_line.ejs
```
{
line_id: #numero intero (es. 1),
line:{
		alias: #(es. 'line1'),
		baus_rate: #(es. 12000),
		can_line: #nome della linea can (es. 1)
	},
can_line_id: #stringa con codice univoco
}
```

### Struttura dati passati a can_drive.ejs
```
{
drive_id: #numero intero (es. 1),
drive_data:{
			  id: '7sZ7JbH',
			  alias: 'drive2',
			  can_line: 'p36Ph5Q',
			  controller_id: '0x56'
			},
can_line_id: #codice univoco
}
```

### Metodo con cui sono eseguiti gli include di can_line.ejs
Per fare in modo che siano eseguiti gli include di Can Line con alias crescenti (1, 2) si esegue il seguente procedimento. (Si noti che il numero massimo di can line è 2).

Itero un array otdinato di dizionari relativi alle can line. Se il primo elemento ha il parametro *can_line* diverso da 1 (considerando che gli indici *can_line* iniziano sempre da 1), l'array è ordinato e quindi metto a true la variabile *ordered*. Se non è così *ordered* è false.

Quando poi devo fare l'include se sono *ordered*  eseguo effettivamente l'include ad ogni ciclo del for, se non sono *oredered* metto l'oggetto relativo alla can line in un array vuoto (di lunghezza 2) all'indice uguale al valore del parametro *can_line*. Così facendo ordino la serie di dizionari ed eseguo l'include ciclando sull'array ordinato.

### Metodo con cui sono eseguiti gli include di can_drive.ejs
Anche in questo caso per ordinare la sequenza di dizionari (max 4) in base a ```
```
ordered_drive[dict.alias[5]]
```
ovvero l'indice, in modo che sia crescente, è stato usato il metodo dell'array vuoto impiegato sopra.
Come sopra viene anche eseguito un controllo sulla lunghezza dell'array delle chiavi di tale dizionario, così da evitare in includere una pagina con dati nulli.

### for dopo il ciclo di include (sia per can_line.ejs, sia per can_drive.ejs)
Dopo ognuno dei cicli di include della can line e dei drive, viene inserito un ciclo for che include dati nulli nelle pagine. Questo server quando nella pagina l'utente modifica il numero delle can line o dei drive. Viene quindi creata una nuova linea con i form vuoti o in cui c'è il placeholder.
L'unico parametro passato infatti è l'indice.

### Inserimento valori nei dropdown
Sia per can line sia per i can drive i valori dei dropdown sono presi da *can_select* e *line_select*, definiti in **module_route.js**.

In entrambi i casi si confronta l'indice, rispettivamente della can line o del drive, con il parametro *id*. In caso positivo viene inserita un *option* che ha il parametro selected, in caso contrario viene inserita solo l'opzione.
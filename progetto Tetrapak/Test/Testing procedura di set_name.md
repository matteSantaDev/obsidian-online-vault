#tetrapak #testing-tetrapak 

---

### Funzionamento funzione _set_name_
La procedura di definizione del nome della scheda, attuabile dall'interfaccia del client con il comando **_set_name_**, consente di impostare il nome alla scheda.

Il nome attribuito alla board non è direttamente associato ad una configurazione impostata dall'utente. Tuttavia, poichè il nome assegnato alla scheda viene salvato su un file di testo nella SD, se l'utente utilizza board differenti utilizzando la stessa scheda sd (in cui è salvato anche il file di configurazione dei sensori) mantiene sia la medesima configurazione sia il medesimo nome.

La board durante la procedura di inizializzazione (quindi in seguito ad un riavvio o reset) legge il nome presente nel file di testo **_fileName.txt_** e lo inserisce nel payload trasmesso con l'advertising BLE. Se il nome non è stato ancora impostato semplicemente esso non sarà visibile dai client BLE che eseguono una scansione.
Eliminando il file **_fileName.txt_** si elimina il nome impostato in precedenza. Se invece viene utilizzato il comando **_set_name_** il nuovo nome sarà sovrascritto al vecchio.

L'utente può impostare un nome con una lunghezza massima pari a 12 byte.

### Passi per eseguire test
- inserire scheda sd sulla board e collegare l'alimentazione
- eseguire il client software
- scrivere il comando **_set_name_**
- scrivere il nome con cui si vuole (ri)nominare la board (la procedura verrà fatta rieseguire se il nome inserito ha una lunghezza maggiore di 12 byte)
- eseguire la scansione scrivendo il comando **_scan_** nel terminale del client software
- verificare il nome impostato, mostrato dopo il mac address e il nome della versione della board
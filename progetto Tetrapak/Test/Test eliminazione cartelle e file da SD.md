#tetrapak #testing-tetrapak

---

###### Obiettivi del test:
- [x] verificare che l'eliminazione dei singoli file funzioni
         Viene messo un file di testo (568 byte) sulla scheda sd ed è verificata l'effettiva eliminazione. Il file è stato eliminato correttamente.
         Stessa prova effettuata con successo con un file JSON (27425 byte).
		  
- [x] verificare che l'operazione di eliminazione funzioni se chiamata consecutivamente all'interno della stessa connessione
          Viene eseguita l'eliminazione successiva del file di testo e del file JSON presenti sulla SD. In seguito è stato controllato che tali file fossero effettivamente stati eliminati dalla SD.
      
- [x] verificare che una singola cartella vuota possa essere eliminata
          Eliminazione eseguita correttamente.

- [x] verificare che l'eliminazione di cartella contenente molteplici file sia effettuata correttamente
          Caricata sull'SD una cartella contenente 1 file. Il comando _remove_ chiamato sulla cartella senza prima eliminare i file al suo interno non funziona.
          Caricata sull'SD una cartella contenente 2 file. Il comando di _remove_ chiamato sulla cartella senza prima eliminare i file al suo interno non funziona.
          Test fatto lasciando un file nella root della SD oltre alla cartella considerata.
          Con la board in debug nessun errore, l'esecuzione entra correttamente in HAL_Delay().
          Eseguendo il remove quando c'è solo cartella piena (senza altri file) nominata "cartella piena" ritorna "Could not find the file". Prova effettuata cambiando il nome della cartella in "cartella_piena". 
          ==Funziona se non si mette spazio nel nome.== La presenza di file singoli tra le cartelle è irrilevante.
          

- [x] verificare la corretta eliminazione di una cartella contenente una cartella piena
          Eliminazione eseguita con successo.

- [x] verificare eliminazione di una cartella contenente molteplici cartelle (piene) e file
![[file_path_delete_test.png]]
          Eliminazione eseguita con successo.

- [x] test caso d'uso in cui si eseguono 3 acquisizioni di 10 secondi su SD. Si eliminano le cartelle al termine dell'acquisizione (acquisisco due volte il sensore 0 e una volta il sensore 2)
          Eliminazione eseguita con successo.


###### Criticità:
- I nomi dei file e cartelle che contengono uno spazio non consento il riconoscimento dei file e la corretta esecuzione della procedura di eliminazione.
- Il cursore utilizzato per la selezione dei file e cartelle non funziona se il sofware viene utilizzato mediante il terminale di visualstudio code.


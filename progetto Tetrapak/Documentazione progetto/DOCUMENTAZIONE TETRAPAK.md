### Flusso dati
Il processo di trasferimento dati (dalla board al client software) inizia sempre con un input proveniente dal client software. La comunicazione avviene tramite il protocollo GATT permesso dal Bluetooth Low Energy. Nello specifico una volta ricevuta la stringa che determina l'inizio dell'acquisizione e trasmissione/salvataggio dei dati (BLE, usb, sd) di uno o più sensori, sono inizializzate a _true_ le flag che determinano lo stato di attivazione del sensore stesso. La funzione in cui questo avviene è **ActivateSensor()** (riga 1479 di ble_config_service.c).
![[Pasted image 20230213121613.png]]
	**readSensors()** (247 di **sensors.c**) è chiamata nel thread **SENSORS_Thread** (480 in **sensors.c**), nello stato EXECUTE (534 in **sensors.c**). In base al sensore attivato, **readSensors()** chiama la funzione [sensor Name]\_**Read_Data_test**() nel file del sensore, definita nel file dedicato al sensore nella cartella Src di HSD_core, al fine di accedere al buffer di dati. 
	L'array di dati è inserito nella coda dedicata allo specifico sensore, la quale è svuotata in **writeExternalComm()** (546 in **external_comm.c**). Il buffer ottenuto dalla coda è poi inviato ai moduli di trasferimento (sd o ble) con la funzione **sendDataToDestinations()**.
	**writeExternalComm()** è chiamata nella nello stato EXECUTE della state machine implementata all'interno di **EXT_Thread()**, (825 di **external_comm.c**). 
	In **sendDataToDestinations()** (632 di **external_comm.c**) può essere eseguita la scrittura su sd dei dati ricevuti, o alternativamente può essere eseguito l'invio del buffer al modulo ble; ciò avviene sfruttando la coda **rawDataQueue_2ND_QUEUE**. All'interno di **ble_notify_Thread** (364 di **ble_comm_manager.c**) i dati sono estratti dalla coda **rawDataQueue_2ND_QUEUE** e scritti nella caratteristica del GATT database.

### Utilizzo delle caratteristiche
###### Caratteristica 1
  Usata per la scrittura del flag ==_get_fs_==, con il quale il client richiede la trasmissione via BLE di un file presente sulla sd.
###### Caratteristica 2
- usata per trasferire i dati dei sensori con meccanismo di notify. Buffer dati devono essere suddivisi in batch da 210 byte, massima lunghezza della caratteristica se si usa la modalità di notify.
- usato per il trasferimento dei file. In questo caso viene segnalata la fine del file con la stringa "interruption_connection".
- usata per la ricezione di file inviati alla scheda dal client. Quest'ultimo esegue una successione di scritture sulla caratteristica.
###### Caratteristica 3
  Caratteristica usata per la scrittura dei flag che consentono di impostare il funzionamento della board dal client.
  - ==_ls_==: determina l'invio dei dati dei sensori tramite BLE.
  - ==_sd_==: determina la scrittura dei dati acquisiti dai sensori sulla scheda sd.
  - ==hs==: determina l'inizio della procedura di acquisizione dati e scrittura dei file sul client sfruttando il collegamento usb.
  - ==_stop_sd_==: comando che termina l'invio dei dati (notify via BLE) o alternativamente la scrittura sulla scheda sd.
###### Caratteristica 4
  Usata per rendere pubblico ed accessibile ai client lo stato della scrittura dei dati sulla scheda sd (poichè è una procedura che consente al client di non rimanere connesso durante la durata del suo svolgimento).

### Utilizzo delle code
  Per conesentire il trasferimento dei dati dal file in cui sono letti i registri allocati ad ogni sensore al modulo di _external communication_, ad ogni sensore è stata dedicata una coda. Ogni coda ha l'id "rawDataQueue_\[nome sensore]\_id_". La lunghezza delle code varia in base alla lunghezza del buffer di dati raccolto dai sensori.

  Per trasferire i dati dal modulo di _external communication_ al thread del BLE la coda con l'id "rawDataQueue_2ND_QUEUE_id". Questa ha la lunghezza del buffer di dati più lungo tra tutti quelli dei sensori. Essendo solo una coda non è possibile effettuare l'invio con il BLE di dati di più sensori contemporaneamente.
  
### Funzionamento state machines
###### External Communication (in **_external_comm.c_**)
 IDLE:
     Stato iniziale della state machine di external communication. In questo stato vengono ricevuti messaggi dalla coda e viene eseguita la transizione allo stato EXECUTE.
 EXECUTE:
     Viene effettuata l'inizializzazione dei sensori o lospegnimento (quest'ultimo non implementato) a seconda dello stato dei sensori stessi. Dopo di che i sensori vengono letti utilizzando la funzione **_readSensors()_**. Dopo la lettura la state machine torna a IDLE.
 ABORTING:
     Non ancora implementato.
 STOPPED
     Non ancora implementato.
 RESETTING
     Non ancora implementato completamente, infatti non si entra mei nello stato. Però all'interno dello stato è chiamata una funzione che carica la configurazione dei sensori salvata in memoria. 
 
###### Main Controller (in **_main_controller.c_**)
 IDLE:  
     Stato di partenza della state machine del main controller. In questo stato può essere fatto         partire lo 'start' o 'stop' dell'acquisizione dati (via BLE, USB o sulla scheda SD).
     Se sono ricevuti messaggi di errore la macchina a stati viene fatta transizionare allo stato EXECUTE.
 EXECUTE: 
     Si entra in questo stato solo per eseguire l'handling degli errori, di nuove configurazione o di firmware update (funzioni di handling che però non sono ancora state implementate).
 ABORTING: (non ci entra mai per ora)
     Stato in cui viene terminato il main controller (non ancora implementato).
 STOPPED: (non ci entra mai per ora)
     Stato in cui viene fermata la lettura del segnale proveniente dai sensori, oltre a che essere fermato il trasferimento dei dati al modulo di external communication.
 RESETTING: (non ci entra mai per ora)
     Stato in cui viene caricata la configurazione dei sensori da memoria e (parte non implementata) sono svuotate le code e gli array. 

 I comandi provenienti dal main controller sono inseriti in una coda e letti nel modulo external communication. I comandi provenienti dal main controller cambiano gli stati della state machine di external communication.


#tetrapak #BLE

---

l thread valutato _ble_notify_Thread_ è nel file _ble_comm_manager.c_ (riga 422).
Nel ciclo for di questo thread è stato inserito un delay per consentire l'invio di nuovi dati tramite BLE.

Il client (software pc) dopo il collegamento rimane in ascolto per 20 second (durata del notify).


durata del delay all'interno del thread | durata del notify | numero di notify ricevuti | dimensione messaggio ricevuto (byte) | dimensione messaggio inviato (byte)| dimensione caratteristica
------------ | ------------ | ------------ | ------------ | ------------ | ------------
50 millisec | 20 sec | 19 | 217 byte / 1736 bit | 100 byte | 500 byte
10 millisec | 20 sec | 19 | 217 byte | 100 byte | 500 byte 
10 millisec | 20 sec | 19 | 217 byte | 17 byte | 500 byte 
10 millisec | 20 sec | 19 | 17 byte | 17 byte | 17 byte
400 millisec | 20 sec | 14 | 100 byte | 100 byte | 500 byte
200 millisec | 20 sec| 16 | 100 byte | 100 byte | 500 byte



FATTORI CHE LIMITANO LA VELOCITÀ DI TRASFERIMENTO DATI DEL BLE
I datarate possibili sono:
- 1 Mbps = 125 000 Bps
- 2 Mbps = 250 000 Bps
- 125 kbps = 15 625 Bps
- 500 kbps = 62 500 Bps

ma sono velocità non ottenibili per le ragioni seguenti:
- c'è un limite sul numero di pacchetti trasmissibili nell'intervallo di connessione
- ritardo tra pacchetti successivi successivi, Inter Frame Space ([[IFS]])
- pacchetti vuoti che devono essere trasmessi da un dispositivo anche se non sono disponibili i dati da trasmettere
- packet overhead, non tutti i byte sono usati per il payload








---
ALLINEAMENTO TETRAPAK 27/10/2022





Attività svolte:
- ricercato la **massimizzazione del throughput**, ovvero massimizzazione dei dati inviati tramite protocollo GATT

		PARAMETRI MODIFICATI
			
			- dimensione caratteristica (massima dimensione settabile secondo la documentazione è 511 byte, massima dimensione effettivamente sfruttabile e visibile dai client BLE è 217 byte)

			- dimensione messaggio inviato (la dimensione del payload non ha nessun impatto sulla dimensione del payload ricevuto dal client BLE, ne tanto meno influisce sulla frequenza di invio/throughput. Le variazioni del payload invece causano una riduzione del throughput; modificando infatti il payload si rallenta il throughput)

			- delay all'interno del thread, quindi frequenza di chiamata della funzione che aggiorna il valore della caratteristica. Modificare il valore (testati valori da 50 millisec a 500 millisec) rende il throughput più regolare ma più lento (miglior valore ottenuto è 217 byte al secondo ottenuto con un delay nel thread di 50 millisec).

			- numero massimo di attributi che possono essere aggiunti ad un servizio (messo a 30), non modifica throughput. Gli attributi devono essere 1 per ogni servizio e 2 per ogni caratteristica; quindi 30 dovrebbe essere un numero sufficiente (3 servizi + 5 caratteristiche = 13 attributi)

			-  intervallo di connessione
			
![[Pasted image 20221026160405.png]]

			funzione in _ble_config_service.c_ che è una callback invocata alla creazione di una nuova connessione.

			Il _connection event_ è il tempo in cui sono ricevuti o inviati pacchetti dati. Il SoftDevice può allungare dinamicamente la durata della connessione al fine di inserire il massimo numero di pacchetti nel _connection event_ prima che finisca il tempo. L'estensione dell'intervallo non può avere una lunghezza maggiore all'intervallo di connessione stesso. 




- analisi dei possibili limiti imposti dalla libreria python con cui è stato creato il client

		- è possibile che durante il notify il client in attesa si disconnetta e si riconnetta costantemente al server
		- verificare il time_sleep()  




- esplorazione delle possibilità di utilizzo del bluetooth normale







---
interfaccia pc della scheda:

DA PC A BOARD
- scaricare firmware
- dati
- new config

DA BOARD A PC
- status

in board c'è logica di comunicazione da organizzare come macchina a stati con 
			- stati idle
			- stati operativi (in base a lista operation della scheda, lista delle op che deve fare la scheda tipo comandi operativi come START, STOP, FORNIRE CONFIG, FORNIRE AGGIORNAMENTO FIRMWARE)

interfaccia anche a riga di comando che attende comandi utente da terminale, e se è uno dei comandi accettati lo fa partire.





--- 



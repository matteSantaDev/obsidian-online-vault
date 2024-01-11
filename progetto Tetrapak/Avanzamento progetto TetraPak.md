#python #tetrapak #C 

Related to: [[OTA characteristic system]]

---
## Modalità di stop dello streaming dati tra scheda e client (tramite protocollo BLE)

La scheda esegue il trasferimento dati (3,5 kB/sec), sfruttando la funzione di _notify_, aggiornando il valore della caratteristica da cui si inviano le notifiche ogni 50 millisecondi.

Le modalità con cui è possibile terminare lo stream dati sono:
- **scrittura di un comando di stop sul terminale python**. Lo script rimane in attesa fintanto che non è inserito il comando di stop. Per ora è scelta **questa** modalità.
- **lettura di una caratteristica in cui viene scritto un flag**: la scheda, nel thread in cui è aggiornato il valore della caratteristica notify, incrementa un contatore. Quando questo raggiunge il numero massimo stabilito su una seconda caratteristica è scritto il flag di stop notify. Lo script parallelamente rimane in attesa dei pacchetti dati notify e legge la seconda caratteristica relativa al flag. Quando il valore prestabilito è letto la connessione è interrotta.

**NB** la variabile **int** in C può arrivare al massimo a 255, quindi quando veniva incrementata fino a 300 il codice non funzionava.



## Controllo dell'inizio del rilevamento dati

iniziare la procedura di raccolta dati (dati rilevati dai sensori attivi, secondo il file di configurazione) tramite protocollo BLE.

- [x] inviati tramite notify usando il protocollo BLE
	- La terza caratteristica ha una lunghezza di 2 byte riservati alla parola chiave _go_ (tradotto dai char _103_ e _111_) che se inserita consente di iniziare o terminare l'acquisizione in corso.
	- la quarta caratteristica permette di conoscere lo stato dell'acquisizione:
		1 -> in corso
		0 -> non in corso
		
	guardare riga 1300 sdcard_manager.c -> _SDM_WriteBuffer_
	anche se i dati non hanno senso, si prova a salvarli nella coda e poi scriverli su caratteristiche differenti. Verificare la fattibilità dell'utilizzo del notify su diverse caratteristche contemporaneamente (possibile).
	
- [ ] dati salvati nel formato .dat nella root della scheda
	non possibile perchè non è stato possibile scrivere file sulla scheda SD

---
test di trasferimento dati:

##### TEST 1, UTILIZZO CODE
è usata una coda per trasferire i dati del sensore **hts221**, sensore di umidità e temperatura, dalla callback _data_ready_ al thread in cui i valori sono scritti nelle caratteristiche (che fanno notify al client). 

###### TEST 2, UTILIZZO ARRAY DICHIARATO COME VARIABILE GLOBALE
throughput molto più veloce, 449 Byte/sec con rilevamento dati spento, con due caratteristiche che fanno notify grandi 4 byte.
Con rilevamento dati acceso il throughput è pressochè identico a quello ottenuto con il test in cui sono usate le code.

###### TEST 3, UTILIZZO ARRAY DICHIARATO IN MEMORY POOL

==le tre soluzioni hanno lo stesso throughput perchè il fattore limitante è dovuto alla bassa frequenza con cui i dati sono scritti nella coda.==



## Update del firmware 

- con che modalità dobbiamo aggiornare il firmware (via USB, OTA)
- (Marcos) come si fa un update del firmware?
- come viene modificata la configurazione della scheda (necessario usare la scrittura perchè il server è la scheda e non può ricevere dei pacchetti di notify in successione -> a meno che non diventi client)


## FIFO da cui sono letti i dati dl giroscopio e dell'accellerometro

==01/12/2022==
- [ ] determinare la velocità di acquisizione del sensore in relazione alla velocità con cui viene popolata la FIFO

- [x] come e se vengono decimati i dati prima di essere messi nella FIFO
- [x] come attivare l'algoritmo di decimazione della FIFO
la FIFO relativa al giroscopio è implementata a livello hardware, è possibile implementare un algoritmo di compressione scrivendo valori in alcuni registri, letti al momento dell'inizializzazione del sensore. Determinare se tale algoritmo opera o no. Teoricamente però tutti i dati in uscita dal modulo hardware sono poi quelli visibili nel buffer usato.

- [ ] come configurare la FIFO nelle 6 diverse modalità possibili

- [x]  (6.5.7) come impostare valori nei registri dedicati alle impostazioni della FIFO (ad esempio per attivare la FIFO compression feature) come si fa a scrivere un registro

## Lettura di file dalla scheda SD

- [x] lettura di un file nella root della scheda sd
- [ ] scrittura in una caratteristica dei dati letti da un file usando coda
	non ancora possibile perchè:
	- se definisco una coda di numeri singoli troppo grande non ho abbastanza [[Heap Memory]]
	- se inserisco nella coda solo il [[pointer|puntatore]] del buffer/array, poichè quest'ultimo è una variabile locale, è un riferimento non valido.
- [x] trasferimento dei dati letti dal file e scrittura della caratteristica senza usare coda
	provando a non usare coda, il trasferimento dell'array avviene utilizzando solo un array globale (lunghezza massima di un array è 512 byte).
	PROBLEMA -> quando sono letti file di dimensione maggiore il buffer non è popolato
- [ ] leggere file di dimensione maggiore di 512 byte








## Lettura sensori
è possibile accedere ai dati dei seguenti sensori:
- [x] ISM330DHCX 
	accelerometro e giroscopio
- [ ]  hts221
	sensore ultracomaptto di temperatura e umidità
- [x]  iis2dh
	accelerometro a tre assi lineare lineare, a basso consumo energetico


JIRA

prima di sviluppo, fare test pattern.
per meeting issue chiuse e commentate.
giovedi mattina 11:00 15/12/2022.


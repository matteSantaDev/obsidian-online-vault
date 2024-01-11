 --- 
## PROCEDURA DI ACCENSIONE BOARD

- assicurarsi che la scheda sd sia inserita.
- collegare la scheda all'alimentazione.
- effettuare il reset della scheda (utilizzando il tasto di reset presente sulla ST board. Versioni successive del firmware consentiranno di eseguire tale procedura dal client software).

## CLIENT

Lanciare l'esecuzione del software (python3) **cli_communication_board.py**. 

###### Scansione e ricerca ST board
La prima volta che viene eseguito il software, è eseguita la scansione Bluetooth dei possibili nodi disponibili. Terminata la scansione il software presenta la lista delle ST board disponibili pre-configurate con il firmware IT-I. L'user può selezionare la ST board a cui connettersi.

![[Pasted image 20230217164408.png]]
All'esecuzione successiva il client software è automaticamente settato, senza effettuare la scansione per individuare la ST board. Nel caso in cui l'utente desiderasse connettersi ad una ST board differente può utilizzare il comando **_scan_**, il quale reitera la procedura di scansione e consente la scelta della ST board.

![[Pasted image 20230217161647.png]]

###### Acquisizione dati
L'utente può iniziare una sessione di acquisizione dati con il comando **_start_**. L'acquisizione può avvenire con 2 modalità differenti:
1) trasmissione dei dati al client software via Bluetooth Low Energy.
    In questa modalità è possibile trasmettere i dati di un solo sensore per volta. Il comando che deve essere usato è il seguente:  
                            **_start ble** \[sensor id] \[sensor stream decimation rate]_
    Con il comando sopra scritto viene creata una cartella nominata con la data e l'orario di avvio dell'acquisizione, all'interno della quale è inserito il file relativo al sensore scelto.
    ![[Pasted image 20230220110159.png]]
2) trasmissione dati al client software via usb.
	In questa modalità è possibile trasmettere i dati dei sensori al client software. Possono essere selezionati da uno a 5 sensori contemporaneamente con il seguente comando:
	                        **_start usb** \[ \[sensor id], sensor id], …. sensor id]] \[sensor stream decimation rate]\[(optional) data acquisition duration]_
	Se devono essere acquisiti i dati di più sensori, gli indici devono essere separati da virgole. I dati sono salvati in file binari nella cartella _acquisition_. È possibile inserire come terzo parametro anche la durata di acquisizione in secondi; in quest'ultimo caso non deve essere utilizzato il comando **_stop_** per terminare l'acquisizione.
3) scrittura dei dati sulla scheda sd. 
    In questo caso possono essere acquisiti i dati di tutti i sensori contemporaneamente. Il comando che deve essere usato è:
        **_start sd** \[ \[sensor id], sensor id], …. sensor id]] \[sensor stream decimation rates]_
    Il comando sopra scritto crea una cartella nominata _TPSH__\[acquisition number]_ in cui sono inseriti i file relativi ai sensori selezionati e un file JSON contenente la configurazione dei sensori utilizzata per l'acquisizione.
                            ![[Pasted image 20230220110312.png]]
_sensor_id_ è l'indice numerico che identifica un sensore. Tali indici sono gli stessi usati nel file di configurazione in formato JSON con il quale possono essere impostati i parametri relativi ad ogni singolo sensore.
L'indice differisce dal codice identificativo del sensore, poichè quest'ultimo è il nome alfanumerico attribuito al sensore dal produttore.
In entrambe le acquisizioni i file sono nominati con il codice identificativo del sensore. Ogni sensore è associato ad un indice numerico:
	0 = IIS3DWB
	2 = IIS2DH
	4 = IMP34DT05
	5 = ISM330DHCX
	6 = LPS22HH
	7 = IMP23ABSU

###### Interruzione acquisizione dati
L'utente può terminare la procedura di acquisizione dati (sia BLE sia sulla scheda sd) utilizzando il comando **_stop_**.

###### Download dei file salvati sulla scheda sd
È possibile scaricare i file salvati sulla scheda sd (ad esempio i file binari in cui sono salvati i dati rilevati da un singolo sensore). L'utente può accedere alla lista dei file presenti sulla sd utilizzando il comando **_download_**, con il quale il client software mostra la lista dei nomi dei file presenti sulla board. L'utente quindi può selezionare il file di cui si desidera effettuare il download dalla scheda al pc utilizzando l'arrow key.

###### Configurazione parametri sensori
La configurazione della scheda viene memorizzata in un file JSON che può essere caricato con il comando **_upload_file_**. Sulla scheda è consentito un solo file di configurazione, tuttavia l'utente può avere diversi file di configurazione con nomi diversi; il software cambierà i nomi di conseguenza durante il caricamento. L'utente può scaricare in qualsiasi momento la configurazione del sensore della scheda utilizzando il comando **_download_**. Si tenga presente però che il nome del file è un nome standard che è diverso dal nome del file stesso scaricato in precedenza.

###### Chiusura sessione
Per effettuare la chiusura del client software può essere utilizzato il comando **_close_**.

###### Firmware update
L'aggiornamento del firmware della board può essere eseguito in 2 modalità:
1) Utilizzando STM32CubeIDE
    - collegare board e st-link al pc.
    ![[Pasted image 20230329170238.png]]
    - apri tendina _file_.
    ![[Pasted image 20230329170846.png]]
    - open projects from file systems.
    - selezionare la cartella con il progetto.
    ![[Pasted image 20230329171005.png]]
    - selezionare finish.
    ![[Pasted image 20230329171342.png]]
    - schiacciare tasto di run dopo aver selezionato il progetto.
2) utilizzando STM32CubeProgrammer
    - collegare board e st-link al pc.
    ![[Pasted image 20230329173516.png]]
        - selezionare l'opzione di collegamento ST-LINK.
    ![[Pasted image 20230329173304.png]]
    ![[Pasted image 20230329173443.png]]
    - schiacciare tasto di _reset_ sulla board e contemporaneamente schiacciare il tasto connect nell'interfaccia di STM32CubeProgrammer.
    ![[Pasted image 20230329173616.png]]
    - (prima interfaccia) selezionare il pannello _Open file_ e scegliere il file binario del firmware che si vuole caricare sulla board.
    ![[Pasted image 20230329173721.png]]
    - schiacciare download.
    - schiacciare disconnect.



###### Documentazione per interpretazione dati trasmessi dalla board
I dati ottenuti dai sensori (sia che essi siano trasferiti via BLE o usb dalla board al client software, sia che venga effettuato il salvataggio localmente sulla sd della board) si presentano nel formato file _.dat_.
Per poter estrarre i dati correttamente è necessario conoscere la struttura generale dei dati utilizzata nei file _.dat_, che è la seguente:

![[Pasted image 20230420093234.png]]
Si noti che questa struttura è stata adottata per i dati di tutti i sensori attivi sulla board.

Di seguito vengono proposti gli schemi relativi alla struttura utilizzata per i dati di tutti i sensori attivi sulla board; sono inoltre indicati i range di valore dei dati grezzi e le eventuali operazioni da effettuare su di essi. I parametri necessari alla conversione dei dati grezzi sono definiti nel file di configurazione _DeviceConfig.json_.

**Sensore 0 - IIS3DWB** e **Sensore 2 - IIS2DH**
![[Pasted image 20230420095837.png]]
Ogni singolo valore grezzo di accelerazione (ad esempio _acc_0_), poichè è un valore a 16 bit, è nel range \[-32768 : 32767]. Tale dato dev'essere convertito in un valore appartenente al range \[-FS : FS]; il parametro FS è definito nel file di configurazione DeviceConfig.json.

Per ottenere il valore di accelerazione dal dato grezzo (_value_) può essere utilizzata la seguente procedura.

		leftSpan = 32767 - (-32768)
		rightSpan = FS - (-FS) 

Normalizzazione del dato con valore in range \[-32768 : 32767] 

		valueScaled = (value - (-32768)) / (leftSpan)

Conversione del dato con valore in range \[0 :1] in un valore appartenente all'intervallo \[-FS : FS] . 

		valore di accelerazione = -FS + (valueScaled * rightSpan)

**Sensore 4 - IMP34DT05**
![[Pasted image 20230420145211.png]]

**Sensore 5 - ISM330DHC**
![[Pasted image 20230420145310.png]]
Anche in questo caso deve essere eseguita la conversione dei valori dal range \[-32768 : 32767] al range \[-FS : FS].

**Sensore 6 - LPS22HH**
![[Pasted image 20230420145550.png]]
I valori del dato grezzo di pressione deve essere diviso per 4096.
I valori del dato grezzo di temperatura deve essere diviso per 100.

**Sensore 7 - IMP23ABSU**
![[Pasted image 20230420150612.png]]


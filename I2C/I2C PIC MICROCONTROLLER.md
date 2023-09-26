### I2C
Sta per **Inter-Integrated Circuit**. Si può chiamare anche IIC, o ITWOC. 
Creata da philips nel 1982.
Fornisce un semplice e robusto canale di comunicazione seriale tra dispositivo periferico e microcontrollore.
![[Pasted image 20230915093155.png]]
Usando I2C si riduce il costo perchè ci sono un minor numero di porte logiche (logic gates).

### Caratteristiche del protocollo I2C
![[Pasted image 20230915093521.png]]

L'host inizia la cominicazione con il client. Per facilitare lo scambio di info viene inviato un segnale clock. Tale segnale è 3,4 MHz in modalità bidirezionale e 5 MHz in modalità unidirezionale.
L'host fornisce sembre il segnale di clock o il data transfer rate. Bus bidirezionale perchè l'host può scrivere al client e leggere dal client.
![[Pasted image 20230915093839.png]]
Il bus è seriale, ciò significa che i dati sono associati al clock o shifted un bit alla volta, bit per bit.
Ci sono 2 bus line:
- SCL -> Serial CLock
- SDA -> Serial DAta
Entrambi i bus sono open drain o open collector, perchè si lascera massima tensione nella linea per creare un 1 logico, mentre si portera la tensione a low per avere 0 logico.
![[Pasted image 20230915094416.png]]

### I2C Data Transfer
![[Pasted image 20230915094642.png]]
La comunicazione sull'I2C inizia con una *start condition*, Viene poi messo il *client address*, si inserisce la condizione di read o write (*R/W*), che è un bit. Ci sono poi bit di acknowledge (*ACK*), i *data byte*. La comunicazione è poi conclusa con una condizione di *stop*.

###### Start condition
La comunicazione deve essere iniziata con lo stato del SDA e del SCL in stato di idle (ovvero con valore di logic 1). Essendo open drain i bus in condizione di idle, quindi non cotrollati, hanno una condizione di default di alto per le resistenze in parallelo.
![[Pasted image 20230915095334.png]]

Condizione eccezionale perchè la tensione di SDA deve sempre variare solo quando SCL è basso.

###### Client Address
Segue la start condition e può essere di 7 o 10 bit, cosa specificata nel datasheet del microcontroller. Importante definire l'address del device client perché senza di esso non è possibile comunicare.
Ogni dispositivo collegato alla stessa I2C deve avere un indirizzo client differente.
![[Pasted image 20230915095843.png]]
Se gli indirizzi non sono univoci i dispositivi risponderebbero contemporaneamente causando una collisione della linea bus, la quale può causare data corruption e comportamenti incorretti. Ad esempio se un device scrive uno 0 e l'altro un 1, l'host vedrà l'1.
![[Pasted image 20230915100228.png]]
Esempio di un client device (EEPROM), che ha un indirizzo a 7 bit, in cui sono presenti i valori dei pin A0, A1 e A2. 
![[Pasted image 20230915100343.png]]
Ogni pin è differente dall'altro e questo permette di avere 8 combinazioni possibili di indirizzi.

###### Read/Write bit (R/W segnato)
![[Pasted image 20230915100653.png]]
1 -> l'host vuole leggere dal client
0 -> l'host scrive sul client

###### Acknowledge Bit
![[Pasted image 20230915101516.png]]
Questo bit ricorre ogni nono ciclo del clock.
0 -> il device ricevente Acnowledges o ACK
1 -> il device ricevente non acknowledge o NACK

Questo bit è bidirezionale, ovvero sia il client, sia l'host possono mettere a down l'SDA per fare acknowledge all'altro. Questo dipende da chi riceve i dati.

###### Data byte
![[Pasted image 20230915101846.png]]
Informazioni trasferite, il significato dei bite dipende dall'operazione svolta e dal device.
I bytes possono essere:
- il word address o il register address
- dati che devono essere scritti al client (in un operazione di **Write**)
- Dati provenienti dal client (in un operazione di **Read**)

###### Stop Condition
![[Pasted image 20230915102027.png]]
Condizione che interrompe la comunicazione. Si verifica quando l'SDA viene rilasciato ad high, mentre SCL è high.
Entrambe le linee rimangono in stato di idle, ad high, così da essere pronte per essere usate nella comunicazione successiva.

___

### Periferiche I2C del PIC
Nei PIC ci sono moduli o registri per gestire e conficìgurare la comunicazione via I2C. 
Per dispositivi PIC a 8 e a 16 bit il modulo I2C è incluso nei registri dell'MSSP (HOSt Synchronous Serial Port). L'MSSP può funzionare usando protocollo SPI o I2C.
![[Pasted image 20230915102812.png]]
Nei dispositivi più recenti si usa la notazione a destra, in cui la x può essere 1, 2 o 3. 

Per la maggior parte dei dispositivi PIC a 16 bit, e per tutti quelli a 32 i registri sono i seguenti.
![[Pasted image 20230915103026.png]]

Poichè l'obiettivo è utilizzare il PIC come dispositivo host i registri più rilevanti sono:
**SSPCON1 / SSPxCON1**
- permette di attivare il modulo di I2C.
- consente di configurare l'MSSP in modo che il dispositivo operi da I2C host.
![[Pasted image 20230915111026.png]]
![[Pasted image 20230915111154.png]]
![[Pasted image 20230915111412.png]]
Avviene il clock stretching fintanto che il client non ha i dati pronti.
![[Pasted image 20230915111621.png]]
SSPEN attiva il modulo I2C e fa si che 2 pin di output siano designati come SDA e SCL.
![[Pasted image 20230915111929.png]]
Registro usabile come flag che si attiva prima di ricevere il byte successivo di dati.
![[Pasted image 20230915112044.png]]
Bit indicatore delle collisioni della linea bus.



**SSPCON2 / SSPxCON2**
- Controlla la comunicazione e il data transfer.
- controllo condizioni di start e di stop.
- controllo le condizioni  di read, write e acknowledge.
- configura il modulo per ricevere un bit dal client.
![[Pasted image 20230915112253.png]]
La _general call_ è una modalità in cui ogni client risponde ad una general call.
ACKSTAT è 0 quando è ricevuto ACK, e settato a 1 quando NACK è ricevuto.
ACKDT a 0 manda un ACK, mentre settato a 1 manda un NACK.
ACKEN a 1 manda il comando di inizio ackowledgment.
RCEN da il controllo dell'SDA al cliente e il device genererà un ciclo sul SCL per facilitare il datatransfer.

![[Pasted image 20230915113503.png]]
Repeat Start Condition è una condizione per cui invece dello stop si mette un trnsizione che porta dallo stop allo start. Non sempre si può usare, si trova questa informazione nel datasheet specifico del device. La condizione di restart usata per permettere all'host di mantenere il controllo dell'I2C bus.






**SSPBUF / SSPxBUF**
- usato per ricevere o trasmettere dati sul I2C bus.

**SSPADD / SSPxADD**
- contiende il client address del client.
- setta la frequenza del clock, SCL.
- (frequenza dell'oscillatore) / ( 4 * (SSPADD + 1) )


**SSPSTAT / SSPxSTAT**
Registro status
![[Pasted image 20230915113907.png]]
Slew rate control utile quando si usano altre frequenza di clock. Permette di rallentare il falling edge del SCL e del SDA, utile per applicazioni sensibili al rumore/interferenze.
![[Pasted image 20230915114153.png]]
![[Pasted image 20230915114230.png]]
![[Pasted image 20230915114241.png]]
Il bit UA se a 1 indica che l'user deve aggiornare l'indirizzo al 10 bit del client.
![[Pasted image 20230915114458.png]]


### Settaggio del I2C con MPLAB
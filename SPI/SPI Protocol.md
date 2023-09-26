Serial Peripheral Interface

___

![[Pasted image 20230925094717.png]]
Interfaccia seriale a 4 cavi, sviluppata da Motorola negli anni '80. Ha migliori prestazioni per quanto riguarda la velocità di trasmissione rispetto a UART e I2C, e supporta comunicazioni full-duplex.
Concetto definito in modo non stringente, perché non definisce il numero di byte trasmessi e livelli di voltaggio.
Usato solitamente per collegare uno smart controller ad una meno "smart" periferica.


SPI utilizza un protocollo master/slave (un master singolo ed uno o più slaves). I master sono i controllers, mentre gli slaves sono le periferiche.

![[Pasted image 20230925095440.png]]
L'SPI è basata su un sistema a 4 cavi:
- **Chip Select (CP**): con cui si sceglie il target della comunicazione (anche chiamata "Slave select").
- **Synchronous Clock (SCLK)**: fornisce la sincronizzazione per i messaggi.
- **Master Out Slave In (MOSI)**: Linea che trasmette i dati inviati dal master.
- **Master In Slave Out (MISO)**: Linea che trasmette i dati inviati dallo slave al master.

NB. ci sono molteplici nomi delle 4 linee.

![[Pasted image 20230925100111.png]]

CS è spesso scritto con il cappello perché di default ha valore alto. La comunicazione con lo slave è iniziata dal master abbassando la linea CS a basso. Dopo di che, basandosi sul clock, viene letto il segnale della linea MOSI. Quando la comunicazione è completa CS ritorna al valore di idle (alto).

Vantaggi:
- non viene utilizzato l'indirizzo dei dispositivi, come in I2C, dato che c'è una linea che permette di attivare la comunicazione tra i vari devices.
- si possono usare linee singole o molteplici.
- il segnale di clock è generato dal master e per questo gli slaves non devono avere un clock interno.
- solitamente la velocità è nel range dei MHz, che rende la comunicazione più veloce della UART o di I2C.

La finalità di avere un clock è indicare quando leggere i dati: più nello specifico quando leggere i valori di tensione. Un bit è letto ad ogni segnale del clock. 
Il clock può essere idle low o idle high, e si può fare sample dei dati alrising edge o al falling edge del valore di tensione del clock.

I dati mandati sono in formato di byte, con LSB o MSB first.
Il numero di byte dipende dall'implementazione. Infatti possono essere inviati sequenzialmente molti bytes e CS può essere mantenuto basso durante tutti i bytes.

La Linea MISO non sempre è utilizzata perché alcuni device ricevono solo dati dal master. In altri casi la linea MISO viene usata per rispondere a dati ricevuti sulla MOSI (comandi, queries...).
NB -> dato che solo il master genera il segnale di clock, il master deve sapere quanti bit devono essere inviati se lo slave invia dati, al fine di generare un giusto numero di cicli del clock. Per questo motivo il master interrogherà lo slave.

### Clock polarity
![[Pasted image 20230925105845.png]]

![[Pasted image 20230925105930.png]]

### Clock Phase
![[Pasted image 20230925110022.png]]

Il dato si può campionare sul leading edge o sul trailing edge del segnale di clock.
Dato che si può avere il clock con un valore di default alto o basso, e che si può campionare il segnale al primo o al secondo edge del segnale di clock esistono 4 mode.
![[Pasted image 20230925110305.png]]
![[Pasted image 20230925110333.png]]

### Molteplici Slaves sulla stessa linea SPI
![[Pasted image 20230925110652.png]]
Ogni slave avrà una linea CS dedicata con il master. Tutte le altre linee sono comuni tra i nodi. Tale configurazione prende il nome di **indipendent slaves**.
![[Pasted image 20230925111331.png]]
Nella configurazione **Daisy chain** gli slaves condividono anche la linea CS. Si ha poi solo una linea MOSI; i dati possono essere mandati agli slave successivi inviando a cascata i dati dal MISO dello slave al MOSI dello slave successivo, durante cicli di clock successivi. 
La stessa logica è usata per l'invio di dati dagli slave al master. Il primo slave invia i dati sulla linea MISO alla porta MOSI dello slave successivo. Poiché gli slave sono collegati in modo concatenato i dati arrivano in cascata all'ultimo slave che li invierà al master.










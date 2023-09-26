
___
### Architettura di un PIC16F1XXX
il pic è come un mini pc o un microcontrollore in cui:
- il PIC16 è il **CPU**
- alcuni pin permettono di collegarsi a **device di input** (switch, sensors)
- alcuni pic si collegano a **device di output** (LED, LCD, motori)

### Come selezionare un device PIC16F1
- quanti I/O sono necessari (6, 8, 14, 18, 20 ... 64)
- quali periferiche sono necessarie (UART, ADC, I2C, PWMs)
- altri fattori (possibilità di espansione, tipo di pacchetto, librerie del firmware, dimensione del codice o dei dati usati)

![[Pasted image 20230825101420.png]]


Il PIC16 ha un architettura **Harvard**, ovvero la memoria per il programma e per i dati sono separate. Questo permette di avere bus di diversa ampiezza. Questo migliora la larghezza di banda e consente un esecuzione più veloce.

![[Pasted image 20230825101743.png]]



___
### Block diagram del funzionamento del PIC16

![[Pasted image 20230825102019.png]]

**Flash Based Program Memory**: dove sono salvate tutte le istruzioni per eseguire il programma.

Program Counter punta a regioni della FBPM per far eseguire certe porzioni di codice, le istruzioni quindi sono caricate nel Istruction Decoder per essere decodificate. Le info decodificate lavorano con il Working Register (con anche i dati salvati nella data memory, dati relativi a device esterni) per eseguire operazioni.

___
### Literal istructions

![[Pasted image 20230825102902.png]]

Le operazioni sono eseguite sequnzialmente dal CPU. 

![[Pasted image 20230825103136.png]]
In alto c'è l'struzione.
Il W regiter è il Working register.
A destra c'è una frazione del data memory, chiamato register files (che vanno dall'indirizzo 20h a quello 2Bh.
Quello piccolo a destra è lo status register. 

L'operazione _mov_ dice di muovere il valore letterale (valore della costante) nel W register. Il valore della costante da spostare è specificato nell'istruzione letterale, 0x55.
Quando l'operaione viene eseguita il W register passa dal valore 0xAA al valore 0X55.

___
#### Addizione op.

```
addwf 0x29 f
```
questa operaione invece significa aggiungere il valore presente nel W register (0x10) al valore presente nel file register. 0x29 è l'indirizzo del file register (quindi si considera il valore 55).
**f** è il designator, ovvero un indicatore di dove viene salvato il risultato dell'operazione. f ovviamente sta per file register.

0x10 + 0x55 = 0x65

0x65 viene salvato al posto di 55.

___
### Sottrazione op.

```
subwf
```
questa operaione è la sottrazione, funziona identica all'addizione.

___
### Decrement op.

![[Pasted image 20230825105111.png]]

```
decfsz 0x29 f
```

si legge:
		decrement f skip if 0

decrementa il valore nel file register (all'indirizzo 0x29), se il risultato del decremento è 0 l'operazione successiva è skippare l'operazione successiva.
La prima volta che viene eseguita il risultato non è 0 quindi viene eseguito got next. La volta successiva passa direttamente a end.

operazione simile a questa, solo con l'incremento è
```
incfsz
```


___

### pro e contro di ASSEMBLY

PRO
- compatto
- veloce

CONTRO
- difficile da debuggare e strutturare
- più lento da fare per la complessità
- non intuitivo
- tedioso, noioso

SI USA QUINDI SEMPRE C PERCHE
- più intutivo
- minor time to market
- sfrutto il compiler per la traduzione da C a Assembly
- 95% dei customers usa C


esempi di conversione da C ad Assembly
![[Pasted image 20230825111706.png]]

![[Pasted image 20230825111754.png]]

___

### Esempio 1 in MPLAB

- selezionare device PIC16F1 5323
- selezionare dove salvare l'esempio
- selezionare il compiler XC8
- finish e crea esempio vuoto Lab1.X


### MCC
è una GUI (Grafical User Interface user friendly).
- ha bottoni e finestre per modificare o selezionare features
- crea codice per inizializzare periferiche
- ha funzini in C per utilizzare periferiche


![[Pasted image 20230825120742.png]]


### Struttura delle porte I/O nel PIC
![[Pasted image 20230825122036.png]]

Ha una Drive Capability di 50 mA, motivo per cui puo alimentare direttamente un LED. Ci sono limiti sul numero di LED che si possono alimentare contemporaneamente. Non server quindi un transistor sull'output.

È presente un TRISx register (o direction control) disponibile per ogni porta I/O, il quale setta la direzione per quella specifica porta. Quindi può determinare se la porta opera da input o da output.

Ogni porta ha un Latch ed è possibile scrivere sulla porta o sul latch, avendo lo stesso effetto. Quando leggo invece dal latch leggo l'ultimo valore, mentre quando leggo la porta leggo il valore della porta (I/O pin a destra) sul pin.

Ogni linea I/O è protetta da diodi di protezione ESD. Protegge dalle scariche elettriche provenienti dall'esterno.

Ogni linea I/O è resettata su un valore di default (high impedance) quando si resetta il dispositivo.

I pin multiplexati con funzioni analogiche si impostano per default come ingressi analogici all'avvio.

### Schema esempio
![[Pasted image 20230825123310.png]]


### Funzionamento timer

![[Pasted image 20230825151154.png]]
L'internal timer a 16 bit incrementa di 1 il valore di TMR1  ogni time cycle (Tcy). Il TMR1 incrementa di 1 partendo da F061 (1) fino ad arrivare al passaggio da FFFF a 0000 (massimo). Quando il valore raggiunge 0000 viene generato un interrupt.
![[Pasted image 20230825151454.png]]

In un millisecondo [mS] viene incrementato il contatore. Quando il contatore è 500 viene fatto lampeggiare il LED e azzerato il contatore stesso.

### Interrupts
![[Pasted image 20230825152450.png]]
![[Pasted image 20230825153017.png]]
Le istruzioni sono eseguite in sequenza. Arrivati all'istruzione I avviene un interrupt. Ciò che succede è che:
- l'indirizzo dell'operazione successiva (I+1) viene salvata nello stack
- il pointer del pc viene riportato direttamente all'istruzione 4
- al return dell'interrupt viene riportato il puntatore del PC all'indirizzo salvato nello stack


Avere un interrupt permette di gestire immediatamente le periferiche senza delay.
Eventi time based sono gestiti in modo istantaneo.
Dopo l'interrupt le funzioni normali del CPU continuano.

#### Tipologie di interrupts sun un PIC16F1
**Single Interrupt Vector**
Ogni interrupt va alla location 4 nel Program Memory. È l'user che deve stabilire le priorità, mettendo ad una location con numero più basso le operazioni con priorità maggiore.

**One Global Interrupt Enable (GIE) bit**
GIE = 0 quando un interrupt avviene
GIE = 1 quando RETFIE è eseguito

**One Peripheral Interrupt Enable (PIE) bit**
bit che consente di attivare interrupt provenienti da periferiche.
PIE = 0 interrupt periferiche disattivato
PIE = 1 interrupt periferiche attivato

**Ogni interrupt ha il proprio _enable_ e la propria _flag_**
ad esempio TMR1IE (timer 1 interrupt enable) e TMR1IF (timer 1 interrupt flag)

**L'MCC si occupa dell'Interrupt Coding**



Un interrupt è usato come un segnale rapido che indica quando una periferica ha terminato il proprio lavoro o funzione. È consigliabile mantenere la durata dell'interrupt più corta possibile (esempio della pausa dal meeting perchè qualcuno arriva a dire che i donuts sono pronti). Nel codice l'interrupt viene controllato solo valutando il flag correlato.
Usare macchine a stati per eseguire i tasks nell'applicazione.

### State machine dell'esempio

![[Pasted image 20230825155344.png]]

In INIT inizializzo i parametri dello stato e passo a WAIT STATE.
In WAIT STATE attendo 500 mS e passo a BLINK LED STATE.
In BLINK LED STATE spendo il led e torno a WAIT STATE.

In un progetto possono esserci diverse state machines(LED, per i pulsanti, per la UART). Nessuna state machine occupa la CPU, ma ognuna viene servita in modo ciclico, tutte sono eseguite nel main loop.
![[Pasted image 20230825160057.png]]

Consigliabile usare switch case invece di if else perchè molto più rapido.

![[Pasted image 20230825160408.png]]


dato che il timer causa un interrupt ogni mS e il pic esegue ad una frequenza di 16 MHz, allora in 1 mS sono eseguite 4000 istruzioni (i tasks sono eseguiti molteplici volte in un mS).

bastano 5 mS per aggiornare i LED/LCD e 50/100 mS per campionare differenti chiavi.

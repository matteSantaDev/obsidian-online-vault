#embedded 

---

Sistema utilizzato per il trasferimento dati molto affidabile, con costo ridotto ed alto data rate.

Sono utilizzati 2 cavi CAN high e CAN low.
![[Pasted image 20230828122132.png]]

Alla fine del can bus ci sono 2 "termination resistors" che si connetto ai cavi alto e basso.
![[Pasted image 20230828122433.png]]
Se tutto funziona basta mettere un multimetro alla fine in parallelo e controllare se legge 60 ohm.

Si usano 2 cavi intrecciati per inviare dati perché:
- 2 cavi permettono di aumentare la robustezza del trasferimento dati e evitano errori causati dalle interferenze elettromagnetiche che causano picchi di tensione che alterano la corretta lettura dei dati inviati.![[Pasted image 20230828122917.png]] Si potrebbe aumentare il voltegaggio, cosa che però necessita che la linea deve essere in grado di supportare un segnale con massiama tensione a 12 volt invece che a 5 volt.![[Pasted image 20230828123105.png]]  Il metodo che si usa invece è avere due linee che hanno un segnale opposto, sempre all'interno del range di 5 volt.![[Pasted image 20230828123645.png]] Utilizzando solo sulla differenza tra can high e can low si ottiene un segnale pulito.![[Pasted image 20230828123928.png]]
- ogni cavo in cui scorre corrente emette il proprio "rumore" elettromagnetico. Dato che però che i cavi sono intrecciati e quando il campo magnetico del can high va su, quello del can low scende, e viceversa. Per un osservatore esterno quindi i fili intrecciati non emettono alcun rumore elettromagnetico.![[Pasted image 20230828124425.png]]


Il digital 1 è uno stato recessivo, mentre o digitale è lo stato dominante. Questo perché se voglio scrivere un 1 non devo far altro che tenere le due linee alla tensione a cui si trovano, ovvero 2,5 volt. Se voglio scrivere uno o le due linee devono essere attivamente portate rispettivamente a 3,5 e a 1,5.

Si usano le 2 resistenze all'inizio e alla fine dei cavi perché:
- aiutano a riportare le 2 linee a 2,5 volt
- si vuole eliminare la bus reflection, ovvero le resistenze impediscono che il segnale si propaghi nel senso e sulla linea opposta quando arrivano alla fine del cavo.![[Pasted image 20230828125108.png]] Le resistenze sono a 120 Ohm perché compensano l'impedenza di 2 cavi attorcigliati.
- Minore è la resistenza e più velocemente le tensioni delle linee sono riportate a 2,5 quando il segnale viene interrotto. Con una resistenza maggiore il segnale non avrebbe una ritorno così ripido, disegnando una curva che non è adatta per i segnali digitali.![[Pasted image 20230828125714.png]]


Il PDM deve è collegato ai cavi senza resistenze è per questo deve essere connesso alla rete ad una distanza non maggiore di 30 cm. Questo perchè quando viene emesso il segnale, si immette anche un segnale riflesso. Se la distanza è breve il segnale non cambia significativamente e si ha un valore netto, mentre se la distanza è maggiore lo scalino viene tutto frastagliato.
L'hub infatti è fatto in questo modo![[Pasted image 20230828144720.png]]Nei casi in cui il collegamento è > 30 cm si ha un disturbo dovuto al segnale riflesso.
![[Pasted image 20230828144903.png]]Quello che si fa per ovviare a tale problema è fare in modo che il filo che collega i vari componenti (che può anche superare i 30 cm) si il can bus stesso.
![[Pasted image 20230828145122.png]]


### Can BUS packet
![[Pasted image 20230828151312.png]]
- Start Of Frame: send 0
- Identifier (11 bits): 2048 id univoci, ma ci può essere anche un extended identifier che è lungo 29 bits.
- RTR: unico bit, indica se stiamo mandando effettivamente dati o solo un messaggio con il suo id.
- IDE: bit usato per indicare se usiamo l'identificatore a 11 bit o a 29.
- r0: bit riservato, non usato.
- DLC: data length, quanti bytes sono nella sezione dati del pacchetto.
- byte dedicati ai dati veri e propri che devono essere trasmessi.
- CRC: cyclic redundancy check, code che permette di eseguire un controllo sui dati inviati, consente di individuare se il messaggio contiene errori.
- ACK: acknoledgment

___


1. **PDM**: PDM comunemente sta per Power Distribution Module, ovvero un modulo di distribuzione di energia. Si tratta di un componente elettronico utilizzato nei veicoli per gestire e distribuire l'energia elettrica a vari sistemi e componenti. Un PDM nel contesto di un bus CAN avrebbe probabilmente il compito di controllare e coordinare la distribuzione di energia alle diverse parti del veicolo, comunicando eventualmente con altri sistemi tramite il bus CAN per garantire una gestione efficiente dell'energia.
    
2. **ECU**: ECU sta per Electronic Control Unit. Nel contesto di un bus CAN, un ECU si riferisce a una delle varie unità di controllo presenti in un veicolo che comunicano tramite la rete Controller Area Network (CAN). Queste unità sono responsabili del controllo di sistemi specifici come il motore, il cambio, l'ABS (Sistema di frenata antibloccaggio), gli airbag, ecc. Scambiano informazioni e comandi attraverso il bus CAN per garantire la corretta coordinazione e il funzionamento dei sistemi del veicolo.
    

In sintesi, sia il PDM che l'ECU sono componenti fondamentali all'interno dell'architettura di un veicolo che utilizzano il bus CAN per la comunicazione e il coordinamento, ma svolgono scopi differenti. Il PDM gestisce la distribuzione dell'energia, mentre l'ECU gestisce e controlla vari sistemi del veicolo.


___

### Can BUS arbitration
viene mandato il messaggio con id inferiore.
![[Pasted image 20230828154255.png]]


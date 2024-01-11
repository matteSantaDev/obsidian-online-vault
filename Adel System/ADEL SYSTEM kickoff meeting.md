12/12/2023 
#AdelSystem

____

modulo per panelli solari  
si accende automaticamente quando pannello riceve luce  
dividere potenza tra carico e batterie (minimizzare uso corrente)  
se manca corrente ci pensa carica batterie intelligente.

![[Diagram.svg]]
#### potenza pannelli  
ricerca dell'ottimo, massima corrente e minima tensione e viceversa, massimizzare potenza

### Struttura progetto  
- config con parametri const ma modificabili. (sola lettura, allarmi, impostazione, monitoraggio(tensione))  
- flusso dati in input  
- flusso dati output  
- comando proveniente dal modulo software che fa supervisione

___
## use case SOL30  (funzioni base/uso standard)


- **wake up on sunset**, all'attivazione pannello solare. all'inizio dell'erogazione della corrente si accende il chip. devo vedere quantità di potenza erogata. 
  - caricamento delle configurazioni e parametri.  ✅
  - all'accensione viene eseguita l'autodiagnosi, qual'è l'evento che ha provocato l'accensione (per verificare che l'accensione sia causata da pannello stesso) (autodiagnosi continua che genera allarmi).  ✅
  - chip da segnale di wake up al CB (carica batterie intelligente) chiudendo un contatto.
  - modulo si mette in uno stato di funzionalità minima [modalità autonoma] in cui non ricevo messaggi dall'esterno ma resto in attesa (eroga massima potenza in uscita cercando di seguire curva massimo) in attesa di messaggio su can.✅
  - se modulo riceve comandi via can bus attiva [modalità controllata] che modifica parametri di funzionamento, modifico corrente o tensione di uscita. Se canale di can bus si perde torno in modalità autonoma dopo can bus.✅
  - chiarire stato SOL30 di chiusura 

==modulo can bus è modulo a se stante, comunica via registri==

- **firmware udate**
	- la modalità di firmware update parte alla ricezione di un comando inviato tramite can bus.
  - durante **firmware update** l'alimentazione viene fatta solo con batterie.
  - è necessario scollegare il pannello solare e mettere il sistema in sicurezza.
  - viene lanciato il bootloader.

- **external comunication** è modulo che riceve segnali da can bus e modifica registri. (già implementato)



## Possibili use cases non implementati
- **configurazione pannello per risparmio energetico**, il pannello carica batteria o fornisce energia al carico (solo quando c'è sole in modo continuo).
- **in assenza di rete** si deve alimentare batteria, quando si scarica muore tutto, fare in modo di limitare il consumo per massimizzare la vita del dispositivo.
- **funzionamento ciclico**, come il funzionamento dei lampioni durante la sera. La logica è la stessa anche in presenza di rete (ma c'è possibilità che non si voglia usare).
- **gestione cortocircuito e overload**. Corto deve essere gestito al massimo ==nell'ordine dei 100 microsec==. Il codice che gestisce gli interrupt è salvato nella RAM, perché la flash è troppo lenta e non consente una gestione real time. I sovraccarichi devono essere gestiti ==nell'ordine minori del secondo==, fare media e ridurre la tensione in uscita.
- **quando viene a mancare energia dal panello** deve: ✅
	- spegnersi, smettere di assorbire energia dalla batteria quando non c'è sole.
	- se non c'è attività sul pannello dei comandi per tempo max eseguire spegnimento.
- **impostare funzionamento via interfaccia utente** (2 led e un bottone): programmare la tensione di alimentazione e altri parametri di funzionamento.✅

### Allarmi  
in presenza di anomalie (temperatura eccessiva, tensioni eccessive, assenza di tensione, cortocircuito in output)  
- attivo bit in registro di allarme.
- segnalo anomalie con certo numero di lampeggi del led.
- a seconda dell'allarme attuo azioni per mettere in sicurezza dispositivo (es. `T` troppo alta riduco corrente in modo graduale, in caso di cortocircuito stacco le uscite)

### Log  
con certa cadenza, impostabile, salvare log in file dati corrente potenza (tutti dati di interesse) oltre che agli eventi (sovraccarico, n vole e orario)

___

### Download progetto e configurazione IDE 
versione 1.12.1

### Dati funzionamento scheda
alimentazione 24V 1.50 mA alimentato dal lato panel o con usb.


---



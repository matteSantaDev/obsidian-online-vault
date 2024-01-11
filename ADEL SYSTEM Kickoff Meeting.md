#AdelSystem 

12/12/2023
____
modulo per panelli solari
si accende automaticamente quando pannello riceve luce
dividere potenza tra carico e batterie (minimizzare uso corrente)
se manca corrente ci pensa carica batterie intelligente.

#### potenza pannelli
ricerca dell'ottimo, massima corrente e minima tensione e viceversa, massimizzare potenza

### struttura progetto
- config con parametri const ma modificabili. (sola lettura, allarmi, impostazione, monitoraggio(tensione))
- flusso dati in input
- flusso dati output
- comando proveniente dal modulo software che fa supervisione

# use case SOL30
- wake up on sunset, all'attivazione panello solare. all inizio dell'erogazione della corrente si accende il chip. devo vedere quantità di potenza erogata. 
	- caricamento delle configurazioni e parametri.
	- all accensione autodiagnosi, qual è evento che ha provocato accensione (per verificare che l'accensione sia causata da pannello stesso) (autodiagnosi continua che genera allarmi).
	- chip da segnale di wake up al CB (carica batterie intelligente) chiudendo un contatto
	- si mette in uno stato di funzionalità minima [modalità autonoma] non ricevo messaggi dall'esterno ma sto in attesa (eroga massima potenza in uscita cercando di seguire curva massimo) in attesa di messaggio su can.
	- se riceve comandi via can bus attiva [modalità controllata] che modifica parametri di funzionamento, modifico corrente o tensione di uscita. se canale di can bus si perde torno in modalità autonoma dopo can bus.

==modulo can bus è modulo a se stante, comunicano via registri==

- alla ricezione di comando via can bus
	- durante **firmware update** alimentazione viene fatta solo con batterie
	- scollegare pannello solare e mettere sistema in sicurezza
	- lanciare bootloader

- external comunication è modulo che riceve segnali da can bus e modifica registri. (già implementato)

### allarmi
in presenza di anomalie (temperatura eccessiva, tensioni eccessive, assenza di tensione, cortocircuito in output)
- attivo bit in registro di allarme
- segnalo anomalie con certo numero di lampeggi del led
- a seconda dell'allarme attuo azioni per mettere in sicurezza dispositivo (es T troppo alta riduco corrente in modo graduale, in caso di cortocircuito stacco le uscite)

### log
con certa cadenza, impostabile, salvare log in file dati corrente potenza (tutti dati di interesse) oltre che agli eventi (sovraccarico, n vole e orario)


- configurazione pannello per risparmio energetico, pannelo carica batteria o fornisce energia al carico (solo quando c'è sole in modo continuo)
- in assenza di rete si deve alimentare batteria, quando si scarica muore tutto, fare in modo di limitare consumo per massimizzare vita
- funzionamento ciclico, funzionamento lampioni durante la sera. anche in presenza di rete (ma c'è possibilità che non si voglia usare)
- gestione cortocircuito e overload. Corto deve essere deve essere gestito nell ordine max 100 microsec. gestione con interrupt con codice in RAM, flash troppo lenta. gestione real time. I sovraccarichi devono essere gestire nell ordine minori del sec., fare media e ridurre uscita tensione.


### download progetto e configurazione ide
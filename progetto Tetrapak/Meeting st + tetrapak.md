#tetrapak 

---

**ST**
- fare powerpoint dettagliato sui problemi e diferenze datalog e datalog high speed
- modificare parametri di inizializzazione


---

**Tetrapak**
fare test con datalog low speed in cui scrivo dati dal client al server e li mostro sul terminale

[[GUI]] definisce il jason, si crea internamente e viene mandato.


- pc interroga scheda su che configurazione è caricata li sopra
- poi si sceglie se creare nuovo file di configurazione, se non è specificato si crea cartella con nome univoco. Il nome è simile a quello con cui sono salvati i file salvati sulla sd. Programma poi, una volta connesso, li salva nella cartella apposta. Definire nome file nel json.
- mettere time stamp nelle cartelle che deve combaciare con time stamp dei dati.
- pc riceve da scheda i dati mentre ne manda anche alla scheda. Unico comando che ferma è stop_acquisition
- usare async_io, usarlo per programmare in maniera asincrona tutto il file di prova
- comandi dell'interfaccia sono START ACQ, STOP ACQ, GET ACQ, NEW ACQ e UPDATE FIRMWARE
- guardare se il reboot può essere fatto via software, OTA (controllare che si chiudano tutti i file e si interrompano tutte le att)

fare:
- stress test throughput fino al salvataggio e la trasmissione per individuare collo di bottiglia



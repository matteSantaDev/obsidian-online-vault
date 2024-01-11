#informatica

Power-On Self-Test

fase di autodiagnosi di personal computer (come anche di altri dispositivi, ad esempio router e stampanti) avviata automaticamente dal [[BIOS]] all'accensione per testare il corretto funzionamento dell'hardware prima dell'avvio delle successive fasi del processo di [[boostrap]].

Oltre al funzionamento della [[scheda madre]] il POST può verificare anche il funzionamento delle periferiche più comuni (mouse, tastiera e scheda video).


FUNZIONAMENTO
All'accensione del sistema viene caricato dalla [[ROM]] il primo programma del BIOS che esegue il POST.
Successivamente sarà il programma stesso a dover caricare ulteriori software per la gestione di alcuni _task_ atti ad inizializzare specifiche periferiche, in particolar modo schede video e dispositivi [[SCSI]].

Nel corso del processo di POST i passi che vengono generalmente eseguiti sono i seguenti:
	-   verifica dell'integrità dello stesso codice del BIOS;
	-   determinazione della causa che ha innescato il processo di POST (accensione, risveglio dallo  _standby_, ecc.);
	-   individuazione, determinazione della dimensione e verifica della [[memoria primaria]] del sistema;
	-   individuazione, inizializzazione e catalogazione di tutti i [[bus]] ed i _device_ del sistema;
	-   (se necessario) rilascio del controllo del processo a BIOS specializzati (ad esempio BIOS video);
	-   rendere disponibile un'interfaccia utente per la configurazione del sistema (alla pressione di un predeterminato tasto);
	-   identificare, organizzare e selezionare i _device_ pronti per la continuazione della fase di bootstrap;
	-   avvio del programma di [[boot]].
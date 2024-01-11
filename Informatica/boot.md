#informatica

Il _booting_ è il processo di avvio del computer. Quando il CPU viene acceso non ha nulla sulla memoria. Quindi per far partire il computer deve essere caricato sulla memoria il sistema operativo nella _Main Memory_. Dopo questo passaggio il computer è pronto per ricevere comandi dall user.

Il booting avviene quando è **acceso il computer** o si esegue il **restart** e può essere eseguito tramite hardware (schiacciando un bottone) o fornendo comandi software.

Un **Boot Device** è un dispositivo che carica il sistema operativo e che contiene le istruzioni e i file che consentono di avviare il computer. (es. hard drive, floppy disk drive, CD drive).

ci sono 2 tipi di booting:
- **Cold Booting**: anche chiamato _hard boot_, ed è il processo dove prima è avviato il computer dal suo stato iniziale (schiacciando il tasto power). Le istruzioni sono lette dalla ROM e il sistema operativo è caricato sulla memoria princopale.
- **Warm Booting**: anche chiamato _soft boot_, riferito a quando facciamo restart del computer. In quetso caso il computer non inizia dallo stato iniziale. Usato quando il sistema si blocca ed è necessario un riavvio. Si effettua schiacciando il bottone di restart o con CTRL+ALT+DELETE.

STEPS OF BOOTING
1. _Startup_: power on, si fornisce corrente ai componenti principali come il BIOS e il processore.
2. _BIOS: Power On Self Test_: test iniziale eseguito dal [[BIOS]]. Si verifica lo stato dei device di input/output, della memoria principale del computer, dei [[disk drive]]... Se c'è qualche errore il sistema produce un suono beep.
3. _Loading of OS_: il sist. operativo è caricato nella memoria principale e comincia ad eseguire tutti i file e le istruzioni iniziali.
4. _System Configuration_: si caricano i [[drivers]] nella memoria principale. I drivers sono programmi che facilitano il funzionamento delle periferiche.
5. _Loading System Utilities_: caricati nella memoria principale le [[system utilities]], ovvero basic functioning programs come colume control o antivirus.
6. _User Autentication_: se è stata messa la password il sistema verifica l'dentità dell'user; inseriti user id e password il sistema inizia.


BOOTING NEI MICROCONTROLLORI
nei [[bare metal]] [[embedded system]]s solitamente sono usati i microcontrollori, nei quali non c'è [[sistema operativo]], e in cui non è usato il [[Kernel]] per gestire le risorse del microcontrollore.
Un codice bare-metal è eseguito direttamente sul microcontrollore; in un ciclo _while_ viene eseguito il codice iterativamente all'infinito.

**start up file**: codice scritto il C o [[assembly]] che esegue diverse funzioni, come la sequenza di booting, l'inizializzazione della RAM e il transfer control alla funzione principale. Contiene inoltre la [[interrupt vector table]] e le [[interrupts service routines]] per ogni interrupt o eccezione.

Quando si compila un codice di uno specifico microcontrollore in un IDE, il compilatore automaticamente include il **startup file** nell'[[immagine del codice binario]]. Quando si fa upload dell'immagine del file binario sul microcontrollore, lo _startup file_ diventa disponibile nella memoria flash del microcontrollore insieme ai file del firmware.

La sequenza di booting di un microcontrollore inizia appena si accende collega l'alimentazione o si schiaccia il bottone di reset. Dopo questo il microcontrollore va a prendere la prima istruzione dall'address 0, in cui l'address del [[reset handler]] è disponibile.

Lo spazio di memoria allocabile inizia dallìindirizzo 0x0000_0000 e finisce in 0xFFFF_FFFF. Gli indirizzi inizia dal 0x0000_0000 contengono la _interrupt vector table_.
Quando un microcontrollore viene resettato, il pc è caricato con l'address 0x0000_0000; questo prmo indirizzo contiene l'indirizzo dell'inizio dello stack o il valore che sarà caricato nel main stack pointer.
Dopo questo il microcontrollore carica il valore salvato nell'address 0x0000_0000 nel main stack pointer o [[MSP]].



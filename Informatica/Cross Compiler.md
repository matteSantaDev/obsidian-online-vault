#informatica

La cross compilazione è una tecnica con cui si compila un codice sorgente con un cross-compilatore, ottenendo un file binario eseguibile su un elaboratore con architettura diversa da quella della macchina su cui è stato lanciato il cross compilatore stesso.

La crosscompilazione è usata anche quando è necessario compilare un programma per un sistema operativo differente dalla macchina su cui si trova il compilatore (o linker).

La macchina che esegue la cross-compilazione è chiamata *host*, mentre la macchina su cui si eseguirà il binario ottenuto dal processo di cross-compilazione è detto *target*.


UTILIZZI
- **[[sistemi embedded]]**: cross-compilazione usata in sistemi embedded in cui le risorse sono molto limitate e non sarebbe quindi possibile effettuare la normale compilazione. Inoltre molti sistemi embedded, come i microcontrollori, non possiedono nessun sistema operativo ad eccezione di un piccolo [[BIOS]]. Per questo motivo quindi non è possibile eseguire nessun compilatore o linker su questi dispositivi.
- **sistemi di grandi dimensioni**: in una grande azienda o in una server farm che possiede un gruppo non omogeneo di piattaforme hardware e/o software è necessaria la tecnica della cross-compilazione quando deve essere distribuito un nuovo software, soprattutto quando sono usati diversi sistemi operativi.
- **nuovi software di base**: Le operazioni di scrittura, compilazione e link di nuovi sistemi operativi devono essere necessariamente effettuate su altri sistemi preesistenti per poterlo salvare su un supporto di memorizzazione esterno. Così facendo si genera un codice oggetto con un file system leggibile dal BIOS che consente di avviare un primo abbozzo del sistema operativo (che almeno supporti la compilazione e il linking)

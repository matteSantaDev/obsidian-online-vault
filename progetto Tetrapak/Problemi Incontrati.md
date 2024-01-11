#tetrapak 

OBIETTIVO: 
	utilizzare la funzione _CDC_Transmit_FS_ affinchè i dati che devono giungere all'[[USB endpoint]] IN siano inviati all'[[interfaccia CDC]]. 
	La funzione _CDC_Transmit_FS_ consente l'invio dei dati al terminale aperto sul pc.
	La funzione _CDC_Transmit_FS_ è stata chiamata in un thread (definito in _ble_comm_manager.c_) precedentemente usato per aggiornare i dati esposti in advertising (la funzione del BLE per questa prova è stata commentata).
	![[Pasted image 20221004102938.png]]

Il file in cui è definita la funzione _CDC_Transmit_FS_ (_usbd_cdc_interface.c_) è incluso in _datalog_application.c_; quest'ultimo a sua volta è stato incluso in _ble_comm_manager.c_. (nessun problema fino a questo punto)

In _usbd_cdc_interface.c_ è usata la funzione **_USBD_CDC_SetTxBuffer_** in cui viene mostrato errore che interrompe la compilazione.

![[Pasted image 20221004111138.png]]
Errore che indica che la funzione non è stata definita.
![[Pasted image 20221004111502.png]]


Ma la funzione è definita in nel file _usbd_cdc.h_, presente nelle cartella Middleware del progetto. Questo path è stato infatti incluso tra quelli che il compilatore deve considerare quando viene eseguita la compilazione. L'errore però persiste. È stato provato a copiare il file _usbd_cdc.h_ e _usbd_cdc.c_
nelle cartelle Inc e Src del progetto ma l'errore permane.
![[Pasted image 20221004111845.png]]

I path sono stati inseriti in _include paths_ di _MCU GCC Compiler_.
![[Pasted image 20221004143053.png]]
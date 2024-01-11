#tetrapak 
***

- eseguito il dowload del loader nell'indirizzo 0x0800 0000 (con STM32CubeProgrammer)
![[Pasted image 20230315121058.png]]

- cambio gli indirizzi di memoria nel linker del progetto![[Pasted image 20230315121159.png]]
- eseguo il flash del progetto sulla board
- il codice compila, ma eseguendolo in modalità di debug quando si fa partire l'esecuzione va in errore.
![[Pasted image 20230315121451.png]]


***
- prova in cui è stato rimesso a 0x2000 0000 l'indirizzo di memoria della RAM1. È stato ottenuto ancora l'errore durante l'esecuzione.
![[Pasted image 20230316101600.png]]
- provando ad usare il linker che ha inviato Giuseppe nella maili sul nostro progetto si ottiene il seguente errore in fase di esecuzione.
![[Pasted image 20230316102009.png]]
- usando il linker file mandato la schda funziona correttamente ma comunque non è possibile effettuare il firmware update dall'applicazione

1) boh
2) boh
3) boh
4) inspiegabile
5) si sarebbe potuto funzionare, tuttavia non conoscendo come il firmware viene passato dalla web app di st all board non è stato possibile implementare il firmware update.
6)  La caratteristica 4 è usata per rendere pubblico ed accessibile ai client lo stato della scrittura dei dati sulla scheda sd (poichè è una procedura che consente al client di non rimanere connesso durante la durata del suo svolgimento).
7) boh
8) si
9) no anche alla velocità dell'acquisizione del client. via BLE il throughput è di 2kB, mentre è molto più alto via usb.
10) il cambiamento di tipo è dovuto all'utilizzo delle code che accettano solo valori con tipo uint32.
11) g


# Risposte domande 2 Tetrapak

1) Forse era possibile; però la caratteristica 3 è usata per la scrittura di indicazioni dal client alla board, mentre la caratteristica 4 è usata per la comunicazione dalla board al client. Infatti lo stato del processo di acquisizione dati su sd deve essere sempre visibile, anche se il client decide di fare partire altri processi (come l'acquisizione ble). 
	Nota:
		Se vengono introdotti dei vincoli per cui il client non può disconnettersi dalla board quando viene fatta iniziare l'acquisizione dati su sd e per cui non possono essere iniziati processi di acquisizione paralleli allora usare una sola caratteristica per fare tutto è possibile.

2) Forse si, non so quali siano i meccanismi con cui l'app di st filtra i risultati. Se il criterio utilizzato per filtrare i dispositivi bluetooth rilevati è solo il nome potrebbe essere fattibile, ma bisogna provare. 
	Nota:
		La difficoltà sta nel fatto che st non ha o non vuole fornire documentazione su come opera la webb app.

3) La decimazione non funziona perché deve essere deciso l'algoritmo con cui fare la decimazione. 
	Quando i dati sono scritti su sd la decimazione non è necessaria quindi tutti i dati sono scritti sul file.
	Quando si utilizza la trasmissione dei dati via ble vengono mandati i primi 210 byte del buffer che si ottiene dal modulo del sensore in questione. Se la lunghezza del buffer è > di 210, i byte >= 210 non sono trasmessi.
	Nota:
		Il decimation rate è un parametro che di fatto ora non è utilizzato perché non è stato definito nel dettaglio l'algoritmo con cui fare decimazione, all'interno del modulo dedicato.
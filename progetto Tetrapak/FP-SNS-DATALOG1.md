#tetrapak 

---


Esempio/pacchetto che permette di implementare un applicazione di datalog ad alta velocità dei dati raccolti dai sensori presenti sulle schede:
				- STEVAL-MKSBOX1V1
				- STEVALSTWINKT1
				- STEVAL-STWINKT1B

Su STEVAL-STWINKT1 e STEVAL-STWINKT1B è possibile anche configurare l'unità [[ ISM330DHCX]] e la [[Machine LearningCore unit]] per riconoscere output dei sensori.



#### SALVATAGGIO DATI DEI SENSORI
I dati misurati dai sensori sono salvati su una scheda microSD formattata con un [[FAT32 file system]] o ne è fatto lo streaming al PC via USB ([[WinUSB class]]) usando il *companion host software* (programma host per il pc).


#### SENSORI PRESENTI NELLA SCHEDA
 - **hts221**: sensore di umidità e temperatura
		 da in output alla callback data_ready un array di dimensione 4, in cui l'ultimo elemento assume i valori _A_ o _B_.
		 


 - **iis2dh**: accelerometro a 3 assi lineare
		 output nella callback dataready è un array di più o meno lunghezza 180, variano solo i primi 4 valori.
		 Nella modalità FIFO il buffer continua a riempire i dati di X, Y e Z dai canali dell'accelerometro fintanto che è pieno (set di 32 campioni). Quando la FIFO è piene viene interrotta l'acquisizione e il contenuto della FIFO rimane non modificato.
		 Viene usato un interrupt 
				 I1_OVERRUN = '1'
		 nel registro ==CTRL_REG3 (22h)==, quando viene interrotta l'acquisizione dati.


- **stts751**: sensore di temperatura low voltage
		output luno 4, ultimo valore rimane _A_, nel sensore **hts221** l'ultimo valore, quando è stata inserito il brakepoint nella callback di questo sensore rimane sempre _B_.

		 



#### CONFIGURAZIONE SCHEDA
La scheda è configurata tramite un file JSON, dall'host (pc) tramite il terminale o stabilendo i parametri dall'applicazione BLE.

L'applicazione BLE oltre a consentire di modificare le configurazioni dei sensori e della scheda, permette lo start/stop dell'acquisizione dati (salvati sulla scheda SD), consente il controllo del labeling e mostra gli output del Machine Learning Core.








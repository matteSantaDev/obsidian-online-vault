#tetrapak #testing-tetrapak

---

###### Obiettivi del test:
- [x] Misurare valore throughput prima della modifica a intensità di comunicazione BLE e ritocchi a delay.

- [x] Misurare throughput post modifiche.


Acquisizione ble di tutti i sensori, durata di 10 secondi. Sono contati i pacchetti dati ricevuti dal client nell'intervallo di tempo della trasmissione dati.
Sono state eseguite 3 acquisizioni (di 10 secondi) consecutive per  ogni sensore. Il valore riportato in tabella è il valore intermedio tra i 3 ottenuti. Notare che l'oscillazione di throughput può variare anche di 200 byte/sec tra due distinte sessioni di acquisizione. 

| Nome Sensore             | throughput prima | throughput dopo |
| ---------------- | --------------- |  --------------- | 
| IIS3DWB       |  814.89                |   1257.22              |
| IIS2DH  *                 |    598.23              |    651.462             |
| IMP34DT05                |     461.43             |  1593.96               |
| ISM330DHCX               |     989.12             |       1185.15          |
| LPS22HH                  |    207.68              |  1372.20               |
| IMP23ABSU                |      1111.19            |   1310.51              |

Il throughput della procedura di trasferimento dati è 2150 byte/sec. Tale valore non è potuto essere migliorato perchè i valori dei delay sono già ottimizzati.


### Bug individuato * [TPSH-76]
Il throughput non è mai costante tra un acquisizione e la successiva. L'acquisizione per il sensore IIS2DH non funziona mai più di 2 volte consecutive. Dopo infatti un paio di acquisizioni il file è creato ma è vuoto, con un throughput di 0. Inoltre il throughput, oltre ad essere molto inferiore rispetto agòli altri sensori, decresce sempre dalla prima alla seconda acquisizione.
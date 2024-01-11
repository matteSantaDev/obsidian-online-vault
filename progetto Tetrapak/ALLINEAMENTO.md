23/12/2022 #tetrapak 

---

OBIETTIVO
creare un esempio in cui sono inviati (tramite BLE) i dati di solo un tipo di sensore alla volta.

---

PUNTO DI PARTENZA
progetto in cui invio correttamente via BLE i dati acquisiti dall accelerometro e giroscopio, e in cui acquisisco correttemente i dati da tutti i sensori richiesti.

Poichè in questa fase del progetto l'obiettivo è inviare i dati di un solo sensore alla volta è usata un'unica coda (creata in origine per il sensore gyro-acc ISM330DHCX). Tale coda è usata anche per i tentativi di invio dei dati di altri sensori.

---

PROBLEMA

![[Pasted image 20221223103038.png]]
external_comm.c (304)

l'inserimento dell'if in cui viene chiamata la funzione _readDataFromSensorQueue()_ fa si che la trasmissione di dati venga interrotta dopo poco andando in hardfault.
L'errore si verifica anche se la condizione dell'if in figura è falsa. 
Se viene commentata il codice funziona.

---

![[Pasted image 20221223105821.png]]


---

comunicazione client-board con BLE

1) usare ble per trasferimento comandi
2) comunicazione wired per comandi

---

documentazione in word quano ci sono moduli completi:

- commenti
- modalità di test
- eventualmente architettura

---

media mobile

sensori -> alg. -> external comunication
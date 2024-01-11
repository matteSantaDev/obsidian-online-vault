Algoritmi che costruiscono modelli basandosi su dati semplici, al fine di fare previsioni e valutazioni di/su nuovi dati.

![[Pasted image 20220929101452.png]]

Il machine learning concettualmente è un sottoinsieme dell'[[Artificial Intelligence]].

Esistono diversi modelli di machine learning, tra cui le [[Regressioni Lineari]], le [[Support Vector Machines]], le [[Random Forests]] e le [[Reti Neurali]].


STEP DEL MACHINE LEARNING
- raccogliere dati
		importante fornire dati affidabili affinchè il modello di machine learing possa trovare i pattern corretti. La qualità dei dati determina l'accuratezza del modello. Se si usano dati incorretti o non aggiornati si avranno outcomes errati o previsioni non rilevanti.
		Un buon set di dati contengono molte poche ripetizioni ed ha una buona rappresentazione delle varie sottocategorie e classi esistenti.
		
-  preparare i dati
		- raccogliere tutti i dati scelti e **randomizzarne** l'ordine così che siano equamente distribuiti e l'ordine non impatti sul processo di apprendimento.
		- pulire i dati ed **eliminare qulli indesiderati, valori mancanti, colonne & righe, valori duplicati e data type conversion**.
		- visualizzare i dati per comprenderne la struttura e la relazione tra le variabili e le classi presenti.
		- dividere i dati puliti in 2 set, un **training set** (set da cui il modello impara) e un **test set** (per valutare l'accuratezza del modello dopo il training).

- scegliere il modello
		scegliere il modello migliore per il task da svolgere. Diversi modelli si adattano meglio a specifici task (speech recognition, image recognition, prediction). Valutare se il modello è più adatto per dati numerici o categorici e scegliere di conseguenza.

- scegliere il modello
		passaggio fondamentale, passando dati al modello questo impara a individuare pattern e prendere decisioni.

- evaluation del modello
		dopo il training vedere come il modello performa. Testing delle prestazioni su dataset mai visti dal modello (che è il test set determinato nella fase di preparazione dei dati) . 
		Se il test è fatto sullo stesso dataset su cui si è fatto il training si otterrebbe un'accuratezza sproporzionata rispetto ale effettive capacità del modello.

- parameter tuning
		fatta l'evaluation del modello si può fare il tuning dei parametri per ottimizzare e migliorare le prestazioni.

- making predictions
		usare il modello su dati mai visti precedentemente.



La rete neurale è un network di equazioni matematiche. 
Si hanno in ingresso uno o più variabili di input, le quali, attraversando un network di equazioni, risultano in uno o più variabili di output.
Si può anche dire che le reti neurali accettano in ingresso vettori di input e ritornano vettori di output.

FUNZIONAMENTO

![[Pasted image 20220929101911.png]]
nelle reti neurali si hanno:
- input layer
		 consiste in una o più *feature variables* (variabili di input o variabili indipendeni), come x1 e x2, ..., xn.
		 
- hidden layers (uno o più)
		consiste in uno o più nodi o unità nascoste
		
- output layer
		una o più unità di output

![[Pasted image 20220929102624.png]]
layer con tanti nodi

![[Pasted image 20220929102717.png]]
rete con tanti layers

Generalmente maggiori sono i nodi e i layers, maggiore è la capacità di eseguire calcoli complessi.

ESEMPIO DI UNA POSSIBILE RETE NEURALE

![[Pasted image 20220929103817.png]]
Come variabili di input si hanno: 
	- lot size
	- numero di camere da letto
	- avg. family income
Fornendo alla rete queste tre informazioni essa ritorna in output il prezzo della casa.

![[Pasted image 20220929104651.png]]

L'activation function è un interruttore che da come risultato un valore tra 0 o 1, e riceve in ingresso il valore ottenuto dalla funzione lineare.
Le *input features* (*x*) sono inserite nella funzione lineare dei nodi. Così si ottiene un valore *z*, il quale è inserito nell'*activation function*, che determina se l'interruttore è acceso o spento.
Il threshold imposto dalla funzione di attivazione determina quali nodi della rete sono attivati equali rimangono spenti.
![[Pasted image 20220929110506.png]]

Ci sono diversi tipi di reti neurali:
		- [[Artificial Neural Networks (ANN)]]
		- [[Convolution Neural Networks (CNN)]]
		- [[Recurrent Neural Networks (RNN)]]


APPLICAZIONE RETI NEURALI

- **riconoscimento immagini e video**, come riconoscimento facciale o [[Bixby vision]].
- **recommender systems**, sistema di consiglio acquisti o visione serie tv usato da Amazon e Netflix.
- **riconoscimento audio**, come OK Google o Siri.
- **guida autonoma**, Tesla.
Structured Query Language

Linguaggio di programmazione progettato per gestire i dati salvati in [[database relazionali]]. 





![[Pasted image 20221006125454.png]]
per creare tabella



![[Pasted image 20221006125642.png]]
per accedere ai valori di una tabella



![[Pasted image 20221006130104.png]]
per inserire nuovi valori nella tabella



![[Pasted image 20221006143502.png]]
SELECT consente di acquisire i dati dal database, nello specifico ritorna _name_ dalla tabella _celebs_.

usando * al posto di _name_ sono selezionati tutte le colonne della tabella.



![[Pasted image 20221006143941.png]]
consnete di modificare la tabella corrente, nello specifico aggiungendo una nuova colonna 



![[Pasted image 20221006145530.png]]
comando che consente di modificare il valore nella colonna del _twitter_handle_ della colonna in cui il valore dell'_id_ è pari a 4.



![[Pasted image 20221006150333.png]]
comando che cancella una o più righe dalla tabella indicata che soddisfano la condizione in WHERE



![[Pasted image 20221006150940.png]]
PRIMARY KEY possono essere usate per identificare e avere nella tabella solo righe con un nome univoco.
UNIQUE le colonne devono avere un valore univoco (rispetto a tutte le altre righe)
NOT NULL impone che le colonne debbano tutte evere un valore 
DEFAULT attribuisce un valore di default ad una riga inserita nella tabella se ad essa non è stato attribuito alcun valore inizialmente.



![[Pasted image 20221006162344.png]]
consente di rinominare una tabella o una colonna con un alias. 



![[Pasted image 20221006163753.png]]
DISTINCT permette di ritornare solo i valori non ripetuti.



![[Pasted image 20221006165025.png]]
l'operatore _ consente di cercare una parola incompleta, da tutti i risultati che hanno le lettere prima e dopo.

Altra wildcard è %, se messa dopo (A%) si cercano tutti i risultati che hanno il nome cercato che inizia per A, se è prima (%a) sono forniti tutti i risultati che terminano con a. 
(%a%) tutti i risultati che contengono la lettera a.


 con NULL è necessario usare le espressioni:
 - IS NULL
 - IS NOT NULL

![[Pasted image 20221006170010.png]]



![[Pasted image 20221006170514.png]]
![[Pasted image 20221006170534.png]]
BETWEEN consente di cercare tra due estremi di una serie/array. Il limite superiore deve essere sempre maggiore di uno rispetto a quello che si vuole ottenere nel risultato (SOLO PER LE SERIE DI STRINGHE O LETTERE).



![[Pasted image 20221006171457.png]]
Operatore AND usato anche per colonne diverse.
Possibile farlo anche con OR.



![[Pasted image 20221006172833.png]]
ORDER BY permette di ordinare il risultato della query in base ai valori della colonna indicata. L'ordine può essere decrescente (DESC, dalla Z alla A), o ascendente (ASC da A a Z).



![[Pasted image 20221006173408.png]]
LIMIT consente di limitare il numero di risultati ottenuti dalla query.



![[Pasted image 20221006174047.png]]
CASE fornisce di modificare l'output della query in base ai valori di ogni singolo risultato.



- COUNT(): conta il numero di righe
![[Pasted image 20221007091033.png]]
- SUM(): somma i valori nella colonna
 ![[Pasted image 20221007091207.png]]
- MAX()/MIN(): maggiore e minore valore
 ![[Pasted image 20221007091240.png]]
- AVG(): media del valore nella colonna
 ![[Pasted image 20221007091728.png]]
- ROUND(): arrotonda i valori in colonna
 ![[Pasted image 20221007091933.png]]




![[Pasted image 20221007100324.png]]
invece di fare tante query distinte si può usare GROUP BY 
![[Pasted image 20221007100445.png]]



![[Pasted image 20221007101113.png]]
può essere scritta anche come
![[Pasted image 20221007101315.png]]




![[Pasted image 20221007105622.png]]
permette di limitare/filtrare i risultati ottenuti da una query in cui si usa GROUP BY



**COME ESEGUIRE UN JOIN**
procedura che consente di incrociare le informazioni di tabelle differenti.
![[Pasted image 20221011103206.png]]
unione delle tabelle _orders_ e _customers_ sovrapponendo la colonna _customer_id_.
è posisbile aggiungere anche la condizione WHERE dopo un join.


![[Pasted image 20221011122304.png]]
**INNER JOIN**: quando unisco in una tabella solo le righe (appartenenti alle 2 tabelle considerate) che rispettano la condizione
**LEFT JOIN**: unisco tutte le righe delle due tabelle, ma quelle che non rispettano la condizione rimangono senza valori (LEFT -> senza valori nella parte destra, RIGHT -> senza valori nella parte sinistra).
![[Pasted image 20221011122613.png]]
aggiungendo WHERE posso inserire un ulteriore condizione che filtra la query che effettua il join.


**chiavi primarie/primary key**: colonne che identificano in modo univoco ogni riga della tabella.
proprietà chiavi primarie:
- nessuno dei valori deve essere NULL.
- ogni valore deve essere unico/univoco.
- ogni tabella non può avere più di una colonna di chiavi primarie.

**foreign key**: quando la primary key di una tabella è presente anche in un altra tabella.

![[Pasted image 20221011151552.png]]
**CROSS JOIN**: combina tutte le colonne di una tabella con tutte le colonne di un altra tabella.


![[Pasted image 20221011155434.png]]
UNION serve per unire/incollare due tabelle insieme.



![[Pasted image 20221011160254.png]]
WITH consente di effettuare delle query su cui eseguire delle altre query.


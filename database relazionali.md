Insieme di dati organizzati in tabelle composte da colonne e righe, che hanno relazioni tra le varie tabelle. 
Il software che controlla un database relazionale è chiamato RDBMS (Relational DataBase Management System) e tipicamente consente di gestire e aggiornare i database tramite l'implementazione dello Structured Query Language ([[SQL]]).

La maggior parte delle tabelle ha una colonna **chiave** che contiene un identificativo uninivoco per ogni riga. La colonna contiene **chiavi primarie**. Le colonne in una tabella che fanno riferimento a chiavi primarie di altre tabelle sono chiamate **foreign key**. 

Righe -> records
Colonne -> fields/campi o attributi

Ogni _tabella_ solitamente rappresenta un tipo di entità (user, prodotto, transazione).
Ogni _riga_ rappresenta una particolare istanza di quel tipo di entità.
Ogni _colonna_ nella riga rappresenta un particolare valore associato all'istanza (nome, prezzo, data).

ESEMPIO
 tabelle: _prodotti_, _vendite_

la tabella _prodotti_ ha colonne per nome, modello, unità, costo, prezzo di vendita.
la tabella _vendite_ ha colonne per data di vendita, metodo di pagamento, indirizzo per la consegna.

ogni elemento nella tabella _vendite_ ha una **foreign key** che fa riferimento alla chiave primaria della tabella _prodotto_.

La relazione tra _prodotti_ e vendite è di tipo one-to-many, perchè per ogni prodotto ci possono essere molteplici vendite.

![[Pasted image 20221006122225.png]]


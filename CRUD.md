#best-practices #sql #http #database

----

CRUD è un acronimo che rappresenta le operazioni fondamentali per la gestione dei dati in un sistema:

1. **Create (Creare)**: Si riferisce alla creazione di nuovi dati o risorse all'interno del sistema. Questa operazione coinvolge l'aggiunta di nuove voci al database o la creazione di nuovi oggetti.
    
2. **Read (Leggere)**: Coinvolge il recupero dei dati esistenti dal sistema. È l'operazione di lettura o visualizzazione delle informazioni presenti nel database senza modificarle.
    
3. **Update (Aggiornare)**: Si riferisce alla modifica o all'aggiornamento dei dati esistenti. Questa operazione implica la modifica delle informazioni esistenti nel database o la correzione di dati già presenti.
    
4. **Delete (Eliminare)**: Riguarda la rimozione dei dati o delle risorse dal sistema. Questa operazione comporta l'eliminazione definitiva di voci o oggetti dal database.

## Casi d'uso
##### Databases
L'acronimo CRUD si riferisce alle principali operazioni implementate nei database. Ogni lettera è associata ad un comando standard SQL.

| CRUD   | SQL    |
| ------ | ------ |
| Create | INSERT |
| Read   | SELECT |
| Update | UPDATE |
| Delete | DELETE |
Anche se in questo caso si considerano database relazionali, i principi CRUD sono applicabili anche a **document db**, **object db**, **XML db**, file di testo e binari.
Alcumi sistemi big data non implementano il comando UPDATE, ma solo un INSERT a cui è associato un time stamp (che salva una versione aggionata dell'oggetto ogni volta).
##### RESTful APIs
Ogni comando dello standard CRUD può essere associato ad un [[Metodi HTTP|metodo dell'HTTP]] (Hypertext Tranfer Protocol).

| CRUD   | HTTP                                         |
| ------ | -------------------------------------------- |
| Create | POST, PUT                                    |
| Read   | GET                                          |
| Update | PUT (per sostituire), PATCH (per modificare) |
| Delete | DELETE                                       |

**POST** usato quando non devono essere inviati id o uuid.

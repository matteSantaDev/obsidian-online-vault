---

---
---

#### Struttura usata per dati inviati in _Can Configuration_
Sono inviati solo gli elementi/campi modificati.
```
{  
	active: {  
		<numero alias>: { //linea != can_line  
			alias: "line1",  
			baud_rate: 125000,  
			can_line: 1,  
			drive: [  
				{  
				alias: "Drive3",  
				can_line: "lCGbSTC", //se la can line a cui è associata è stata aggiunta sarà 'new'  
				controller_id: "0x48",  
				id: "new" //se il drive è stato aggiunto sarà 'new'  
				}  
			],  
			id: "lCGbSTC" //se è stato aggiunto è 'new'  
		}  
	}  
	delete: {  
		can_line: [] //inseriti id delle can line eliminate  
		drive: [] //inseriti id dei drive eliminati  
	}  
}
```


#### Struttura usata per dati inviati in _Can Topics Configuration_

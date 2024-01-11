
#AdelSystem 
____
`14/12/23`
- [x] mettere in uscita (batteria) una tensione custom e verificare corrispondenza con oscilloscopio
- [x] capire perché la tensione letta internamente è differente di quella effettivamente data (24V)
     Il coefficiente di calibrazione `V_SCALE_FACTOR` è sbagliato, risolto deducendo il coefficiente come il rapporto tra il valore della tensione nominale e il valore della tensione row.
     - [x] test per vedere se il coefficiente è giusto, alimento la scheda a 12V e vedo cosa leggo in `Vin`.
     Leggo la tensione corretta quindi il coefficiente calcolato empiricamente è abbastanza giusto.
     - [ ] partitore resistivo
     - [ ] zero corrisponde a 0 e 4096 
```C
GetVin_mV() = 13898;
V_SCALE_FACTOR = 15.2;
Vin = 14641;

Vin = (GetVin_mV() * V_SCALE_FACTOR)

0.00172911


3.3/68+3.3   *12
3.3/ 71.3 = 0.046  3.3/0.046
4096 71.74
12.3/3.3 *71.73 = 
x*3.3  /71.3    *4096      0.56v/3.3 = 0.17*4096 = 24*
```

- [x] capire perché la tensione in uscita verso la batteria `Vout` è sbagliata
- [x] capire funzionamento batterie in relazione alla variazione di tensione e corrente. Determinare le condizioni che indicano che una batteria è carica (tensione costante fino a che la corrente non cala?).

_____
`18/12/2023`

- [x] ordino casi d'uso, li integro e li metto in tabella
- [ ] inserire legenda dei nomi usati all'inizio del documento
- [ ] implementazione primo test virtuale (meccanismo di testing)
- [ ] analisi algoritmo carica batterie, andamento tipico della tensione e della corrente (con valori nominali)

- [ ] implementazione algoritmo di carica batteria al piombo

- [ ] implementazione caso uso base in cui il micro entra in modalità autonoma

-----
### interfaccia CBI rete ethernet
192.168.1.100
user -> admin
pw -> password

Elenco dei filtri che permettono di filtrare gli input dell'utente nella pagina *CAN CONFIGURATION*. Modifiche in **can_configuration_script.ejs**.

---
## Controllo sul valore di _Can Line_ in Can Line
###### Aspetti controllati
Controllo che:
- non sia selezionato lo stesso can bus nelle 2 can line.

###### Output
Se nel dropdown di una can line viene selezionato un can bus già usato dall'altra can line la selezione del valore è annullata, lasciando valore nullo. Inoltre il form ha controno rosso e simbolo che indica l'errore.

###### Funzioni in cui sono suddivisi i controlli
```
$(`#select-can-${k}`).on('change', () => {
	...
}
```
==Quando si modifica il valore del can bus==, nel gestore di eventi viene controllato che il can bus selezionato sia differente di quello utilizzato nell'altra can line, se esiste.

```
$(`#bus-number`).on('change', () => {
	...
}
```
Nel gestore di eventi sopra viene fatto lo stesso controllo sulle can line, ma ==quando viene aggiunta una can line==.

## Controllo sul valore di _Baud rate_ in Can Line
###### Aspetti controllati
Controllo che:
- il valore del baud rate non sia nullo.

###### Output
Se il valore è nullo il form ha contorno rosso e simbolo di errore. Se il valore non è nullo il form ha colore standard.

###### Funzioni in cui sono suddivisi i controlli
```
$(`#input-can-${k}`).on('change', () => {
	...
}
```
Nel gestore di eventi, ==quando si modifica il valore del baud rate==, si controlla che quest'ultimo non sia nullo.

```
$(`#drive-number, #bus-number`).on('change', () => {
	...
}
```
Nel gestore di eventi viene controllato che il valore del baud rate non sia nullo ==al variare del numero di drives==.


___

## Controllo sul valore di _Can Line_ nel drive
###### Aspetti controllati
Controllo che:
- il valore della can line selezionata dal dropdown sia un can bus attivo, ovvero utilizzato in una Can Line.
- controllo che quando viene modificato il can bus di una can line non ci siano drive con quel bus
###### Output
Se la can line selezionata non è attiva il form ha contorno rosso, e sotto ad esso c'è un messaggio che spiega l'errore. Se la can line selezionata è attiva il form ha colore standard.

###### Funzioni in cui sono suddivisi i controlli
La funzione _validateDriveCanLine(drive)_ fa si che ==ogni volta che si modifica il valore di una can line di un drive==, si controlli se è attiva. Ciò significa vedere se il can bus selezionato è utilizzato anche dalle can line.
```
$(`#select-can-1`).on('change', () => {
	...
}
```
Nel gestore di eventi viene controllato che, ==quando è modificato il can bus della prima canline==, non ci siano drive che sono rimasti associati ad una can line non attiva.
```
$(`#bus-number`).on('change', () => {
	...
}
```
Con questo gestore di eventi invece si tolgono i segnali di errore ==quando viene aggiunta la seconda can line==, perché in questo caso è impossibile che ci siano drive associati a can line inattive. Infatti l'evento scatenante è la modifica al numero di can line.
```
$(`#select-can-2`).on('change', () => {
	...
}
```
==Quando seleziono il can bus della seconda can line==, tolgo il segnale di errore alle can line dei drive con stesso can bus.

```
$(`#drive-number, #bus-number`).on('change', () => {
	...
}
```
Nel gestore di eventi sopra viene controllato, ==all'inserimento di un nuovo drive==, che il can bus del drive sia attivo. Tale controllo è eseguito anche ==quando viene modificato il numero di can line==.



## Controllo sul valore di _Controller Id_ nel drive
###### Aspetti controllati
Controllo che:
- il valore non sia nullo
- il valore inserito rispetti il formato esadecimale. A questo scopo è stata scritta la funzione _isHexadecimal()_.
- i drive impostati sulla stessa can line non abbiano lo stesso _controller id_.
###### Output
Se tali condizioni sono rispettate non viene mostrato nulla, se invece non sono rispettate il form ha contorno e simbolo rosso.
###### Funzioni in cui sono suddivisi i controlli
Nella funzione _validateCIdOnInput(field)_ controllo che, ==all'inserimento del valore nel form==, quest'ultimo non sia nullo o non rispetti il formato esadecimale.
Il controllo sul valore nullo è effettuato anche quando sono inseriti nuovi drive, quindi al modificarsi del numero di drive. In questa eventualità non è eseguito il controllo sul formato esadecimale.
```
$(`#drive-number, #bus-number`).on('change', () => {
	...
}
```


Nella funzione _validateCIdOnCanLineChange(field)_ viene controllato che, per ogni driver, ==al modificarsi del _controller id_==, il driver non abbia un controller id duplicato, tra drive associati alla stessa can line.
Questo controllo è fatto anche in _validateAdressInput(field)_, ma ==al caricamento della pagina==.

###### Ridondanze
_ValidateCanLineSelect(field)_ sembra fare il controllo del valore dei controller id ==al modificarsi del can bus==. Vincolo che controlla se drive con stesso bus hanno id ridondanti, fatto in modo contro intuitivo, eliminabile.

## Controllo sul valore di _Alias_ nel drive
###### Aspetti controllati
Controllo che:
- gli alias non siano ridondanti.

###### Output
Se un alias ha un valore già esistente viene contorniato il form di rosso e aggiunto un icona. Se l'alias è valido il form ha colore standard.

###### Funzioni in cui sono suddivisi i controlli
La funzione _validateDriveAliases(drive)_, ==quando viene modificato il valore dell'alias==, controlla che tale valore non sia già usato negli alias dei drive esistenti.
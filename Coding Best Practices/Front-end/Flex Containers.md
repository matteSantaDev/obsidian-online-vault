#css #JavaScript

Una Flexible Box (Flexbox Layout) ha come obiettivo fornire un modo più efficiente di ordinare e allineare/distribuire lo spazio tra elementi in un container, anche quando la loro dimensione è ignota.

L'idea principale dietro al flex layout è fornire al container l'abilità di alterare altezza e larghezza, in modo di adattarsi al meglio agli spazi. Un container flex espande gli elementi per riempire lo spazio disponibile o li stringe per prevenire la sovrapposizione.

Il flexbox è **direction agnostic**.

Flexbox layout --> componenti di un applicazione, layout di piccole dimensioni
Grid layout       --> layout su scale maggiori 


![[Pasted image 20231026100958.png]]

Gli elementi verranno disposti seguendo sia l'asse principale (da inizio a fine) che l'asse trasversale (da inizio trasversale a fine trasversale).

- **main axis** - Asse principale: L'asse principale di un contenitore flessibile è l'asse primario lungo il quale gli elementi flessibili vengono disposti. Attenzione, non è necessariamente orizzontale; dipende dalla proprietà di direzione flessibile (vedi sotto).
- **main-start | main-end** - Inizio-asse principale | Fine-asse principale: Gli elementi flessibili vengono posizionati all'interno del contenitore partendo dall'inizio-asse principale e procedendo verso la fine-asse principale.
- **main size** - Dimensione principale: La larghezza o l'altezza di un elemento flessibile, a seconda della dimensione principale, costituisce la dimensione principale dell'elemento. La proprietà di dimensione principale dell'elemento flessibile è la proprietà 'larghezza' o 'altezza', a seconda della dimensione principale.
- **cross axis** - Asse trasversale: L'asse perpendicolare all'asse principale è chiamato asse trasversale. La sua direzione dipende dalla direzione dell'asse principale.
- **cross-start | cross-end** - Inizio-asse trasversale | Fine-asse trasversale: Le righe flessibili vengono riempite con elementi e posizionate all'interno del contenitore a partire dal lato iniziale-asse trasversale del contenitore flessibile e procedendo verso il lato fine-asse trasversale.
- **cross size** - Dimensione trasversale: La larghezza o l'altezza di un elemento flessibile, a seconda della dimensione trasversale, costituisce la dimensione trasversale dell'elemento. La proprietà di dimensione trasversale è 'larghezza' o 'altezza', a seconda della dimensione trasversale.

### Proprietà flexbox Parent (flex container)
![[Pasted image 20231026101451.png]]


##### display
Definisce un container flex. Inline o block in base al valore che si da. Abilità il contesto per tutti i suoi children.
```css
.container {
  display: flex; /* or inline-flex */
}
```
##### flex direction
 Stabilisce l'asse principale. Flexbox è, eccetto i casi in cui si fa un optional wrapping, un _single-direction layout concept_. Quindi configurazioni standard della flexbox sono o riga orizzontale o colonna verticale.
```css
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```
	![[Pasted image 20231026102435.png]]
	- `row` (default): da sinistra a destra in ltr; da destra a sinistra in rtl
	- `row-reverse`: da destra a sinistra in ltr; da sinistra a destra in rtl
	- `column`: come la riga ma dall'alto verso il basso
	- `column-reverse`: come la riga-inversa ma dal basso verso l'alto
##### flex-wrap
 Per andare a capo se si è in configurazione di riga orizzontale.
```css
.container {
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
	![[Pasted image 20231026102729.png]]
##### flex-flow
 Versione più breve rispetto a `flex-direction` e `flex-wrap`, che insieme definiscono l'asse main e l'asse cross.
```css
.container {
  flex-flow: column wrap;
}
```

##### justify-content
![[Pasted image 20231026103253.png]]
 Definisce l'allineamento sull'asse main e getsisce extra spazio nella linea.
```css
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly | start | end | left | right ... + safe | unsafe;
}
```
 Si possono aggiungere le keyword `safe` e `unsafe` per evitare di creare un elemento che va fuori lo schermo e non può essere scrollato.
 Non tutte le opzioni funzionano in tutti i browser.

##### align-items
![[Pasted image 20231026103651.png]]
 Come gli elementi sono ordinati sul cross axis se la configurazione generale è linea orizzontale.
```css
.container {
  align-items: stretch | flex-start | flex-end | center | baseline | first baseline | last baseline | start | end | self-start | self-end + ... safe | unsafe;
}
```
 Anche qui possono essere usati `safe` `unsafe` in congiunzione con le keyword.

##### align-content
![[Pasted image 20231026103900.png]]
 Gestione dello spazio nel raggruppamento degli elementi sull asse cross axis.
```css
.container {
  align-content: flex-start | flex-end | center | space-between | space-around | space-evenly | stretch | start | end | baseline | first baseline | last baseline + ... safe | unsafe;
}
```
 Anche qui possono essere usati `safe` `unsafe` in congiunzione con le keyword.


##### gap, row-gap, column-gap
![[Pasted image 20231026104215.png]]
 La proprietà gap controlla esplicitamente lo spazio tra gli item flex. Solo tra gli item e non la distanza dai bordi esterni.
```css
.container {
  display: flex;
  ...
  gap: 10px;
  gap: 10px 20px; /* row-gap column gap */
  row-gap: 10px;
  column-gap: 20px;
}
```





### Proprietà flexbox Children (flex item)
![[Pasted image 20231026101530.png]]
##### order
![[Pasted image 20231026104430.png]]
 Di default gli elementi sono ordinati in base all'ordine in cui sono nella struttura dati. Questa proprietà controlla l'ordine in cui sono inseriti gli item nella flexbox.
```css
.item {
  order: 5; /* default is 0 */
}
```

##### flex-grow
![[Pasted image 20231026104737.png]]
 Definisce l'abilità degli items di crescere e allargarsi. Indica la dimensione che l'item deve occupare nel flex container. Definito lo spazio per un elemento, il restante spazio è ridistribuito tra gli altri items.
```css
.item {
  flex-grow: 4; /* default 0 */
}
```
 Non sono validi valori negativi.

##### flex-shrink
 Definisce la capacità dell'item di comprimersi se necessario.
```css
.item {
  flex-shrink: 3; /* default 1 */
}
```


##### flex-basis
 Definisce la dimensione dell'item quando ancora lo spazio non è stato ancora tutto distribuito. Può essere una lunghezza (20%, 50%) o una keyword.
 `auto`: dimensine determinata dalle proprietà di altezza e larghezza definite da **main-size**.
 `content`: dimensione basata sulla dimensione dell'item.
```css
.item {
  flex-basis:  | auto; /* default auto */
}
```
 Se impostato su 0, lo spazio aggiuntivo intorno al contenuto non viene considerato.

##### flex
 Questo è il comando abbreviato per **flex-grow**, **flex-shrink** e **flex-basis** combinati. I secondi e terzi parametri (`flex-shrink` e `flex-basis`) sono opzionali. 
 Il valore predefinito è `0`, `1`, `auto`, ma se lo imposti con un singolo valore numerico, come `flex: 5;`, ciò modifica il flex-basis a 0%, quindi è come impostare 
```css
flex-grow: 5;
flex-shrink: 1;
flex-basis: 0%;
```

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
 Meglio usare questa versione accorciata rispetto alle singole proprietà.


##### align-self
![[Pasted image 20231026105934.png]]
 Comando che permette all'impostazione di allineamento degli items di essere sovrascritto con un comando singolo dedicato allo specifico item.
```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```



---



### References
https://css-tricks.com/snippets/css/a-guide-to-flexbox/
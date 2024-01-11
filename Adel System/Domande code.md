- [x] in `sol_manage_app.c` in che modo si può gestire l'alimentazione al pannello, con quale funzione o modificando quale parametro.
- perché durante il debug la tensione in input e in output non sono nulle (in  `sol_manage_app.c`)
![[Pasted image 20231213113906.png]]
- [x]  `Vout` e  `Iout` sono i parametri con i quali controllo il pannello fotovoltaico?
Per settare il voltaggio e la corrente in output verso il pannello viene usato il comando `HAL_DAC_SetValue`, funzione definita in `stm32f0xx_hal_dac.c`. Questa scrive il registro con la tensione.

-  il codice attuale cosa fa?
	-  alimentazione
	-  inizializzazione
	-  alimentazione batteria, tenerla costante e vedere l'output (tensione) sui pin bat
	-  pannello fotovoltaico non gestito

- [ ] come alimentare batteria
	- [ ] valore di tensione in input del pannello `Vin = GetPanelVoltage_mV();` ha valore 14641, dovrebbe essere 24000V.

----


attivare battery enable
2579 

sol_manager

usare anche il misuratore di corrente assorbita
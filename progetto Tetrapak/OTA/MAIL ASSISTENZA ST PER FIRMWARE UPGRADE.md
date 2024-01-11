#tetrapak #OTA

---

- come fare firmware update via scheda specifica nostra
- quali sono le funzioni che permettono di invocare l'OTA loader e scrivere il nuovo firmware
- è già presente un ota loader nel progetto hs datalog.
- come utilizzare le funzioni di ota firmware update già presenti nel progetto
- esistono esempi e documentazione per firmware update OTA per la scheda steval stwin

Buongiorno,
vorremmo sapere come eseguire il firmware update OTA  sulla scheda STEVAL-STWINKT1B. 
Stiamo infatti sviluppando un progetto sulla base dell'esempio High Speed Datalog per STEVAL-STWINKT1B, in cui è necessario che la scheda esegua il firmware upgrade in seguito ad un comando fornito da un client che comunica via BLE. Siamo in grado di trasferire il file binario del nuovo firmware compilato dal client alla board e salvarlo sulla sd. Per caricare il nuovo firmware abbiamo scelto di usare le funzioni che erano presenti nel file OTA.c, presente nel progetto High Speed Datalog.
Nello specifico utilizziamo la funzione **_BLE_OTA_upgrade_chunk()_**, a cui forniamo il binario del firmware. Otteniamo sempre però i seguenti errori.

![[errori_ota_firm_update.png]]

Più in generale gli interrogativi che ci poniamo sono:
- esistono esempi o documentazione che mostrino come eseguire il firmware upgrade (lato board) della scheda STEVAL-STWINKT1B, (o per schede con medesimo cpu).
- Come possono essere usate le funzioni presenti nel file OTA.c presenti nel progetto HSDatalog per la board STEVAL-STWINKT1B al fine di eseguire il firmware update?
- Guardando il video relativo all'OTA firmware update per la scheda P-NUCLEO_WB55 viene citato l' OTA Loader App, necessario alla scrittura del firmware sulla flash durante l'update. L' OTA Loader App è già presente nell'esempio HSDatalog per la scheda STEVAL-STWINKT1B? Nel caso negativo come si può integrare nel progetto e quali sono le funzioni da utilizzare per eseguire la scrittura del nuovo firmware sulla memoria FLASH?
![[Pasted image 20230310142148.png]]
- FLASH, RAM e CCMRAM sono utilizzate solo dal firmware o anche dal modulo OTA e dal bootloader?


- Quali funzioni/file devono essere prese dall'esempio FP_IND_PREDMNT1 per poter implementare un firmware update OTA funzionante sul nostro progetto.
- Cosa deve essere fatto per avere il **boot loader** e il **modulo di che gestisce l'OTA** nel nostro progetto?
- Come deve essere poi configurato il file Linker?
- Dove posso trovare il batch file.



---


### FUNZIONI USATE
- **BLE_OTA_upgrade_chunk()**        in _ble_config_service_                         (1474)
- **UpdateFWBlueMS()**                   in _ble_config_service_                         (1055)
- **HAL_FLASH_Program()**              in _OTA_                                              (87)
- **stm32I4xx_hal_flash()**                in _FLASH_WaitForLastOperation_      (181)
- error = (FLASH -> SR & FL...      in ==                                                (666) 



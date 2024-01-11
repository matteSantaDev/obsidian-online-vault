
Premessa: abbiamo tentato di utilizzare una funzione di *firmware update* già presente nel progetto originario (presente nell'esempio HSDatalog da cui siamo partiti). Nella sua versione originale la funzione riceve pezzi del file binario, li salva in un buffer ed esegue il firmware update.
Per semplicità abbiamo modificato la funzione in modo che potesse accettare il file binario (letto dalla scheda sd) in un unica volta. Non riuscendo a far funzionare correttamente il firmware update, anche partizionando il file in più chunk, è stato tentato di considerare il modulo di firmware update come una blackbox. In tale modo sarebbe necessario conoscere solo gli input forniti dalla webapp per poter far iniziare il firmware update.



- non ci è stata fornita da st la documentazione su come funziona la webapp che consente di eseguire il firmware update in modalità OTA. Più nello specifico non c'è documentazione riguardo al modo (partizionamento del file e meccanismi di sicurezza) con sia scritto il file binario sulla caratteristica adibita dalla board a ricevere il firmware. Ci è stato indicato di visionare il repo pubblico della webapp presente su github. L'analisi non semplicissima di reverse engeenering non è stata fatta per motivi di tempo.

	I file alla repo della webapp:

  
Per ANDROID:
	[https://github.com/STMicroelectronics/STBlueMS_Android/tree/FullAppSnapshot/BlueSTSDK_GUI/src/main/java/com/st/BlueSTSDK/gui/fwUpgrade/fwUpgradeConsole](https://github.com/STMicroelectronics/STBlueMS_Android/tree/FullAppSnapshot/BlueSTSDK_GUI/src/main/java/com/st/BlueSTSDK/gui/fwUpgrade/fwUpgradeConsole)

**Per iOS:**

[https://github.com/STMicroelectronics/STBLESensor_iOS/tree/master/STLibs/BlueSTSDK_Gui/BlueSTSDK_Gui/Classes/Core/FwUpgrade/FwUpgradeConsole](https://github.com/STMicroelectronics/STBLESensor_iOS/tree/master/STLibs/BlueSTSDK_Gui/BlueSTSDK_Gui/Classes/Core/FwUpgrade/FwUpgradeConsole)

- la possibilità di eseguire il firmware update OTA direttamente dall'applicazione di ST viene impedito dal fatto che i progetti a cui viene modificato il nome non sono visibili dalla webapp.
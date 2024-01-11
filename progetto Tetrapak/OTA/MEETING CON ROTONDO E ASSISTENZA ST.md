13/12/2022
#tetrapak 

---

control_end_point manda dati in due direzioni.

6 endpoint, 1 endpoint di controllo per parametri e impostazioni

aggiungere nuovo sensore come oggetto.
DLL -> parte di codice per modificare dati di output verso pc

## inizializzazione, config endpoint:
end point -> messaggi json
end point (5) -> usati per dati dei sensori (1234 veloci) (5 lento multiplexato, letti nel quadro c)

USBD_wcid_streaming_fILLtXdATAbUFFER()

che cazzo è un endpoint

https://github.com/pbatard/libwdi/wiki/WCID-Devices
endpoint L4 (6+1) a seconda del micro della scheda varia il numero di endpoint allocabili, se si aggiunge una nuova classe si devono allocare 2 o 3 endpoint dai (6+1). 

SFRUTTARE END POINT DI CONTROLLO

punto di blocco è DLL

---

problema di partire del progetto da 0
non trova file

definire configurazione di progetto

---

guardare la init dell esempio di datalog lento
inizializzazioni tutti nel main

problema legato alla configuazione

---

I^2c per accedere ai registri ad accelerometro e giroscopio
boh problemi di nicola, prendere pacchetto software più generico possibile X-CUBE-MEMS1 (https://www.st.com/en/embedded-software/x-cube-mems1.html)

---


IN ATTESA DI
- [ ] ppt endpoint
- [ ] codice aperto per modificare comunicazione via usb

FARE 
- [ ] cercare brano di codice in cui viene fatta la scrittura dei file.dat sul pc via usb, con wcid





---
progetto acquisizione microfono: https://www.st.com/en/embedded-software/fp-aud-smartmic1.html#documentation

quick start progetto acquisizione microfono: file:///C:/Users/arscontrol/Downloads/en.fp-aud-smartmic1_quick_start_guide%20(1).pdf

middleware progetto microfono: file:///C:/Users/arscontrol/Downloads/um2214-getting-started-with-acousticbf-realtime-beam-forming-middleware-stmicroelectronics%20(2).pdf

progetto su github: https://github.com/STMicroelectronics/fp-aud-smartmic1


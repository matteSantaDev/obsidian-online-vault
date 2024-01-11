#informatica

(anche chiamato sistema di elaborazione dati) è un [[sistema meccanografico]], un computer o un insieme di più computer, apparati o sottoinsiemi elettronici (come server, database, mainframe, supercomputer, switch, router, modem, terminali) tra loro interconessi in rete, in un architettura di base di tipo client-server, e preposti a una o più funzionalità o a servizi di elaborazione a favore degli utenti.

Chiamato anche infrastruttura IT di un'azienda, cioè il complesso delle risorse informatiche a livello hardware e software di un'azienda.

Sistema composto da hardware e software, tipicamente a uso [[intranet]], eventualmente connesso ad internet, centralizzato o distribuito e interconnesso tramite [[VPN]] con un architettura che varia a seconda delle esigenze e della sua progettazione e, attraverso opportune applicazioni (app web) elabora dati e informazioni per restituire altri dati e info utili.

esempi: pc, sistemi informatici aziendali per servizi di business e logistici
			 il [[sistema informativo]] è invece l'insieme delle attività logiche di gestione delle info, delle relative modalità e degli strumenti tecnologici usati a tale scopo, di cui il sistema informatico ne costituisce la parte infrastrutturale.

PROPRIETA
- **efficienza**: capactà del sist. software di usare il minor numeor di risorse informatiche possibile durante la sua esecuzione (tempo di utilizzo della CPU, spazio occupato dal programma e dai dati in memoria).
- **affidabilità**: capacità di rispettare le specifiche tecniche di funzionamento nel tempo. Può anche essere definita come la probabilità che l'insisme o il singolo componente non si guasti in un determinato lasso di tempo.
- **disponibilità**: attitudine di un entità o un sistema ad essere in grado di svolgere una funzione in determinate condizioni ad un dato istante o durante un certo lasso di tempo (es. fornire serevizio) supponendo che siano assicurati i mezzi esterni necessari. 
									(tempo effettivo di funzionamento)/(tempo totale)
									
- **sicurezza/security**: insieme dei mezz, delle tecnologie e delle procedure atte alla protezione dei sistemi informatici in termini di disponibilità, confidenzialità e integrità dei beni o asset informatici.
- **scalabilità**: capacità di un sistema di aumentare o diminuire di scala in funzione delle necessità e edisponibilità.



IL sistema informatico necessita di protocolli (set di regole e vincoli) che descrivono le condizioni necessarie alla comunicazione con entità esterne. Quando un sistema rispetta le regole di uno standard (documento che specifica quali sono le regole da rispettare) il sistema è _conforme_ o _compliant_.



FUNZIONALITA

_logica di presentazione (presentation layer)_: front-end, ha il compito di presentare i dati all'utente ed inviare le rischieste di questi verso la parte centrale-elaborativa del sistema, costituendo un interfaccia uomo-macchina.
_logica applicativa/di business (application layer)_: fornisce gli applicativi agli utenti, tipicamente sotto forma di un application server o web server, per poter usufruire dei servizi offerti dal sistema informativo.
_logica di accesso ai dati (access data layer)_: back-end, server dati che interrogano il database o il system legacy (sistema informatico obsoleto ma insostituibile).




ARCHITETTURE POSSIBILI

- **architettura 1-tier**: le 3 funzionalità sono ospitate su una macchina come sistema centralizzato tipo mainframe (computer centralizzato).
- **architettura 2-tier**: le 3 funzionalità sono ospitate su 2 tipi di macchine. Quella di presentazione sui client e le altre su application e database server.
- **architettura 3-tier**: le tre funzionalità sono implementate ognuna su una macchina o sistema di macchine indipendenti.
- **architettura n-tier**: funzionalità implementate su più di  tre livelli, con architetture motlo più distribuite.
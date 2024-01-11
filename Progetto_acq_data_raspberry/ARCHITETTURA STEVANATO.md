script bash

WHILE (condizione == true)
	-> esecuzione script digiducer per acquisire i dati dell'accelerometro/sensore di vibrazione.
	-> esecuzione script sensore distanza.
	-> invio file via mqtt (mediante esecuzione script di python)


##### PROBLEMA SCRIPT DIGIDUCER
nello script che acquisisce dati dal digiducer è visibile solo un dispositivo usb, anche quando sono collegati entrambi i sensori (accelerometro e sensore di vibrazione). L'output dello script infatti è sempre un unico file .wav.

con lsusb si ottiene solo:
		Bus 001 Device 005: ID 29da:000a Digiducer.com  485B39 200159807943590799265210723

con ls -l /sys/bus/usb/devices/
		lrwxrwxrwx 1 root root 0 Apr  7 18:04 1-1.4.3 -> ../../../devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.4/1-1.4.3  
         lrwxrwxrwx 1 root root 0 Apr  7 18:04 1-1.4.3:1.0 -> ../../../devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.4/1-1.4.3/1-1.4.3:1.0  
         lrwxrwxrwx 1 root root 0 Apr  7 18:04 1-1.4.3:1.1 -> ../../../devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.4/1-1.4.3/1-1.4.3:1.1


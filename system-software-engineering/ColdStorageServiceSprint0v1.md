## Introduzione

In questo sprint vengono definiti i dettagli e chiarimenti dei requisiti del sistema richiesto. 
Viene inoltre proposto un piano di lavoro suddiviso in molteplici sprint.

## Requirements

I requisiti sono scritti dal committente al seguente in TemaFinale23.

## Requirements analysis

Dopo opportuni colloqui con il committente, possiano affermare che :
- KeyPoint1:  le operazioni di carico e di scarico della ColdRoom potrebbero essere effettuate in parallelo oppure in maniera sequenziale. Per semplicità di realizzazione, dato che il committente non ha espresso riflessioni in materia, vengono effettuate in maniera sequenziale, ma nel caso realistico esse verrebbero fatte in parallelo.
- nel caso non ci sia spazio nella ColdRoom, il ticket non verrà emesso. Dunque quando il ticket viene inviato a un cliente, allora questa è la garanzia che il servizio verrà portato a termine.
- Non deve succedere che un camion inserisca il proprio ticket nell'applicazione, ma che poi quando arriva in INDOOR rimanga in attesa.
- E' possibile che il transport trolley non riesca a scaricare un intero truck tutto in un solo viaggio
- L'applicazione ColdStorageService deve poter prendere in considerazione richieste di generazione di ticket anche quando il transport trolley sta gestendo altre operazioni di scarico. Dunque, la generazione dei ticket non dipende dallo stato del transport trolley. 
- Dato che l'applicazione ColdStorageService deve poter gestire richieste di ticket anche quando delle deposit action non sono terminate, ci sono due stati della ColdRoom:
1) Stato Reale: la quantità effettiva di kg di cibo all'interno della ColdRoom
2) Stato Ghost: la quantità prevista di kg di cibo all'interno della ColdRoom dopo che vengono effettuate tutte le deposit action dei ticket emessi
- La ServiceAcessGUI deve mostrare lo stato della ColdRoom considerando anche le Volendlo se no, il committente può avere una gui con lo stato del frigo, cowsì se il frigo è pieno, allora non gli conviene fare la richiesta.
- Il ticket ha al suo interno un "ticket number", e per ragioni di sicurezza due o più Fridge truck driver non devono poter conoscere i ticket degli altri, e non devono poter inserire i ticket degli altri né in maniera malevola né incidentale 


Dai requisiti possiamo asserire che
- si tratta di realizzare il software per un **sistema distribuito** costituito da più nodi di elaborazione;
- i nodi di elaborazione devono potersi scambiare informazione via rete;
- Le macroentità sono:
	- ColdStorageService
	- Transport Trolley (fisico o virtuale)
	- ServiceAcessGUI 
![[macrocomponents 1.png]]
## Problem Analysis

## Architettura logica
Il sistema è composto da:
  - ColdStorageService: prende in carico richieste di generazione ticket e richieste di invio di un camion; si interfaccia con la ColdRoom per saperne lo stato; fa partire le deposit action;
  - Transport Trolley: invia al Basic Robot la sequenza di comandi necessari per effettuare una deposit action
  - Basic Robot: esegue i comandi del Transport Trolley
  - Sonar-Led Controller: invia i comandi al led in base allo stato del robot; è Observer del Sonar di modo da rilevare la misura della distanza.
  - Sonar: effettua la misurazione e emette eventi
  - ServiceAccessGui: si interfaccia con ColdStorageService per la richiesa di ticket
  - Cold Room: aggiorna lo stato della quantità di kg
  - Scarico: entità esterna che effettua una operazione di scarico della Cold Room, diminuendo i kg presenti in essa
![[appl1qakarch 1.png]]

## Piano di lavoro
Il lavoro verrà suddiviso in sprint. In particolare:
Sprint1a) prototipazione del sistema senza l'alarm requirements
Sprint1b) creazione di una infrastruttura per facilitare il testing
Sprint1c) test dello Sprint1a
Sprint2a)  prototipazione del sistema con anche l'alarm requirements
Sprint2b)  test dello Sprint2a
Sprint3a) prototipazione del sistema con la GUI
Sprint3b) test dello Sprint3a
Sprint4) deployment su robot fisico


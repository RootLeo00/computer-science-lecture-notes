# LABORATORIO DI INGEGNERIA DEI SISTEMI SOFTWARE

## Requisiti
-   fare in modo che il robot si fermi per un tempo prefissato ( `2sec` ) ogni volta che esso viene rilevato da uno dei Sonar cablati nelle pareti della stanza;
-   prefigurare i tempi previsti per lo sviluppo e 
- i tempi effettivi necessari per il completamento del sistema;
-   indicare il modo con cui si ritiene opportuno distribuire il prodotto finale.

## Analisi dei requisiti
- per *Sonar* si intende un dispositivo che rileva la presenza del robot e ne misura la distanza, come descritto in ["La scena di WEnv"](file:///home/leo/github/sw-eng/issLab23/iss23Material/html//VirtualRobot23.html#la-scena-di-wenv)
- il sonar emetterà dei messaggi asincroni non bloccanti, perchè non necessita di alcuna risposta o conferma di ricezione

### Architettura logica
Nello sprint2 è stata realizzata la seguente architettura logica:
![[Pasted image 20230321152403.png]]
l'architettura logica è composta da tre entità principali:
1) CmdConsole, la command console, utilizzata dall'utente finale per mandare comandi a Appl1 che poi gestirà con il robot
2) Appl1, applicazione principale che riceve le richieste dalle command console, le interpreta e manda i comandi opportuni al virtual robot
3) VirtualRobot23, applicazione di virtualizzazione del robot

### Analisi problema
- I messaggi del sonar vengono inviati direttamente dal virtual robot ad Appl1
- per la realizzazione del sistema distribuito, sfrutterei la libreria [unibo.basicomm23.actors23](file:///home/leo/github/sw-eng/issLab23/iss23Material/html/Actors23.html#unibo-basicomm23-actors23-actorbasic23). In tal caso, posso descrivere dei contesti, cioè dei nodi della rete, che contengono più attori, che nel nostro caso saranno più entità Appl1 o virtual robot. Si veda [descrizione di un sistema distribuito](file:///home/leo/github/sw-eng/issLab23/iss23Material/html/Actors23.html#descrizione-di-un-sistema-distribuito)

Refactoring della architettura logica utilizzando gli attori:
![[Pasted image 20230424135913.png]]

## Test plans
Dall’analisi emerge l’opportunità di impostare il seguente piano di lavoro:
1) realizzazione del sistema distribuito:
	- due contesti
	- nel primo contesto inserisco 1 attore, nell'altro invece 2 attori
		- connetto una command console al singolo attore del primo contesto, mentre collego due console per ogni attore nel secondo contesto
![[Pasted image 20230424140021.png]]

### tempi e costi di testing
- per realizzare quanto descritto nel piano di testing vanno impiegate al massimo 4 ore

## Project

## Testing

## Deployment
Per ogni contesto realizzerò un'immagine docker, appositamente configurata.

## Maintenance

![[Pasted image 20230328152258.png]]
# LABORATORIO DI INGEGNERIA DEI SISTEMI SOFTWARE
## Introduction
Esempio di simulatore di ping-pong

## Requisiti
A partire da ciò che è descritto in issLab23/iss23Material/html/unibo.basicomm23.html con l'applicazione parzialmente implementata (ProducerConsumer), realizzare un simulatore di ping-pong. La comunicazione è realizzata con protocollo udp.

## Analisi dei requisiti
- per *ping-pong* si intende un comportamento di due entità in cui la prima manda un messaggio di *ping* alla seconda, che poi, dopo aver ricevuto il messaggio, manderà un altro messaggio di *pong* alla prima.
- In particolare, per *ping* si intende un messaggio, il cui contenuto è la stringa "PING"; per *pong* si intende un messaggio, il cui contenuto è la stringa "PONG"
- dalla implementazione di ProducerConsumer, si evince che siccome vogliamo un sistema distribuito eterogeneo ci sono metodi che "mandano" IApplMessage, ma in realtà mandano delle stringhe
- ConnectionHandler è un gestore: quando c'è una request, viene creato un ConnectionHandler che incapsula un thread con la connessione. Quindi ci sarà un ConnectionHandler per ogni connessione.
- ConnectionHandler interpreta il messaggio che gli arriva e lo gira a ConsumerMessageHandler che poi lo elaborerà
- elaborate è a metà perchè è ConsumerMessageHandler che deve fabbricare il messaggio di risposta (non può farlo la ConsumerLogic)


### Architettura logica
![[Pasted image 20230328145610.png]]
## Analisi problema
- cosa succede se il producer manda qualcosa di diverso dal ping? Io ho deciso che in tal caso, il consumer non ritornerà alcuna risposta. Però in questo modo il Producer rimarrà bloccato.
### piano di lavoro
Dall’analisi emerge l’opportunità di impostare il seguente piano di lavoro:
sviluppo della logica del ping pong:

**Producer**
1. nella classe *ProducerLogic* realizzo un metodo **doPing()** che ritorna la stringa rappresentante di un ping  
2. nella classe ProducerCaller, nel metodo body(), modifico il contenuto del messaggio con la richiesta doPing()

**Consumer**
1. nella classe *ConsumerLogic* realizzo un metodo **doPong()** che ritorna la stringa rappresentante di un pong  
2.  nella classe *ConsumerMsgHandler*, nel metodo body(), modifico il contenuto della risposta con evalPing()

**Main**
1. nella classe *MainProdCons*, il metodo *configureTheSystemFinal* il metodo *ProdConsConfig.setProtocol* imposterà il tipo di protocollo udp

## Test plans

## Project
Il progetto è pubblicato al link: https://github.com/RootLeo00/sw-eng/tree/main/Appl1-HTTPPingPong

## Testing

## Deployment

## Maintenance

![[Pasted image 20230328152258.png]]
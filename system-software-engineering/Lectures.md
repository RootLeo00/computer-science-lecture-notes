lez 1:

Domande da fare per i requisiti:

-   Chiedere che cos’è ogni parola che il committente dice. Ad esempio, cos’è il DDR ? Non basta il linguaggio naturale, serve avere del codice. Inoltre siccome è un robot, chiedere al cliente se ce l’ha lui.
    
-   Transport trolley: fa azioni in automatico ? oppure prende richieste di alto livello ? Quando comincia a muoversi? Che tipo di segnale lo fa muovere ? (SISTEMA DISTRIBUITO c/s)
  

lez 2:

le websocket vanno in maniera asincrona, mentre http in maniera sincrona.

Differenze risposta sync e async:

se gli do un cpmando sync di muoversi per 5 secondi ottengo per prima la risposta async che ah sbattuto.

![](https://lh4.googleusercontent.com/2bxUonyCpQBxhUTQ1u8gRZ8lZaIa3z7wN1d9LiHuwv2rcZ396aPdH678fMNFosJRRqqdyJ1vt9WNL2XwTeG-uBpyjrv_mPzlxtIwajJLnyRECqGl2o9kTOOksda6RVtisX4Y4xm609asAMEUQnE__pY)

Successivamente mi arriva la risposta sync:

![](https://lh3.googleusercontent.com/I9_leLXLbujPAsHC6RqBDe_2MD_HWzpguifZ9ExjzJoayC9ikCoXmwF7r2A2gMGaWQGBHdTRfudiBYFnF0fKOtMMdllu6vl02QWAoRK0cq0l_bItA0Xow42dYY-WnFOftvL1-JbX6Aw0AdW5kj1Ne5w)

  

Il sonar emette messaggi: è una entità attiva

  
  

Se mando più comandi async, ne cattura solo uno, perchè poi mi segnala:

![](https://lh6.googleusercontent.com/2MzBrf7f6FNtECI6fXJ5SMK9wGAbU-fpdVS-81as0tddALl8J6fInbpDw5Msei_AtZlTqh1HhE2FKuWc_km7zuRhsFiebLbUGyTA3c-YeQU9IpDhZsNcCse8QckGYiUwSM1f12LqLb_8SmOxgiNDFdI)

  
  

Gradle è domain specific: questo vuol dire che è stato inventato nel dominio della costruzione automatizzata del sw. Gradle vuol dire che io voglio che il sistema venga costruito automaticamente da gradle. Non è un linguaggio general purpose

  

lez 3:

todo:

-   input field con localhost parametrizzato dal cliente + bottone “CONNETTI”. Così viene modificato il localhost alla riga 8 del file node/clients/NaiveGui.html
    
-   scrivere test per webSocket in cartella test
    
-   nel template incollare i requirements che ci sono già nella repo
    
-   nel requirements analys mettere tu 
    
-   problem analysys capire cosa i req ti dicono di fare
    
-   in test plans mettere piani di test scritti in linguaggio formale (junit o pseudocodice)?
    

  
  

lez 4:

ANALISI DEI REQUISITI (Template23.html#l-analisi-dei-requisiti):

sicuramente non userò un pojo, perché come fai a interagire con un pojo? Per interagire con un pojo si deve chiamare un suo metodo. Ma non si può fare una interfaccia in cui chiami un metodo ? NO. Inoltre a noi serve un’entità che sappia inviare E ricevere messaggi. Un POJO riesce a inviare messaggi, ma non è fatto così bene per ricevere i messaggi.

“Leggendo la documentazione fornita dal committente (si veda VirtualRobot23.html), si evince che il virtual robot sarà implementato con un servizio (server), in quanto per interagire con esso bisognerà inviargli dei messaggi”

“il sistema è distribuito e avrà almeno 2 componenti: il virtual robot (VirtualRobot23.html, che poi tecnicamente è il server) e l’applicazione (Applicazione1.html#appl1-natura-dei-componenti, che sarà il client di virtual robot)”

Ci sono dei vincoli tecnologici? SI: il committente ha scritto che bisogna utilizzare interazioni http ([#vincolo tecnologico]). Quindi le interazioni saranno sincrone.

Il disegno dell'architettura a due componenti è:

![](https://lh3.googleusercontent.com/AGomX0YJJBwDW_CXxTt29QTs9H8K9U7aTeVy3pQh-YZ-XHIaoGh57PF9bhL4aByVeYLionlFeO7LM46BfQPeqL6zySfupceRVW8OVAMFfjJFm7s36Nz58kd_7dsLbtvM3R6IBq6m_eH59OQ0q8xwDRs)![](https://lh4.googleusercontent.com/6g0ElkAqLU_Aao97PkVlfo-PdJ4eZ9rDWHdDap6eWwYTX1mTVOrMNHg3FN3arojhYM5wB1MsvezCW-BITYXq_tr4pMddnUKvUy9h1w7ghmN7zHFsyPutqG8MtgmKZn4wJ4Mt7HtZKf3ZdlbYWAkQ5Kw)

Ho tolto WS, perchè ci è stato imposto come vincolo che dobbiamo utilizzare una interazione http.

  

ANALISI DEL PROBLEMA (Template23.html#l-analisi-del-problema):

chiarire le problematiche implicate dai requisiti: capisco le problematiche che quei requisiti mi impongono.

In Boundary Walk bisogna capire quali tipi di comandi bisogna dare con l’interazione http.

E’ fattibile? SI

Quali sono i problemi? Si riesce a fargli fare il perimetro, ma è molto molto difficile capire se il virtual robot ha fatto tutto il perimetro.

fornire informazioni su costi/tempi/risorse:capisco le problematiche che quei requisiti mi impongono e soprattutto quali tempi e quali risorse mi impongono.

Quanto ci metto a fare il progetto? Nella mia analisi temporale, ci metto x ore per fare il programma per fargli fare il giro (SPRINT A= implementazione), e tot x ore per fare il testing automatizzato (SPRINT B= testing).

Perché dividiamo in due sprint? Perchè bisogna prima avere un prodotto finito che funziona da fare vedere (è una piccola strategia di mercato)

Per sprint, vedi: [https://pm.stackexchange.com/questions/22510/sprint-vs-milestone-vs-release](https://pm.stackexchange.com/questions/22510/sprint-vs-milestone-vs-release)

Architettura logica: NATURA= iconcine; 

Ogni componente segue il principio di responsabilità: vuol dire che ogni componente ha delle specifiche responsabilità. Ad esempio, il componente applicazione ha la responsabilità di inviare i messaggi.

NB: nell’analisi possiamo mettere del codice o pseudocodice (deve essere capito dalla macchina). Ma non scriveremo mai “Io farei così: <codice>”, bensì “va fatto così:<codice>”.

  

PROGETTAZIONE (Template23.html#progettazione-e-sviluppo-evolutivo):

  

Problema: due protocolli => due codifiche. Vorremmo scrivere le soluzioni protocol independent. Quidni se un giorno un domani volessi utilizzare WS al posto di http? Dovrei cambiare il codice… bleah 🙁

Non esiste un’astrazione che non sia protocol independent?

  
  
  
  
  

parte di lab:  
in application1/build.gradle c’è “id 'eclipse' //crea i .classpath e .project

”

id 'jacoco' : plugin per fare i test in automatico (bellissimo)

  
  
  

lez6:

coding semplice

sistema distribuito

esiste abstraction gap

testing non semplice: perchè le chiamate http non ci danno molte info, ad esempio quando il robot collide, ci dice che collide ma non contro cosa. Inoltre questo tipo di test non è una somma di test sulle piccole singole mosse. infatti il test del perimetro nel complesso ha 2 problematiche maggiori: 1) capire che è in home in ogni momento. 2) capire che il robot ha fatto tutto il giro 1 volta. Analisi del problema: per capire che è in home, il robot ha il muro dietro e alla sua destra. Come conseguenza si ha che al termine del giro il robot non solo deve essere nella cella di home, ma anche direzionato nel verso giusto.

Abstraction gap: non abbiamo modo di verificare la direzione del robot.

Se a una certa mi accorgo che sto scrivendo le classi dell’app solo per far funzionare il testing poi succede CASINO. Obiettivo: perturbare la logica applicativa il meno possibile.

Idea di testing per sapere se il robot ha fatto il giro: posso usare un file di log che riesca a segnare ogni comando/messaggio del robot. Problema: sublimare questa idea. Ciò evitare si cablare contenuti tecnologici in essa del tipo “file di log”, perchè questo farebbe inserire nel codice delle indicazioni di salvataggio di info nel file che poi fa schifo.

Facciamo un modello matematico della scena: vedi “Appl1-HTTPSprint1.html#modello-della-stanza”. Ci vuole anche l’unità di misura.

Qualcuno ha anche pensato di dover usare la formula fisica s=v*t. Problema: se faccio girare il robot più volte scopro che non ha velocità costante. 

Chiedere al committente se il robot rimane sulla terra.

Non serve considerare il tempo e la velocità, ma utilizziamo come unità di misura la distanza: infatti in VirtualRobot23.html c’è scritto “Il robot virtuale (e in futuro anche quelli reali) viene considerato un oggetto inscrivibile in un cerchio di raggio R”. Quindi faccio diventare il robot stesso, l’unità di misura delle mie distanze.

#TODO: modellare la stanza con un pojo.

Sento necessità di sapere momento per momento la posizione del robot.

Obiettivo futuro: far fare il percorso al robot da A a B nella stanza anche se ci sono ostacoli. 

  

il request sync (classe che  è un’astrazione che abbiamo dovuto aggiungere perchè c’è un abstraction gap tra (?)

  

#todo: nuovo progetto App1-HttpSprint2 (Appl1-HTTPSprint2.html) in cui faccio tutto il template completo. Fare i test da soli anche se ci sono già nelle dispense. 

  

NB: fare la console, ma non distribuita, bensì in locale.

Fare test di walkbysteppingwithstop

Il sistema ha nel main solo la configurazione di cmdConsole e ApplCore1

Concepisco l’applicazione come una entità osservabile come dice il GOF. Cioè bisogna rilevare tutti i design pattern. Ad esempio il POJO è un pattern e quindi una entità osservabile.

Farò quindi tanti observer che sono accessori: uno che fa i log, uno i test, etc etc…

Vedi pattern observer

Vuol dire che aggancio tanti observer alla applicazione, così posso farle emettere informazioni.


lez 7:
il 23 marzo c'è simulazione online con presentazione sprint1-sprint2-sprint3

Il main dovrebbe fare solo la parte di configurazione del sistema e avvio del sistema, invece nello sprint1 abbiamo anche parte della logica applicativa (NON VA BENE).
Quindi creiamo una classe Appl1Core che faccia la parte della logica applicativa.



lez 8:
nel template mettere il pattern observer.
La composizione (freccia diamante pieno) vuol dire che c'è bisogno di tutti i componenti perchè l'entità esista (ad esempio l'uomo è una composizione di organi, altrimenti non sarebbe vivo)
L'aggregazione (freccia diamante vuoto) vuol dire che le varie entità possono esistere indipendentemente.
Guardando il pattern observer, per diventare observer del subject dobbiamo prima registrarci: per registrarsi bisognerà avere un rifornimento al Subject e poi chiamare la registerObserver(observer) passando me stesso (this).
Deve nascere la possibilità che il Subject possa chiamare qualcuno senzaa conoscerlo, però guardando il pattern Observable la notifyObservers() scorre tutti i riferimenti CHE CONOSCE (sono quelli che si sono registrati). Ovviamente, con questo pattern nel distribuito non va bene. Pensaci: cosa succede se un Observer si registra al Subject e con l'update si inloopa? Fotte l'applicazione. Perchè l'update è bloccante e quindi l'applicazione si ferma perchè il controllo è passato all'Observer, fino a quando l'update non fa la return.

Il pojo è un organismo con un mitocondrio (in arancione). Il pojo quindi è composto dal mitocondrio, perchè è definito con esso (infatti è all'interno del pojo).
Nello sprint1 non ci vanno bene tutte quelle variabili di stato che abbiamo dovuto introdurre per far funzionare il testing.

Nel template devo esplicitare la nozione di "percorso": un percorso è una stringa, cioè una sequenza di caratteri che possono essere "u", "l" o "|".
NB: gli step non cominciano neanche nel momento in cui parte l'applicazione..

Adesso la nostra applicazione è fatta da 3 componenti (Appl1, vr e observer). Questi 3 componenti vengono messi asssieme dal configuratore (ad esempio il main). Quindi il main dovrà fare:
- il supporto ad http per la comunicazione (perchè è il mitocondrio dentro l'app stessa)
- il vr
- l'app
- l'observer
ATT!!! nel progetto del prof l'observer è inserito e registrato direttamente dall'applicazione ma sarebbe meglio farlo nel main


#TODO fare il configuratore
#TODO fare i vari todo all'interno del codice
#TODO riscrivere il template sprint 2 e sprint 1 solo keywords
#TODO iniziare il template 3 con brainstorming sulla parte di analisi



lez 9 martedì 21 marzo 2023:
la figura di Requisiti SPRINT3 mostra un oggetto attivo che al suo interno utilizza un pojo.
#TODO Nell'analisi per giovedì dobbiamo discutere su quali sono i pro e i contro dei vari protocolli usabili per far parlare la console con appl1.
Nell'analisi: non è Apll1Core che deve ricevere i messaggi, bensì Appl1 perchè Appl1Core è già di per sè funzionante.
#TODO: problematiche dovute a CmdConsoleRemote:
la console dele poter inviare i messaggi e appl1 deve poter ricevere i messaggi. Nella classe Appl1HttpSprint2CmdConsole devo modificare la parte di update() di modo che non sia cablato http. Anzi,  Appl1HttpSprint2CmdConsole non ci sarà più perchè sarà App1 a fare la logica. Possiamo riusare la classe Apache...
#TODO: defnire un piano di lavoro: pensare a cosa fare per risolvere la problematica di interazione tra la console e l'applicazione. Appl1 diventerà un web server (tomcat). HTTP nasce per la human machine interaction. HTTP è costoso, basterebbe usare una normalissima socket che possiamo implementare tcp o udp. Solo che tcp è più affidabile invece udp è più veloce. Se dobbiamo aprire tcp allora il piano di lavoro è: devo fare una classe CmdTCP. Dobbiamo poi fare un server tcp in appl1 che riceve le chiamate e per ogni chiamata crea un nuovo thread (perchè java fa così, ricorda, node invece no, perchè ha la coda di richieste). Quindi avrà tanti thread quanti i clienti fanno la richiesta di  utilizzo del robot. Discussione che dal procedure call passa al message driven.
Ma se volessimo utilizzare più protocolli? Cosa dovremmo usare ? Che astrazione ?
#TODO: comunicare con il VirtualRobot via WS
#TODO: pensare al problema seguente: se in futuro il mio pojo dovesse counicare con ws al posto che con http? Che problemi avrei?



lez 10 martedì 28 marzo
- in un mondo distribuito ho bisogno di una comunicazione ASINCRONA, dunque non useremo RMI che impone una comunicazione sincrona a due vie
- nb: tcp non impone una connessione sincrona
- siccome vogliamo un sistema distribuito eterogeneo io ho dei metodi che "mandano" IApplMessage, ma in realtà mandano delle stringhe:
![[Pasted image 20230328102341.png]]
Questo perchè non vogliamo creare connessioni java to java, bensì anche connessioni eterogenee java to python, to c etc etc
- la tabella di lookup del dns mi assocerà il nome univoco all'indirizzo ip del nodo corrispondente
-  la logica applicativa del Consumer deve essere attivata alla ricezione di un messaggio quindi non potremmo utilizzare rmi
- ConnectionHandler è un gestore: quando c'è una request, viene creato un ConnectionHandler che incapsula un thread con la connessione. Quindi ci sarà un ConnectionHandler per ogni connessione.
- ConnectionHandler interpreta il messaggio che gli arriva e lo gira a ConsumerìMessageHandler che poi lo elaborerà
- ogni componente così implementa il principio di single responsibility
- elaborate è a metà perchè è ConsumerMessageHandler che deve fabbricare il messaggio di risposta (non può farlo la ConsumerLogic)
- #TODO simulatore ping pong: uno fa "ping" e l'altro risponde con "pong" con udp. L'importante è realizzare la comunicazione
  - #TODO copiare ConnectionHandler
  - configureTheSstemNoCoap serve per configurare a mano 



lez 11 30 marzo 2023
- #TODO ping pong simmetrico: il ProducerLogic dovrà avere due metodi (uno che fa ping doPing(), l'altro che fa pong evalPing())
-  siccome la request è bloccante, il ping se non viene ricevuto allora rimane bloccato. Dunque, come analista del problema, devo introdurre una nuova primitiva che abbia due parametri: 1. il contenuto del messaggio (la stringa ping) e il secondo il tempo del timeout. Se scatta il timeout allora posso scegliere se 1. dare un messaggio di errore e poi abortire 2. lanciare una exception (recommended)
- quando ho fatto questa primitiva, poi la inserisco in una libreria 
- #TODO in ProducerCaller.body() devo fare un test un po' più sofisticato: lascio che mandi per 3 volte il ping, e poi mi aspetto che ritornino 3 pong. Ma devo anche fare la situazione più complicata in cui a una certa non arrivi il pong (timeout). Per simularlo, posso fare che il Consumer al secondo ping non risponda.
- il VRHLMovesInteractonSync fa una comunicazione sincrona
- ci sarà da creare VRHLMovesInteractonAsync per fare comunicazione asincrona
- ci sarà Interaction come interfaccia, che avrà sotto due sotto interfacce InteractionSync e InteractionAsync. InteractionSync viene  concretizzato da VrobotHLMovesInteractionSynch che poi sotto ha le varie concretizzazioni con le classi che implementano i vari protocolli (ad esempio VrobotHLMovesHTTPApache) 
- guardando VrobotHLSupportFactory, per creare un supporto per la connessione di tipo X, istanzierò un oggetto di tipo Interaction, passandogli una istanza della factory XConnection. Quindi HTTPConnection sarà una concretizzazione di Connection. Interaction è una interfaccia. 
- Appl1Core è un POJO e non un attore perchè non è un componente attivo: non crea un thread. E' un non-attore, che però è capace di inviare messaggi, ma questo perchè dentro ha un mitocondrio (VrobotHLMovesInteractionSynch) che è capace di inviare messaggi.
- Moreover, un attore sarà in grado di geneticamente inviare e ricevere messaggi.
- problema: non sappiamo come trovare dinamicamente un attore. Adeso devo sapere il suo ip statico
- #TODO sprint3 completo con progetto che gira


lez 12 martedì 4 aprile 2023
- il pattern adapter non va bene perchè il gof dice che è un POJO, e a me serve  un ente attivo, cioè che riceva messaggi. Quindi mi server un server!
- Gli abilitatori di comunicazione che oi abbiamo nel mercato sono Spring per Java, mentre Express per javascript. Un abilitatore di comunicazione deve poter avere una entità che ricdve mesaggi, ma anche poter inviare un messaggio a partire da un pojo.
- Appl1 è un abilitatore: ha un server, ServerFactory, che è capace di ricevere, poi interpreta il messaggio ricevuto e chiamare i metodi di Appl1Core, quindi sa anche emettere messaggi. L'ente collante tra i messaggi ricevuti e la chiamata all'Appl1Core nello sprint3 è Appl1MsgHandler.
- in StepAsynch si vede che un actor sarà message driven, in base ai messaggi che stanno nella coda delle richieste
- è appl1cre che gestisce le azioni
- #TODO dove mettere le callback 



lez 14 martedì 18 aprile
- Interaction è una interfaccia che rende l'applicazione indipendente dalla logica applicativa 
- il file delle WS non hanno una response, quindi la dobbiamo inserire noi !
- nel mainLoop() la take è bloccante! Vuol dire che se la coda è vuota allora rimane in attesa di messaggi
- nb: il robot non gira quando ricevo la collision, ma qunaod ho una step fail. Questo perchè se mi muovo alla collision, rischio di non aver finito lo step e quindi dicendogli prima di girare a sx, rischio di ricevere un not allowed.
- la coda funziona così: arriva un messaggio al server che inoltra le info al msg handler. Il msg handler poi mette il messaggio NELLA CODA.
- con gli attori non ho più RIFERIMENTI di attori ma NOMI di attori. Anche per quelli locali
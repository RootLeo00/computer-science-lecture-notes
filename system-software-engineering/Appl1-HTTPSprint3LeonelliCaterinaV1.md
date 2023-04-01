## Introduction
Nello [Appl1-HTTPSprint2](file:///home/leo/github/sw-eng/issLab23/iss23Material/html/Appl1-HTTPSprint2.html#appl1-httpsprint2), abbiamo costruito e testato un sistema che soddisfa i requisiti del core-businness in ambiente locale.

## Requisiti
1.  Affrontare il progetto e la realizzazione della _CmdConsole_ remota.
2.  Affrontare il progetto e la realizzazione di un sistema distribuito.

## Analisi dei requisiti
- La _CmdConsole_ è un dispositivo di input, che si vuole mantenere libero da aspetti applicativi;
- _CmdConsole_ deve diventare un [actor](file:///home/leo/github/sw-eng/issLab23/iss23Material/html/unibo.basicomm23.html#verso-gli-attori) che interagisce con un utente umano e che invia comandi ad `Appl1` usando il protocollo `P`.
- per _sistema distribuito_ si intende un insieme di processi interconnessi tra loro in cui 
	1. I componenti-base sono enti attivi, che d’ora in poi possiamo denominare, genericamente, attori.
	2. Gli attori **non hanno memoria comune** e scambiano informazione tramite messaggi.


### Architettura logica
![[Appl1FinalArchitecture.png]]
## Analisi problema

### problematiche dovute a CmdConsole
- La _CmdConsole_ deve poter inviare messaggi ad Appl1, che quindi deve poter ricevere messaggi;
- Dopo colloquio con il committente, si è convenuto di usare `TCP`.
- La _CmdConsole_ deve usare un dispatch in quanto si tratta di inviare un comando a _Appl1_
-  Metodologie design protocol independent: non voglio cablare il protocollo nelle classi che implementano la comunicazione (come ad esempio la *VrobotHLMovesHTTPApache*). Per questo uso :
		- l'interfaccia *unibo.basicomm23.interfaces.Interaction*
		- una classe astratta *unibo.basicomm23.utils.Connection* 
		- una factory ConnectionFactory che restituisce un oggetto Interaction con uno specifico protocollo passato come parametro al costruttore.
- **Problema**: Così facendo però dovrei creare una classe per ogni protocollo di comunicazione. Se ci fosse una libreria già fatta? Oppure se ci fosse un altro modo di specificare il protocollo di comunicazione?


La fase di interpretazione dei comandi è affidata a _Appl1_ in quanto:
- Appl1 (assieme ad Appl1Core) deve ricevere i messaggi e quindi lo implementerei con un web server. Non sarà Appl1Core di per sè a ricevere e interperetare i messaggi dato che essa è una classe che si occupa solamente della logica di business e l'abbiamo ottenuto già funzionante dallo sprint2.


### comunicare con il VirtualRobot via WS
- Le websocket richiedono necessariamente una implementazione tcp e poi una comunicazione in http per il setup iniziale
- La comunicazione è bidirezionale dunque il server può fare push delle informazioni al client a differenza di http 
- WS è un event-driven protocol, dunque può essere utilizzato per delle vere comunicazioni real time: gli update possono essere inviati non appena sono disponibili

### altre problematiche
- problema: non sappiamo come trovare dinamicamente un attore. Adesso devo sapere per forza il suo ip statico
### piano di lavoro
Dall’analisi emerge l’opportunità di impostare il seguente piano di lavoro:

sviluppo della CmdConsole remota:
1. refactoring della classe Appl1HttpSprint2CmdConsole, creando una nuova classe CmdConsoleRemote
2. uso ActorNaiveCaller per i parametri di connessione della cmd console
3. mantengo la GUI implementata dallo sprint2 che si realizza nel package console.gui
4. continuo ad utilizzare il pattern observer per la gui e il CmdConsole: CmdConsole è observer della GUI che è observable

sviluppo della Appl1:
1. sviluppo della ServerFactory per ricevere e inviare i messaggi nella Interaction
2. sviluppo Appl1MsgHandler per elaborare il messaggio e chiamare la logica di business di Appl1Core 
3. Appl1Core è Observable perchè
4. Sviluppo classe Appl1Config, che è il file di configurazione di Appl1
5. sviluppo interfaccia Connection
6. sviluppo n classi Connectionxxx (e.g ConnectionHTTP) che implementano ognuna le comunicazioni con un solo protocollo
7. sviluppo VRobotHLSupportFactory che ritorna un supporto IVrobotMoves con il protocollo di comunicazione scelto per la interazione tra il robot e appl1
8. sviluppo VrobotHLMovesInteractionSynch e VrobotHLMovesInteractionAsynch che sono concretizzazioni della interfaccia IVrobotMoves e che inviano i messaggi di comando con il protocollo scelto. In particolare:
	1. VrobotHLMovesInteractionSynch, usa una comunicazione sincrona, con protocollo http
	2. VrobotHLMovesInteractionAsynch, con supporto WebSocket
	#TODO metodo step(time) in entrambe le classi

## Test plans

### testing locale
1. creare una classe TestAppl1HTTPSprint3 con le funzioni già presenti in TestAppl1HTTPSprint2 per testare la comunicazione tra appl1 e il vrobot
2. creare una classe TestCmdConsoleSprint3 per testare la comunicazione tra la console remota e appl1
3. creare una classe TestSprint3 per testare la comunicazione tra cmd console, appl1 e vrobot

## Project
Il progetto è pubblicato al link: https://github.com/RootLeo00/sw-eng/tree/main/Appl1-HTTPSprint3

## Testing


## Deployment


## Maintenance


![[Pasted image 20230328152258.png]]
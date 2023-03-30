## Introduction
Sprint 3 BoundaryWalk con HTTP.

##Requisiti
detto `P` uno dei protocolli definiti in [ProtocolType](file:///home/leo/github/sw-eng/issLab23/iss23Material/html/Appl1-HTTPSprint1.html#unibo-basicomm23-msg-protocoltype):

1.  L’applicazione _Appl1_ viene ora concepito come un ente attivo capace di ricevere messaggi (comandi `start/stop/resume`) via `P` e interpretare tali messaggi, convertendoli in comandi a [Appl1Core](file:///home/leo/github/sw-eng/issLab23/iss23Material/html/Appl1-HTTPSprint2.html#unibo-appl1-http-appl1core) (e di qui a [VirtualRobot23](file:///home/leo/github/sw-eng/issLab23/iss23Material/html/VirtualRobot23.html#virtualrobot23)).
    
2.  _CmdConsole_ deve diventare un ente attivo che interagisce con un utente umano e che invia comandi ad `Appl1` usando il protocollo `P`.

##Analisi dei requisiti
- per "_Appl1_" si intende l'entità omonima mostrata nell'architettura logica (di seguito), che deve ricevere i messaggi da CmdConsole.
- per "interpretare" i messaggi l'Appl1 deve confrontare i messaggi che gli arrivano da CmdConsole e valutare che corrispondano ai messaggi che può riconoscere (comandi `start/stop/resume`). Una volta riconosciuti, attiverà un'azione corrispondente ad ogni comando


**Architettura logica**
![[Pasted image 20230321152403.png]]
##Analisi problema

**problematiche dovute a CmdConsoleRemote**
- La console deve poter inviare messaggi ad Appl1, che quindi deve poter ricevere messaggi
La fase di interpretazione dei comandi è affidata a _Appl1_ in quanto:
- Appl1 (assieme ad Appl1Core) deve ricevere i messaggi e quindi lo implementerei con un web server. Non sarà Appl1Core di per sè a ricevere e interperetare i messaggi dato che essa è una classe che si occupa solamente della logica di business e l'abbiamo ottenuto già funzionante dallo sprint2.
- Per creare una comunicazione remota tra CmdConsole e Appl1, devo modificare la classe Appl1HttpSprint2CmdConsole in questo modo:
1) inserire nella classe di CmdConsole una chiamata remota, sfruttando librerie come *unibo.basicomm23.http* al posto di creare una istanza di Appl1CoreSprint2. Questa modifica attualmente viene realizzata nel metodo update() di Appl1HttpSprint2CmdConsole
![[Pasted image 20230321140911.png]]
In questo modo però, arriverei a cablare la specifica di protocollo all'interno della update(), perciò se in futuro dovessi voler utilizzare un protocollo dierso, dovrei poi modificare il codice core della CmdConsole. Per evitare ciò:
	1) Metodologie design protocol independent: Potrei creare una interfaccia ProtocolInterface che ha una factory ProtocolFactory che leggendo un file di configurazione (dove è scritto il protocollo d comunicazione che si vuole usare) restituisce una istanza della clase che implementa tale protocollo di comunicazione. Così facendo però dovrei creare una classe per ogni protocollo di comunicazione. Se ci fosse una libreria già fatta? Oppure se ci fosse un altro modo di specificare il protocollo di comunicazione?

**piano di lavoro**
Dall’analisi emerge l’opportunità di impostare il seguente piano di lavoro:
sviluppo della CmdConsole:
1. refactoring della classe Appl1HttpSprint2CmdConsole, inserendo una chiamata remota all'Appl1 grazie alla libreria *unibo.basicomm23.http*
2. mantengo la lettura del file di configurazione di Appl1HttpSprint2CmdConsole
3. mantengo la GUI implementata in Appl1HttpSprint2CmdConsole
4. continuo ad utilizzare il pattern observer per la gui e il CmdConsole: CmdConsole è observer della GUI che è observable

sviluppo della Appl1:
1. sviluppo della classe factory ProtocolFactory che legge il file di configurazione e restituisce un oggetto IProtocol
2. sviluppo interfaccia IProtocol
3. sviluppo n classi Protocolxxx (e.g ProtocolHTTP) che implementano ognuna le comunicazioni con un solo protocollo
4. Appl1 è un Observer della classe Protocolxxx (che quindi è observable). Questo perchè Protocolxxx implementa la comunicazione con CmdConsole e dunque quando CmdConsole riceve un messaggio, invoca la notify. Quindi il thread corrispondente di Appl1 verrà "risvegliato" e Appl1 potrà andare a leggere cosa c'è nel canale di comunicazione dell'observable che lo ha risvegliato (viene invocata la update).
Esempio classe Appl1:
![[Pasted image 20230321165953.png]]
![[Pasted image 20230321170013.png]]

**comunicare con il VirtualRobot via WS**
- Le websocket richiedono necessariamente una implementazione tcp e poi una comunicazione in http per il setup iniziale
- La comunicazione è bidirezionale dunque il server può fare push delle informazioni al client a differenza di http 
- WS è un event-driven protocol, dunque può essere utilizzato per delle vere comunicazioni real time: gli update possono essere inviati non appena sono disponibili

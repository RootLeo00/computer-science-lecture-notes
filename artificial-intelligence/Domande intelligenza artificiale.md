1) PIANIFICATORE STRIPS: Si consideri il problema del mondo a blocchi e la sua descrizione in termini STRIPSlike, con predicati on, handempty, clear, ontable ed azioni putdown, pickup, stack ed unstack. Dato un determinato stato iniziale (figura) ed il goals (in figura) si indichi come si comporta un pianificatore lineare (STRIPS) in questa situazione, descrivendo lo stato finale, il goal nel linguaggio STRIPS-like ed i due piani generati per la soluzione considerando i due possibili ordinamenti dei goals. Quale sarebbe il piano ottimo? Perché non lo genera e come si chiama questo problema in letteratura? Come si potrebbe ottenere il piano ottimo utilizzando una diversa metodologia di planning?

STRIPS è un sistema di pianificazione, composto da un linguaggio di rappresentazione degli stati, il goal e delle azioni e da un algoritmo di ricerca backward. Fa parte dei sistemi di ppianificazione classica con planning non deduttivo lineare: si riformula il problema in uno di ricerca nello spazione degli stati.

Rappresentazioni e piani generati per la risoluzione
![[Pasted image 20230610165247.png]]
Risoluzione:
![[Pasted image 20230610165335.png]]
Questa risoluzione soffre il problema dell'anomalia di sussman: infatti se il risolutore parte con il goal 1, on(a,b), allora toglierebbe c da a e poi metterebbe a sopra b, ma poi non riuscirebbe a trovare una risoluzione del goal 2 senza disfare la risoluzione del goal 1. In maniera simile, se il risolutore parte con il risolvere il goal 2, on(b,c), allora prenderebbe il blocco b e lo impilerebbe subito sopra c, ma poi non riuscirebbe a risolvere il goal 1 senza disfare la risoluzione del goal 2 appena fatta. 
L'algoritmo strips quindi procede con la risoluzione di uno dei due goal (ad esempio g1), poi prova a soddisfarre il secondo goal, e quindi dopo vede che la congiunzione on(a,b) ^ on(b,c) non è soddisfqatta. Dunque inserisce di nuovo on(a,b) nello stack e così si raggiunge il goal. 
2) sistema strips: come funziona e limiti
2) Si spieghi cosa si intende per “frame” problem nella pianificazione e si riportino come esempio in Prolog alcuni degli assiomi che lo trattano nella formulazione (calcolo delle situazioni) del mondo a blocchi di Kowalski.

frame problem: occorre specificare esplicitamente tutti i fluent che cambiano
dopo una transizione di stato e anche quelli che non cambiano. Al crescere della
complessità di dominio il numero di tali assiomi cresce enormemente.

Kovalski:
Per descrivere un’azione oltre agli effect axioms occorre specificare tutti i fluent che NON
sono invalidati dall’azione stessa (frame axioms). Nel nostro esempio occorrono i
seguenti assiomi:
on(U,V,S) and diff(U,X) _ on(U,V,do(move(X,Y,Z),S))
clear(U,S) and diff(U,Z) _ clear(U,do(move(X,Y,Z),S))
Occorre esplicitare un frame axioms per ogni relazione NON
modificata dall'azione. Se il problema è complicato la complessità diventa inaccettabile
 Viene utilizzato il predicato holds(rel, s/a) per indicare tutte le relazioni rel vere in
un certo stato s o rese vere da una certa azione a.
 Il predicato poss(s) indica che uno stato s è possibile ovvero raggiungibile.
 Il predicato pact(a,s) si usa per affermare che è lecito compiere una determinata
azione a in uno stato s, ovvero che le precondizioni per svolgere l’azione a sono
soddisfatte in s.
 Se uno stato s è possibile e se le precondizioni di patc di un’azione U sono
soddisfatte in quello stato allora anche lo stato prodotto do(U,S) è possibile:
poss(S) and pact(S)  poss(do(U,S)).
 Quelli che nella formulazione di green sono predicati qui diventano termini. In
pratica, guadagniamo i vantaggi di una formulazione del II ordine mantenendoci in
un sistema del I ordine. In questo mondo abbiamo bisogno di una singola frame
assertion per ogni azione(vantaggio rispetto a Green).
Nell’esempio precedente l’unica frame assertion che deve essere esplicitata è:
 holds(V,S) Ù diff(V,clear(Z)) Ù diff(V,on(X,Y)) _
 holds(V,do(move(X,Y,Z),S)))
che afferma che tutti i termini differenti da clear(Z) e on(X,Y) valgono
ancora in tutti gli stati prodotti dall'esecuzione di move.


3) Si illustri poi la differenza fra Planning Lineare e Partial Order Plannning (POP).
Un pianificatore lineare riformula il problema di pianificazione come problema di ricerca nello
spazio degli stati e utilizza le strategie di ricerca classiche: l’algoritmo di ricerca può essere
forward(dallo stato iniziale al goal)o backward(dal goal ai fatti). Il Partial Order Programming invece riformula il problema in un problema di ricerca degli ordinamenti: si passa ad una ricerca nello spazio dei piani. Infatti in un partial order plan vengono specificate le azioni che devono essere effetuate per arrivare al goal e ne viene specificato anche l'ordine delle azioni qualora sia strettament4e necessario (al contrario del total order programming). Con il POP si riesce a risolvere facilmente il rpoblema ddell'anomalia di sussman in maniera efficiente.

4) Si spieghi cosa e’ il meccanismo di regressione nel planning e a cosa serve.
La regressione è il meccanismo di base per ridurre il goal in sottogoal in un planner backward attraverso regole. Dati un goal G e una regola R:
- Precond: Plist
- Delete: Dlist
- Add: Alist
La regressione di G attraverso R Regr[G,R] è:
 Regr[G,R] = true se G appartiene a Alist.
 Regr[G,R] = false se G appartiene a Dlist.
 Regr[G,R] = G altrimenti.

5) Che cosa si intende per pianificazione gerarchica. Inoltre, si descriva un pianificatore esistente che la realizza.
I pianificatori gerarchici sono algoritmi di ricerca che gestiscono la creazione di piani complessi a diversi livelli di astrazione, considerando i dettagli più semplici solo dopo aver trovato una soluzione per i più difficile.
Tutti gli operatori sono definiti ancora con PRECONDIZIONI e EFFETTI. Algoritmi più famosi:
- STRIPS-Like
- Partial-Order
Dato un goal il pianificatore gerarchico effettua una ricerca di “meta-livello” per generare un piano anch’esso detto “di meta livello” che porta da uno stato molto vicino allo stato iniziale ad uno stato molto vicino al goal. Questo piano va poi rifinito con una ricerca di più basso livello che tiene conto dei dettagli fin qui tralasciati.
ABSTRIPS: Pianificatore gerarchico che usa la definizione delle azioni Strips dove in più a ciascuna precondizione è associato un valore di criticità che consiste nella sua difficoltà di raggiungimento.
Metodologia:
1. Viene fissato un valore di soglia.
2. Si considerano vere tutte le precondizioni il cui valore di criticità è minore del valore di
soglia.
3. Si procede come STRIPS per la ricerca di un piano che soddisfi tutte le precondizioni con
valore di criticità superiore o uguale al valore di soglia.
4. Si usa poi lo schema di piano completo così ottenuto come guida e si abbassa il valore di
soglia di 1
5. Si estende il piano con gli operatori che soddisfano le precondizioni di livello di criticità
maggiore o uguale al nuovo valore di soglia.
6. Si abbassa il valore di soglia fino a che si sono considerate tutte le precondizioni delle
regole originarie.


## PROLOG
1) Si discuta la negazione in Prolog, perché non è la negazione classica, i suoi problemi di utilizzo e se ne mostri la sua implementazione in Prolog. Spiega, quindi, il predicato not e fai la query:
In logica classica, si adopera l'assunzione Open World Assumption, cioè ciò che non è presente nella knowledge base, potrebbe essere vero oppure falso. Al contrario, Prolog adopera la Closed World Assumption, cioè ciò che non è presente nella knowledge base, è considerato falso. Questo vuol dire che i programmi prolog non contengono degli atomi negati nella knowledge base e, conseguentemente, attraverso la risoluzione sld non si possono derivare informazioni negative. Tuttavia, Prolog implementa la negazione per falllimento: queste deriva le negazioni di atomi la cui dimostrazione termina con fallimento in tempo finito.  Dunque se un atomo A appartiene all'insieme di fallimento finito (FF(P)) allora A non è conseguenza logica di P. Per riolvere i letterali negativi, si estende la risoluzione SLD con la NF: si realizza così la risoluzione SLDNF. Quando si deve risolvere il ~qualcosa(X), si considera il positivo di questo predicato (qualcosa(X)) e lo si risolve con SLD: se questa ritorna vero, allora ~qualcosa(X) è falso, altrimenti il contrario. 
il meccanismo della negazione per fallimento in prolog si basa sul concetto che: not(P) è vero se P non è derivabile dal programma. Una possibile realizzazione è la seguente:
not(P) :- call(P), !, fail.
not(P).
La combinazione !,fail è interessante ogni qual volta si voglia, all'interno di una delle clausole per una relazione p, generare un fallimento globale per p (e non soltanto un backtracking verso altre clausole per p). Consideriamo il problema di voler definire una proprietà p che vale per tutti gli individui di una data classe tranne alcune eccezioni. 
vola(X) :- pinguino(X), !, fail.
vola(X) :- struzzo(X), !, fail.
....
vola(X) :- uccello(X).
Attenzione!!! Prolog non adotta una regola di selezione SAFE.
Es. :- not p(X). Il significato di tale query è
ⱻX (not p(X)).
Prolog verifica il goal :- p(X) e il significato di tale query è
ⱻX p(X)
Dopo di che si nega il risultato, ossia: not(ⱻX p(X)) che corrisponde a ⱯX not(p(X))
Mentre noi ci aspettavamo:
ⱻX not(p(X))
Usando la regola di selezione del Prolog, si possono ottenere risultati diversi da quelli attesi a causa delle quantificazioni delle variabili. È buona regola di programmazione verificare che i goal negativi siano sempre GROUND al momento della selezione. Questo controllo è a carico dell’utente!
3) unificazione e occur check in prolog
L’unificazione è un meccanismo che permette di calcolare una sostituzione al fine di rendere uguali due espressioni.
Per espressione intendiamo un termine, un letterale o una congiunzione/disgiunzione di letterali.
Es.
Espressione 1: c(X,Y)
Espressione 2: c(a,K)
Sostituzione unificatrice: θ = [X/a, Y/K]
Le sostituzioni si possono anche comporre: La composizione di sostituzioni θ1θ2 con θ1= [X1/T1, X2/T2,…, Xn/Tn] e θ2= [Y1/Q1, Y2/Q2,…, Yn/Qn] è:
θ1θ2 = [X1/[T1]θ2, …, Xn/[Tn]θ2, Y1/Q1, Y2/Q2, …, Yn/Qn], equivale quindi ad applicare prima θ1 e poi θ2.
(in breve, la composizione di sostituzioni equivale ad applicare prima una delle due sostituzioni e poi l'altra).
In generale, in prolog, sulla base dei diversi tipi di termini, si possono fare le seguenti unificazioni:
tabella
In prolog, si utilizza la Most General Unifier.
Occur check: bisogna stare attenti all'unificazione tra una variabile e un termine composto perchè se il termine composto contiene al suo interno la variabile da unificare, allora l'algoritmo di unificazione non riuscirebbe a terminare e quindi l'unificazione risulterebbe non corretta.
Es. Si consideri l’unificazione tra p(X,X) e p(Y,f(Y)). La sostituzione è [X/Y, X/f(X)]. Chiaramente, due termini unificati con lo stesso termine, sono uguali tra loro. Quindi, Y/f(Y), ma questo implica Y=f(f(f(f(…)))) e il procedimento non termina.
7) Si spieghino i predicati setof, bagof e findall di Prolog e si mostri per ciascuno un esempio.
setof e bagof e findall sono metapredicati di prolog.
setof e bagof servono per risondere a query del tipo “quale è l'insieme S di elementi X che soddisfano la query p(X)?”.
setof(X,P,S) e bagof(X,P,L) forniscono in S e in L, rispettivamente, l’insieme e la lista
delle istanze X che soddisfano il goal P. bagof produce liste in cui possono esservi
ripetizioni, mentre setof elimina le ripetizioni.
Vediamo un esempio. Supponiamo di
avere la base di conoscenza:
p(0).
p(1).
p(2).
p(1).
Invocando setof(X, p(X), S) otteniamo YES, S=[0,1,2], mentre con bagof(X, P(X), L) otteniamo YES,
L=[0,1,2,1]. In entrambi i casi otteniamo anche X=X, dunque la variabile X non viene legata ad alcun valore.
setof viene utilizzato per realizzare una implicazione, mentre bagof viene usato per realizzare una iterazione.
Il predicato findall(X,P,S) differisce da setof/3 e bagof/3 in quanto quantifica esistenzialmente la variabile non usata nel primo argomento.
Supponiamo di avere un data base del tipo:
padre(giovanni,mario). 
padre(giovanni,giuseppe).
padre(mario, paola).
padre(mario,aldo).
padre(giuseppe,maria).
:- findall(X, padre(X,Y), S). -> yes S=[giovanni, mario, giuseppe]
X=X
Y=Y
Findall e’ dunque vero se S e’ la lista delle istanze X senza ripetizioni per cui la proprieta’ P e’ vera. Quindi usare findall(X, padre(X,Y), S) ci porta alla fine ad avere anche Y non istanziata.
Per ottenere lo stesso effetto con setof basta usare una sintassi del tipo setof(X, Y^padre(X,Y), S).
9) Incompletezza dell’interprete prolog
La risoluzione in prolog risuta essere incompleta, dato che l'albero sld viene esplorato con tecnica depth first. 
12) call(T)
il termine T viene trattato come un atomo predicativo e viene richiesta la valutazione del goal T all'interprete Prolog. Viene definito come un predicato di meta-livello in quanto consente l’invocazione dell’interprete Prolog all’interno dell’interprete stesso.
Es. Si supponga di voler realizzare un costrutto condizionale del tipo if_then_else:
if_then_else(Cond,Goal1,Goal2) -> Se Cond è vera viene valutato Goal1, altrimenti Goal2
if_then_else(Cond,Goal1,Goal2):- call(Cond), !, call(Goal1).
if_then_else(Cond,Goal1,Goal2):- call(Goal2).
14) esempio di uso del cut in prolog
Il predicato CUT rende definitive alcune scelte fatte nel corso della valutazione dall’interprete Prolog. 
Il cut può essere usato per realizzare la mutua esclusione tra clausole.
if a(.) then b else c.  con il CUT:
p(X) :- a(X), !, b.
p(X) :- c.

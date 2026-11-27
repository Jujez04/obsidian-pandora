 Correzione esercizi per casa.

1 Si consideri l'alfabeto {a,b}. Definire una RE per i linguaggi che 
contengono le stringhe del seguente tipo:

b- un numero pari di "a" ed esattamente una "b"
"test_b.txt"
Accept
Accept
Accept
Accept
Reject
Reject

c- ogni gruppo di "b" adiacenti contiene un numero di "b" multiplo di 2 o di 3
"test_c.txt"
Accept
Accept
Accept
Accept
Reject
Reject
Reject

------------------------------------------------------------------------------

1. Esercizio su minimizzazione DFA. Effettuare (a mano con l'algoritmo riempi-tabella 
visto a lezione) la minimizzazione dell'automa in exMinimization.jff

2. Verificare se le seguenti due espressioni regolari sono equivalenti (trasformarle 
in DFA automaticamente con JFLAP, senza minimizzarle (!) e poi verificarne l'equivalenza 
a mano con l'algoritmo riempi-tabella visto a lezione):
- (0+1)\*01
- (0+1)\*(10+!)01
I DFA e i nomi degli stati da usare sono nel file "stati equivalenza.jpg"

3. Dire se i seguenti linguaggi sono regolari (possibilmente sfruttando le proprieta' 
di chiusura) o meno. Giustificare la risposta.

ESEMPIO GIA' DISCUSSO
a- Linguaggio $L=\{0 ^h 1^ h | h>=0\}$ su alfabeto {0,1} 

Non e' regolare intuitivamente perche' un ipotetico automa a stati finiti per L 
(che ha una "memoria" costante data dal suo numero n di stati), mentre consuma 
una stringa 0 ^h 1^ h in input, dovrebbe "ricordare" la quantita' degli 0 (che sono 
in quantita' h illimitata, dipendente dalla dimensione dell'input) per poter 
controllare che gli 1 siano effettivamente tanti quanto gli 0 (altrimenti uscirebbe 
dal linguaggio L) e accettare. 

Piu' formalmente (ma non vi viene richiesto):
Sia n il numero degli stati di un ipotetico DFA A tale che L(A) = L. 
Si ha 0 ^n 1^ n appartiene ad L.
Nel leggere lo 0^n iniziale A deve passare per un loop 
(tocca n+1 stati, includendo quello iniziale e l'ultimo raggiunto).
Omettendo il loop si ha che A accetta anche una stringa 0 ^m 1^ n con m\<n
(la quale non appartiene ad L).
Contraddizione.

b- Linguaggio L su alfabeto {0,1} delle stringhe il cui numero di 0 e' uguale al numero di 1 -> No

c- Linguaggio L su alfabeto {a,b} delle stringhe con un numero pari di a ed almeno 3 b -> Sì perché può essere espresso come intersezione di due linguaggi regolari.

d- Linguaggio L su alfabeto {0,1} definito induttivamente come segue.
Base: \epsilon, 0, e 1 appartengono ad L.
Induzione: se w appartiene ad L, anche 0w0 e 1w1 appartengono ad L.
(nessun altra stringa appartiene ad L)
Prima di tutto capire che linguaggio e' L (descriverlo direttamente come insieme di stringhe).
-> Sono le stringhe palindromiche su alfabeto {0,1}

SI 010, 1001
$L = \{w \in \{0,1\}^* | w = w^R\}$

No perché bisogna ricordare la prima parte della stringa per poterla confrontare con la seconda parte.

PER CASA

1) Terminare l'esercizio 2

2) Dire se i seguenti linguaggi sono regolari (possibilmente sfruttando le proprieta' 
di chiusura) o meno. Giustificare la risposta.

e- Linguaggio L delle parentesi tonde bilanciate; cioe', delle stringhe, sull'alfabeto 
composto dai due simboli "(" e ")", definito induttivamente come segue.
Base: \epsilon appartiene ad L.
Induzione: se w e w' appartengono ad L, anche ww' e (w) appartengono ad L.
(nessun altra stringa appartiene ad L)

Esempi di stringhe del linguaggio: 
()()((()))
(()())
(())((()))
Esempi di stringhe non del linguaggio: 
)(
(()


Risposta informale: uno deve conoscere il numero di parentesi aperte a priori per poterle chiudere correttamente, quindi non e' regolare.
Risposta formale:  Sia n il numero degli stati di un ipotetico DFA A tale che L(A) = L. 
Per esempio si ha $( ^n )^ n$ appartiene ad L.
Nel leggere lo (^n iniziale A deve passare per un loop 
(tocca n+1 stati, includendo quello iniziale e l'ultimo raggiunto).
Omettendo il loop si ha che A accetta anche una stringa ( ^m )^ n con m\<n
(la quale non appartiene ad L).
Contraddizione.


  ![[stati equivalenza.jpg]]
ESERCIZI PER CASA

1) Si consideri il PDA costruito dalla CFG del file "CFG_5.jff" all'esercizio 5.
Considerare la stringa "aabc". Mostrare:
- una derivazione canonica sinistra (lm) di tale stringa con la CFG
- la sequenza di passaggi di configurazione (descrizioni istantanee) che portano
il PDA a riconoscere tale stringa

2) Definire un PDA che riconosca **per pila vuota** il linguaggio delle stringhe binarie in cui il numero di 0 è uguale al doppio del numero di 1.

-----

Esercizi su parsing con algoritmo CYK e backpointers

Prima riprendiamo esempio parsing stringa "baaba" visto in slides di lezione.

Per calcolare le variabili A che fanno parte, es., dell'insieme X25 
(cioe' le variabili da cui è possibile derivare la sottostringa "aaba")
dobbiamo considerare le produzioni A->BC con B e C appartenenti 
alle coppie di insiemi (B appartiene al primo e C al secondo): 
- (X22,X35) cioe' con B derivo "a" e con C derivo "aba"
- (X23,X45) cioe' con B derivo "aa" e con C derivo "ba"
- (X24,X55) cioe' con B derivo "aab" e con C derivo "a"

1. Si consideri la grammatica in CNF con variabile iniziale S:

S -> PB | AV | SJ
J -> CS
A -> NN
B -> VN | VP | MV
N -> f | s | b | l
P -> i | t
V -> c | f | s | b | l
M -> c
C -> a

a) Usare l'algoritmo CYK per fare parsing della stringa "blb" segnando le produzioni applicate (backpointers).
b) Disegnare il parse tree di "blb" ottenuto seguendo i backpointers.


{S}
{A, B}              {A, B}
{N, V}             {N, V}           {N, V}
b                      l                    b


S
|  \ 
A V
|   \  \
N N  b
|   |
b  l


2. Si aggiunga alla grammatica dell'esercizio 1 la produzione: S -> NB 

a) Usare l'algoritmo CYK per fare parsing della stringa "blb" segnando le produzioni applicate (backpointers).
b) Disegnare i parse trees di "blb" ottenuti seguendo i backpointers.

![[esercizio2a.jpg]]
{S}
{A, B}              {A, B}
{N, V}             {N, V}           {N, V}
b                      l                    b

![[esercizio2b.png]]



3. Si consideri la stessa grammatica G dell'esercizio 2 e la stringa w = "icfatcs"

S -> PB | AV | SJ | NB
J -> CS
A -> NN
B -> VN | VP | MV
N -> f | s | b | l
P -> i | t
V -> c | f | s | b | l
M -> c
C -> a


a) Usare l'algoritmo CYK per fare parsing della stringa w segnando le produzioni applicate (backpointers).
b) Dire se la stringa w appartiene o meno a L(G). Giustificare la risposta. In caso affermativo mostrare i parse trees di w ottenuti seguendo i backpointers.
![[esercizio3a.jpg]]
{S}
{}            {}
{}            {}              {}
{}            {}              {}               {J}
{S}         {}              {}                {}             {S}  
{}            {B}           {}                {}             {}             {B}
{P}         {M, V}      {V, N}         {C}           {P}           {V,M}       {V, N}
i              c              f                   a              t               c              s

![[esercizio3b.png]]

4. Come esercizio 3, considerando però la stringa w = "atcs"

S -> PB | AV | SJ | NB
J -> CS
A -> NN
B -> VN | VP | MV
N -> f | s | b | l
P -> i | t
V -> c | f | s | b | l
M -> c
C -> a

![[esercizio4a.jpg]]

La stringa "atcs" non appartiene a L(G) in quanto X_14 non contiene la variabile iniziale S
(non ci sono quindi parse trees per "atcs").

----

5. Applicare la trasformazione standard per costruire una CFG a partire dal PDA esercizio5.jff (che riconosce **per pila vuota**) -> DA RIGUARDARE

```
G = (V,T,P,S)

V={S} U {[q X q']| q,q'\in Q e X \in \Gamma}
cioè:
V= { S,  
   [q0 0 q0], [q0 0 q1], [q1 0 q0], [q1 0 q1],
   [q0 1 q0], [q0 1 q1], [q1 1 q0], [q1 1 q1],  
   [q0 Z q0], [q0 Z q1], [q1 Z q0], [q1 Z q1], 
   }

T= {0,1}

**** PRODUZIONI P ******

S->[q0 Z r] PER OGNI r \in Q,
quindi


S->[q0 Z q0] | [q0 Z q1]


---- transizioni di q0

TRANSIZIONE 1 (k=2)
ho le produzioni [q0 Z r2] -> 1[q0 1 r1][r1 Z r2] PER OGNI r1,r2 \in Q, 
quindi:

                                     r1  r2
[q0 Z q0] -> 1 [q0 1 q0] [q0 Z q0]   q0  q0
[q0 Z q0] -> 1 [q0 1 q1] [q1 Z q0]   q1  q0
[q0 Z q1] -> 1 [q0 1 q0] [q0 Z q1]   q0  q1
[q0 Z q1] -> 1 [q0 1 q1] [q1 Z q1]   q1  q1


TRANSIZIONE 2 (k=0) 
[q0 1 q0] -> 0

TRANSIZIONE 3 
[q0 0 q0] -> 1

TRANSIZIONE 4
ho le produzioni [q0 1 r2] -> 1[q0 1 r1][r1 1 r2] PER OGNI r1,r2 \in Q

[q0 1 q0]-> 1 [q0 1 q0][q0 1 q0] |
            1 [q0 1 q1][q1 1 q0]
[q0 1 q1]-> 1 [q0 1 q0][q0 1 q1] |
            1 [q0 1 q1][q1 1 q1]

TRANSIZIONE 5
ho le produzioni [q0 0 r2] -> 0[q0 0 r1][r1 0 r2] PER OGNI r1,r2 \in Q

[q0 0 q0] -> 0 [q0 0 q0] [q0 0 q0] | 
             0 [q0 0 q1] [q1 0 q0]
[q0 0 q1] -> 0 [q0 0 q0] [q0 0 q1] | 
             0 [q0 0 q1] [q1 0 q1]

TRANSIZIONE 6
ho le produzioni [q0 Z r2] -> 0[q0 0 r1][r1 Z r2] PER OGNI r1,r2 \in Q

[q0 Z q0] -> 0 [q0 0 q0] [q0 Z q0] |
             0 [q0 0 q1] [q1 Z q0] 
[q0 Z q1] -> 0 [q0 0 q0] [q0 Z q1] |
             0 [q0 0 q1] [q1 Z q1] 

TRANSIZIONE 7
ho le produzioni [q0 Z r1] -> [q1 Z r1] PER OGNI r1 \in Q

[q0 Z q0] -> [q1 Z q0]
[q0 Z q1] -> [q1 Z q1]

TRANSIZIONE 8
ho le produzioni [q0 0 r1] -> [q1 0 r1] PER OGNI r1 \in Q

[q0 0 q0] -> [q1 0 q0]
[q0 0 q1] -> [q1 0 q1]

----  transizioni di q1

TRANSIZIONE 1
[q1 Z q1] -> \epsilon

TRANSIZIONE 2
[q1 0 q1] -> \epsilon

```
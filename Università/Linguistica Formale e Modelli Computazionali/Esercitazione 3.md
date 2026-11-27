Correzione esercizi per casa.

Abbiamo realizzato (prima esercitazione in laboratorio) un DFA che controlla la correttezza 
delle somme binarie. Data la stringa: a_0 b_0 c_0 ... a_n b_n c_n controlla se 
a_n...a_0 + b_n...b_0 = c_n...c_0
Esercizio era valutare se sia possibile realizzare un DFA di questo tipo che riceve pero' 
l'input nel formato a_n b_n c_n...a_0 b_0 c_0 (esercizio 1) oppure 
a_n...a_0 b_n...b_0 c_n...c_0 (esercizio 2).

-------------------------------------------------------------------------------

[NOTA: in JFLAP l'espressione regolare stringa vuota e' indicata con !]

1 Considerare l'espressione regolare E=0*(0+11)1*. Dire quale linguaggio rappresenta(ovvero L(E)).

L(0) = 0
L(0\*) = L(0)\* = {0}\* = $\bigcup_{i \geq 0} \{0\}^i$  
L(0 + 11) = L(0) ∪ L(11) = {0} ∪ {11}  = {0, 11}
L(E) = L(0\*(0+11)1\*) = L(0)\*L(0+11)L(1)\* = $\{0^n x 1^m \space| n \geq 0, m \geq 0, x \in \{0,11\}\}$

2 Considerare l'espressione regolare 1\*0+0\*1. Trasformarla in un epsilon-NFA equivalente usando l'algoritmo visto a lezione (del libro di testo). Confrontare poi tale epsilon-NFA con quello ottenuto 
trasformando l'espressione regolare in un epsilon-NFA con JFLAP.

3 Trasformare in RE l'automa nel file "nfa2re" usando l'algoritmo visto a lezione (del libro di testo).

4 Si consideri l'alfabeto {a,b}. Definire una RE per i linguaggi che 
contengono le stringhe del seguente tipo (verificare, trasformando con JFLAP la RE in un epsilon-NFA, se vengono gestite correttamente le stringhe nei file "test_x.txt" indicati):

(aa)\*(bbb)\*(bb)

a- un numero pari di a seguito da un numero di b che diviso per 3 da' resto 2
"test_a.txt"
Accept
Accept
Accept
Accept
Reject
Reject



---------
Per casa:
--------- 

Si consideri l'alfabeto {a,b}. Definire una RE per i linguaggi che 
contengono le stringhe del seguente tipo (verificare, trasformando con JFLAP la RE in un epsilon-NFA, se vengono gestite correttamente le stringhe nei file "test_x.txt" indicati):

b- un numero pari di "a" ed esattamente una "b"
"test_b.txt"
Accept
Accept
Accept
Accept
Reject
Reject

((aa)\*(b)(aa)\*) + ((aa)\*ab(aa)\*a)
`(aa)*(b)(aa)*+(aa)*ab(aa)*a`

c- ogni gruppo di "b" adiacenti contiene un numero di "b" multiplo di 2 o di 3
"test_c.txt"
Accept
Accept
Accept
Accept
Reject
Reject
Reject

`((bb)*+(bbb)*)a)*((bb)*+(bbb)*)`
`((bb)*+(bbb)*))(a(bb)*+(bbb)*)*`

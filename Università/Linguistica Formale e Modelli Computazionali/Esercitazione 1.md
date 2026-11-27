** Costruzione di DFA **

Consideriamo l'alfabeto {a,b}. 
Realizzare dei DFA che riconoscono i seguenti linguaggi:

1 - stringhe con un numero dispari di a

2 - stringhe che terminano con bb

$L = \{xbb | x \in \{a,b\}*\}$

3 - stringhe che non terminano con bb

4 - stringhe con un numero pari di a ed almeno 3 b

SI bbb,bababb

NO $\epsilon$,bbaa,bababa

5 - stringhe che contengono la sottostringa aaa o
la sottostringa aba (contengono almeno una delle due)

SI babab,aaaa,aaba

NO $\epsilon$,abba,a,b,ab

6 - Realizzare un DFA che riconosca il seguente linguaggio su alfabeto {0,1}:
stringhe che interpretate come numero binario risultano un multiplo di 5 

SI 101,1010,1111,0, 10100, 11001

NO 111,1,10

Nota: pensa allo shift a destra con 0 è un \*2x mentre se è con 1 è un \*(2x+1), dove x è il resto

7 - Sempre considerando alfabeto {0,1}, realizzare un DFA che controlla la
correttezza delle somme binarie:
data la stringa: $a_0 b_0 c_0 a_1 b_1 c_1 ... a_n b_n c_n$ controlla 
se $a_n...a_1 a_0 + b_n...b_1 b_0 = c_n...c_1 c_0$
(cioè a+b=c con a,b,c numeri binari con stessa lunghezza) 

011 + 010 = 101

SI  110001 001110

casi base
0 + 0 = 0
1 + 1 = 0 ma deve venire da un riporto
1 + 0 = 1

0 + 1 = 0 -> riporto
1 + 0 = 0 -> riporto
1 + 1 = 1 -> riporto



Per casa: 

1 Valutare se e' possibile realizzare un DFA che controlli la  
correttezza delle somme binarie come nell'esercizio 7, che riceva 
pero' l'input nel formato $a_n b_n c_n...a_0 b_0 c_0$

Sì perché è regolare, in particolare si può costruire il DFA concentrandosi sul riporto.
Per vedere dettagli sull'implentazione vedere il file allegato.

SI 001010110, 011010110, 011001100110
No 0, 11, 110001

2 Valutare se e' possibile realizzare un DFA che controlli la  
correttezza delle somme binarie come nell'esercizio 7, che riceva 
pero' l'input nel formato a_0...a_n b_0...b_n c_0...c_n

No perché non è regolare. La sequenza di caratteri, in particolare con questo input servirebbe conoscere a priori la lunghezza di a,b,c per poter fare il controllo della somma, quindi non è possibile realizzare un DFA.
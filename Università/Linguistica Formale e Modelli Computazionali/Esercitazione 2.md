**Costruzione di NFA**

1 - Dato l'alfabeto {a,b} realizzare un NFA che riconosce le stringhe che 
contengono aaa oppure aba

SI babab,aaaa,aaaba, aaba

NO \epsilon,abba,a,b,ab

2 - Realizzare un NFA che riconosce le stringhe non vuote sull'alfabeto {0,1,2} 
in cui l'ultima cifra appare almeno una volta in precedenza

SI 011,121,22,0120

NO \epsilon,012,20,1

3 - Realizzare un NFA che riconosce le stringhe non vuote sull'alfabeto {0,1,2} 
in cui l'ultima cifra NON appare in precedenza (da riguardare)

SI 012,20,1

NO \epsilon,011,121,22,0120

**Costruzione di epsilon-NFA**

4 - Realizzare un epsilon-NFA che riconosce le stringhe non vuote sull'alfabeto {0,1,2} 
in cui l'ultima cifra NON appare in precedenza 
(rispetto all'esercizio precedente si sfrutti il fatto di disporre delle transizioni epsilon).

** Trasformazione da NFA a DFA **

5 - Dato l'alfabeto {a,b} si consideri l'NFA fatto all'esercizio 1, che riconosce 
le stringhe che contengono aaa oppure aba. Trasformarlo in DFA.


Manda file txt in caso di non possibilità della soluzione
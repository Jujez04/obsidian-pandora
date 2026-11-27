Esercizi per casa.

PER CASA

Si consideri il linguaggio su alfabeto {(,)} delle stringhe generate dalla
CFG
S -> SS | (S) | ()
Dire se tale CFG è ambigua o meno. In caso affermativo mostrare una stringa generata dalla CFG che abbia (almeno) due alberi sitattici/derivazioni canoniche sinistre. Mostrare tali alberi sintattici e le corrispondenti derivazioni canoniche sinistre.
Mostrare inoltre le corrispondenti derivazioni canoniche destre.

-------

1. Definire un automa a pila (PDA) che riconosca **per stato finale** i seguenti linguaggi sull'alfabeto {0,1}:
a- linguaggio delle stringhe 0^n 1^n (mostrare che "0011" è riconosciuta dal PDA)
b- linguaggio delle stringhe in cui il numero di 0 e' uguale al numero di 1
c- linguaggio delle stringhe in cui il numero di 0 e' maggiore o uguale del numero di 1 

2. Definire un PDA che riconosca **per pila vuota** il linguaggio delle stringhe binarie in cui il numero di 0 è uguale al doppio del numero di 1.

3. Si consideri il PDA del punto "c" dell'esercizio 1. Applicare la trasformazione standard per costruire un PDA che riconosce lo stesso linguaggio **per pila vuota**.

4. Applicare la trasformazione standard per costruire, a partire dal PDA che riconosce **per pila vuota** dell'esercizio 2, un PDA che riconosce lo stesso linguaggio **per stato finale**.

5. Applicare la trasformazione standard per costruire un PDA che riconosce **per pila vuota** a partire dalla CFG del file "CFG_5.jff" (che genera il linguaggio delle stringhe a^n b^m c^p in cui n <= m+p, fatta all'esercitazione scorsa).

PER CASA

1) Si consideri il PDA costruito dalla CFG del file "CFG_5.jff" all'esercizio 5. Considerare la stringa "aabc". Mostrare:
- una derivazione canonica sinistra (lm) di tale stringa con la CFG
	- S => aSc => aBc => aaBbc => aabc
- la sequenza di passaggi di configurazione (descrizioni istantanee) che portano il PDA a riconoscere tale stringa
	- (q0, aabc, Z) => (q0, aabc, SZ) => (q0, aabc, aScZ) => (q0, abc, ScZ)=> (q0, abc, BcZ) => (q0, abc, aBbcZ) => (q0, bc, BbcZ) =>(q0, bc, bcZ) => (q0, c, cZ) (q0, $\epsilon$, Z)  
1) Definire un PDA che riconosca **per pila vuota** il linguaggio delle stringhe binarie in cui il numero di 0 è uguale al doppio del numero di 1.
	- L'invariante principale è che la lunghezza delle stringhe deve essere multipla di 3
	- Stringhe SI: 010, 001, 100, 000011, 001001, 11000



 --1) Dire qual'è il linguaggio riconosciuto (per stato finale) dal PDA "percasa.jff". 

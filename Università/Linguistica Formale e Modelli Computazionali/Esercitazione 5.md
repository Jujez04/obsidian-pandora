Es n.1

Grammatica per parentesi:
- 1) S -> $\epsilon$ 
- 2) S -> (S)
- 3) S -> SS


Definire una grammatica libera dal contesto (CFG) per i seguenti linguaggi sull'alfabeto $\{a, b, c\}$:
- Linguaggio delle stringhe $\{a^nb^{2n}|n\geq0\}$, mostrare che "aabbbb" è appartente al linguaggio:
	- Stringhe SI: abb, aabbbb. $\epsilon$, ...
	- Stringhe NO: bab, aabbb
	- S -> $\epsilon$
	- S -> aSbb  
- Linguaggio delle stringhe $(abc)^n(cba)^n$
	- Stringhe SI: abccba, abcabccbacba
	- Stringhe NO: abcabc, a, b
	- S -> $\epsilon$
	- S -> abcScba
- Linguaggio delle stringhe $a^nb^m$ in cui $n\leq m$
	- Stringhe SI: aabb, abb, $\epsilon$, b
	- Stringhe NO: aaabb
	- S -> $\epsilon$
	- S -> aSb
	- S -> Sb
- Linguaggio sulle stringhe $a^nb^mc^n$ con $n$ pari e $m$ dispari
	- S -> aaScc
	- S -> bI
	- I -> bbI
	- I -> $\epsilon$
- Linguaggio sulle stringhe $a^nb^mc^p$ con $n\leq m + p$
	- Stringhe SI: $\epsilon$, ab, ac, abc
	- S -> Sc
	- S -> aSc
	- S -> P
	- P -> aPb
	- P -> Pb  
	- P -> $\epsilon$

Ex n.2

Dire se la CFG del punto e dell'es 1 è ambigua o meno. Si basta prendere abc ed è possibile vedere che esistono due alberi sintattici

S => aSc => aBc => aBbc => abc
S => Sc =>Bc => aBbc => abc

Per casa 

Si consideri il linguaggio delle parentesi
- S -> SS
- S -> (S)
- S -> ()

Mostrare alberi sintattici, derivazioni canoniche sinistre e corrispondenti canoniche destre

S => SS => ()S => ()SS => ()()S => ()()()
S => SS => SSS => ()SS => ()()S => ()()()

S => SS => S() => SS() => S()() => ()()()
S => SS => SSS => SS() => S()() => ()()()
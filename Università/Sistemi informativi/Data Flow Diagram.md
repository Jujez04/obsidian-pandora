- Traggono origine dalla teoria dei grafi e sono stati utilizzati anche precedentemente all’avvento dei computer per la gestione delle informazioni.
- Non esiste in letteratura a tutt’oggi una definizione universalmente accettata e sono presenti molteplici differenti formulazioni operative
- Si focalizza esclusivamente su **cosa fanno i dati**: da dove arrivano, quali processi li elaborano, dove vengono archiviati e verso quali destinazioni esterne escono.
# Caratteristiche principali

## Componenti
Qualsiasi DFD, indipendentemente dalla complessità, è costruito combinando esclusivamente quattro elementi sintattici:
1. **Processo (Process)**: trasforma i dati in ingresso in dati in uscita attraverso calcoli, aggregazioni o regole di business.
    - _Notazione_: Cerchio (Yourdon/DeMarco) o rettangolo con angoli arrotondati diviso in sezioni (Gane/Sarson).
    - _Convenzione di denominazione_: Verbo all'infinito + Sostantivo (es. _Calcola Imposte_, _Verifica Credenziali_).
2. **Flusso di Dati (Data Flow)**: il canale attraverso cui transitano le informazioni tra processi, depositi ed entità esterne.
    - _Notazione_: Freccia direzionale con etichetta.
    - _Convenzione di denominazione_: Sostantivo descrittivo del pacchetto dati (es. _Ordine inviato_, _Dati pagamento_).
3. **Deposito Dati (Data Store)**: archivio statico in cui i dati vengono memorizzati per essere riutilizzati in un secondo momento (equivalente a tabelle database, file o archivi cartacei).
    - _Notazione_: Due linee parallele orizzontali (Yourdon/DeMarco) o rettangolo aperto a destra con barra numerica (Gane/Sarson).
    - _Convenzione di denominazione_: Sostantivo al plurale (es. _Clienti_, _Transazioni_, _Giacenze di magazzino_).
4. **Entità/Agente Esterna (Terminator / External Entity)**: attori, reparti aziendali o sistemi software esterni che interagiscono con il sistema senza farne parte. Possono agire da sorgente (_source_) o da destinazione (_sink_) dei dati.
    - _Notazione_: Rettangolo standard a spigoli vivi.
    - _Convenzione di denominazione_: Sostantivo singolare o collettivo (es. _Cliente_, _Gateway di Pagamento_, _Agenzia delle Entrate_).
- Tabella riassuntiva:
![[Pasted image 20261013170748.png]]

## Sintassi
- La sintassi dei DFD impone un principio fondamentale: **i dati non si muovono né si trasformano autonomamente**. Qualsiasi spostamento o modifica informativa deve essere mediato da un processo.

|**Collegamento**|**Consentito?**|**Motivo**|
|---|---|---|
|**Processo $\leftrightarrow$ Processo**|Sì|Normale passaggio di dati elaborati tra moduli|
|**Processo $\leftrightarrow$ Deposito Dati**|Sì|Operazioni di lettura (_query_) o scrittura (_update/insert_)|
|**Processo $\leftrightarrow$ Entità Esterna**|Sì|Input dall'esterno o output verso l'esterno|
|**Entità Esterna $\leftrightarrow$ Entità Esterna**|**No**|Comunicazione esterna non gestita dal sistema in esame|
|**Entità Esterna $\leftrightarrow$ Deposito Dati**|**No**|Un utente o sistema terzo non scrive né legge da un archivio senza logica applicativa|
|**Deposito Dati $\leftrightarrow$ Deposito Dati**|**No**|I dati non migrano spontaneamente da una tabella all'altra senza un'istruzione di elaborazione|
- In particolare un DFD è una quartupla $(P, D, A, F)$ dove:
    - $P = \{p_1, p_2, ...\}$  insieme dei processi
    - $D = \{d_1, d_2, ...\}$ insieme dei depositi dati
    - $A = \{a_1, a_2, ...\}$  insieme degli agenti esterni
    - $F = \{f \in (P\times(P\cup D \cup A)) \cup ((P\cup D \cup A)\times P)\}$ è un insieme finito dei flussi di dati
- Quindi può essere espresso come un grafo in cui ogni nodo può appartenere a uno dei tre insiemi $P$, $D$ o $A$, mentre ogni arco appartiene all'insieme $F$.
![[Pasted image 20261013173917.png]]
## Strategie di costruzione
- **Top-down**: decomposizione di un processo in una serie di sottoprocessi chiaramente identificabili e indipendenti.
- **Bottom-up**: a partire da una collezione di concetti elementari si costruiscono via via le connessioni fra essi.
- **Mixed**: raffinamento di un DFD di massima in stadi successivi con tecniche top-down e bottom-up
- **Outside-in**: parte dalle interfacce con il sistema e propaga in avanti gli ingressi evidenziando i processi coinvolti nei flussi di dati o, in alternativa, propaga all’indietro le uscite.
## Convenzioni e vincoli
- Nomi univoci per identificare processi, flussi di dati, agenti e depositi.
- Numerare i processi.
- Non rappresentare le eccezioni e il trattamento degli errori, rimandando questi dettagli alle fasi finali dell’analisi.
- Accertarsi della coerenza interna di un DFD e che sia coerente con quelli ad esso associati
- Un DFD non è un diagramma di flusso, dove le frecce indicano un ordinamento negli eventi.
- In alcune estensioni, come nella notazione OMT è possibile rappresentare anche flussi di controllo; anticiparli in un DFD comporta di fatto una duplicazione se si usa anche un modello dinamico.

## Errori tipici
- **Black Hole (Buco Nero o pozzo)**: un processo riceve input ma non produce alcun output verso l'esterno o verso un archivio.
- **Miracle (Generazione Spontanea)**: un processo genera flussi di output senza ricevere alcun input da cui ricavarli.
- **Gray Hole (Buco Grigio)**: il processo produce un output con attributi che non possono essere logicamente derivati dagli input ricevuti (es. riceve _Codice Articolo_ e genera _Fattura con Indirizzo Cliente_, senza consultare l'anagrafica clienti).
# Architettura a livelli
- Per gestire la complessità dei sistemi reali, i DFD vengono organizzati gerarchicamente a livelli progressivi di dettaglio:
	- **Livello 0 (Diagramma di Contesto)**: la vista ad altissimo livello. L'intero sistema è racchiuso in un unico processo numerato `0`. Non mostra alcun deposito dati interno, ma solo i flussi di dati in entrata e in uscita con le entità esterne. Serve a definire con precisione i **confini del sistema** (_system boundaries_).
	- **Livello 1**: "esplode" il processo `0` nei principali sotto-processi funzionali (numerati `1.0`, `2.0`, `3.0`, ecc.) e introduce per la prima volta i depositi dati necessari all'operatività interna.
	- **Livello 2 e successivi**: scompone ulteriormente processi complessi (es. il processo `2.0` diventa `2.1`, `2.2`, `2.3`). Si scende di livello finché ogni processo non è ridotto a una singola funzione elementare (_process specification_ o _minispec_).
- **Regola del Bilanciamento (Balancing):** Tutti i flussi di dati che entrano o escono da un processo a un livello $N$ devono apparire identici nei confini del diagramma di livello $N+1$ che lo scompone. Nessun dato può sparire o apparire dal nulla tra un livello e l'altro.

# Applicazioni #opt
Anche se nei moderni progetti software lo standard UML (diagrammi di sequenza, classi e casi d'uso) e BPMN hanno parzialmente soppiantato i DFD per la progettazione pura del codice, il Data Flow Diagram resta insostituibile in tre ambiti:
1. **Threat Modeling e Sicurezza (Metodologia STRIDE)**: Microsoft e l'industria della cybersecurity usano i DFD come base primaria per mappare i confini di fiducia (_Trust Boundaries_). Tracciando il percorso esatto dei dati non affidabili attraverso i processi, si identificano vulnerabilità di Spoofing, Tampering e Information Disclosure.
2. **Compliance e Privacy (GDPR / HIPAA)**: ideale per documentare i registri di trattamento dei dati personali, mostrando con trasparenza dove i dati sensibili entrano, quali processi li anonimizzano/trasformano e su quali server risiedono.
3. **Comunicazione con gli Stakeholder di Business**: data la semplicità visiva e l'assenza di sovrastrutture sintattiche complesse, un DFD di livello 0 e 1 viene compreso istantaneamente anche da figure non tecniche.
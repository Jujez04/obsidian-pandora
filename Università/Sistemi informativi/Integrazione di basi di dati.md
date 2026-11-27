- Se voi foste un CIO quali sarebbero le strategie per applicare la circolarità dell'informazione?

# Motivazioni
- Lo sviluppo di un sistema software è *incrementale* ed *evolutivo*, basando la propria realizzazione su tecnologie che possono anche emergere durante il corso del tempo
- In genere si punta a *coerenza*, *consistenza* e *veridicità* (problema del divario percettivo)
- Il raggiungimento di questo risultato necessita di *integrazione*, *pulizia* e *trasformazione* dei dati.

# Aspetti intensionali ed estensionali
- Intensionale: 'insieme delle regole, della struttura e della descrizione dei dati che formano la base di dati
- Estensionale: È il contenuto effettivo dei dati memorizzati nella base di dati in un preciso istante.
- Databricks: Data-warehouse e data lake devono essere compattati
- Le attività che si svolgono in una fase di integrazione sono diverse:
	- **Integrazione dello schema**: operazione a livello intensionale che rende consistenti gli schemi dei diversi moduli. Le operazioni che si fanno in questa attività sono:
		- **Analisi e normalizzazione**: si produce un insieme di schemi completi e consistenti
		- **Definizione dello schema rinconciliato**: i diversi schemi locali vengono fusi in un unico schema globalmente consistente
		- **Definizione del mapping**: sapere quale relazione intercorre tra concetti degli schemi sorgenti e dello schema rinconciliato.
	- **Trasformazione dei dati**: operazione a livello estensionale che trasforma i dati degli schemi locali nei dati dello schema globale.
	- **Pulizia dei dati**: operazione a livello estensionale che controlla ed elimina eventuali inconsistenze ed errori.

![[Pasted image 20261022155530.png]]

# Architettura per il SI integrato
![[Pasted image 20261025100107.png]]
- *Legacy*: è un software, un hardware o un processo informatico obsoleto ma ancora in uso in un'azienda o organizzazione. Solitamente viene ereditato in quanto svolge funzioni critiche e sostituirlo comporta costi elevati.
- Prima di parlare del processo d'integrazione, è utile sapere che le applicazioni del sistema operano su **viste** dello schema integrato che può essere:
	- **Virtuale**: viene definita solo la meta-conoscenza necessaria a ottenere le informazioni sullo schema globale
	- **Materializzato**: i dati vengono trasformati e memorizzati in versione duplicata. Questa soluzione viene utilizzata per esempio nei sistemi DW.
- A causa della complessità dell’operazione non è sempre possibile ottenere un unico schema globale; talvolta può essere sufficiente creare poche porzioni integrate a partire da molti schemi locali.
- **Master Data Management (MDM)**: l’integrazione è limitata ai dati aziendali critici

# Analisi e normalizzazione
- In questa fase il progettista deve confrontarsi con gli stakeholders per verificare la *completezza degli schemi locali*. In particolare deve notare se ci sono correlazioni involontariamente *omesse*, che possono essere state tralasciate oppure perché si sono create nel corso del tempo.
- Le *trasformazioni* **non** *devono introdurre nuovi concetti*, bensì esplicitare quelli ricavabili dai dati presenti nelle sorgenti operazionali. Questo implica che non ci devono essere informazioni aggiunte
- Lo **schema logico** ottenuto in questa fase deve rappresentare tutto il contenuto informativo di questa fase. Nel caso di assenza di uno schema tra le varie sorgenti deve essere ricavato mediante *reverse engineering*.
## Problemi da affrontare
- La fase di integrazione non si deve limitare a evidenziare le differenze di rappresentazione dei concetti comuni a più schemi, ma deve anche identificare l’insieme di concetti distinti e memorizzati in schemi differenti che sono correlati attraverso proprietà semantiche, dette *proprietà* *inter-schema*.
- Le **proprietà inter-schema** (o vincoli inter-schema) sono regole, relazioni o condizioni che stabiliscono un collegamento tra i dati contenuti in **database diversi** o in **schemi logici distinti** all'interno dello stesso sistema informatico.
![[Pasted image 20261028093645.png]]
- In questo caso per esempio non conviene aggiungere una sotto-categoria, semmai sarebbe da pensare in fase di integrazione
### Diversità di prospettiva
- Il punto di vista rispetto al quale diversi gruppi di utenti vedono uno stesso oggetto del dominio applicativo può differenziarsi notevolmente in base agli aspetti rilevanti ai fini della funzione a cui essi sono preposti.
- Ex: decidere se la suddivisione dei dipendenti in base ai progetti è utile nel definire l'organigramma aziendale.
![[Pasted image 20261025105347.png]]
### Equivalenza dei costrutti
- I formalismi di modellazione permettono di rappresentare uno stesso concetto utilizzando combinazioni diverse dei costrutti a disposizione.
- Ex: le diversità possono essere sintattiche, ciò implica che le modellazioni in tal caso sono equivalenti
- Molto spesso dipende dal contesto e si mira alle *performance*
![[Pasted image 20261025105522.png]]
- In questo caso dipende se bisogna fare tante modifiche o tante letture.
- Sistemi batch (finire tutto il lavoro il prima possibile) e interattivi (fare in modo che arrivi la prima risposta possibile) determinano la modellazione
### Incompatibilità delle specifiche
- Si verifica quando due schemi modellano una *stessa porzione del dominio applicativo e racchiudono concetti diversi*, **in contrasto tra loro**.
- Tali diversità derivano da scelte progettuali errate, come la scelta di nomi, tipi di dati e vincoli di integrità
![[Pasted image 20261025105744.png]]
## Concetti comuni
- Siano $R_1$ e $R_2$ rappresentazioni di uno stesso concetto
- Le relazioni che possono intercorrere tra le due rappresentazioni sono:
	- **Identità**: $R_1$  e $R_2$ coincidono e vengono utilizzati gli stessi costrutti per modellarli
	- **Equivalenza**: $R_1$ e $R_2$ non sono esattamente le stesse in quanto vengono utilizzati costrutti diversi (ma equivalenti), senza commettere nessun errore
	- **Comparabilità**: $R_1$ e $R_2$ non sono né identici né equivalenti, ma i costrutti utilizzati e i punti di vista dei progettisti non sono in contrasto tra loro
	- **Incompatibilità**: $R_1$ e $R_2$ sono in conflitto a causa dell'incoerenza delle specifiche
- **Conflitto**: quando $R_1$ e $R_2$ non sono delle rappresentazioni identiche tra loro
- **Equivalenza**: due schemi $R_1$ e $R_2$ sono equivalenti se le loro istanze possono essere messe in *corrispondenza uno-a-uno*. Ciò implica che a livello estensionale esistono sempre due insiemi di dati diversi che memorizzano le stesse informazioni.
![[Pasted image 20261025111319.png]]

## Concetti correlati
- A seguito dell’integrazione, molti concetti diversi ma correlati verranno a trovarsi nello stesso schema dando vita a nuove relazioni (*proprietà inter-schema*) che non erano percepibili in precedenza.
![[Pasted image 20261025111516.png]]

# Definizione dello schema riconciliato o Integrazione
## Fasi dell'integrazione
- Le sequenze di passi da seguire per ottenere uno schema globale partendo dagli schemi locali concordate in letteratura sono:
	- **Preintegrazione**: analisi e scelta di strategie di integrazione
	- **Comparazione degli schemi**: evidenziati conflitti e concetti correlati
	- **Allineamento degli schemi**: risolvere conflitti precedentemente identificati
	- **Fusione e ristrutturazione degli schemi**: gli schemi resi coerenti possono essere fusi in unico schema globale.
## Strategie di integrazione
![[Pasted image 20261027095645.png]]
- Indicano l'ordine con cui si procederà nell'integrazione degli schemi
- Si dividono in:
	- **Tecniche ennarie**: processo di integrazione considera più di due schemi contemporaneamente. Risulta tuttavia complesso.
		- **Tecniche a stella** (o *a un passo*): Tutti gli schemi parziali disponibili vengono integrati simultaneamente in un'unica operazione globale. Fornisce una visione globale immediata di tutte le entità e relazioni, ma la complessità concettuale e il numero di conflitti semantici/sintattici da risolvere contemporaneamente crescono in modo esponenziale all'aumentare del numero di schemi. Diventa facile se i concetti sono semplici
		- **Tecniche iterative**: Combina passi di integrazione con cardinalità variabile: alcuni gruppi di schemi possono essere integrati a blocchi di $k$ elementi ($k \ge 2$), e gli schemi risultanti possono essere ulteriormente integrati sia in modo binario che a gruppi. È la strategia più flessibile e realistica in contesti complessi, poiché consente di raggruppare insieme insiemi di schemi fortemente affini per dominio di business o area applicativa prima di integrarli con le restanti parti del sistema.
	- **Tecniche binarie**: il processo di integrazione considera sempre coppie di schemi. Può dare problemi se ci sono concetti sovrapposti gestiti diversamente
		- **Tecniche a scala**: Si parte integrando due schemi iniziali; il risultato parziale ottenuto viene poi integrato con un terzo schema, poi con un quarto, e così via in modo lineare fino all'esaurimento di tutte le viste. Se ho una sorgente più completa ogni volta allora tanto vale risolvere i problemi più importanti all'inizio della scala
		- **Tecniche bilanciate**: Gli schemi sorgente vengono divisi in coppie e integrati in parallelo a coppie a due a due; i risultati intermedi ottenuti vengono a loro volta accoppiati e integrati progressivamente secondo una struttura ad albero binario simmetrico. Mantiene il processo simmetrico e bilanciato, evitando che uno schema prevalga sugli altri.
- Normalmente si preferisce iniziare con gli schemi che costituiscono il cuore del sistema informativo
## Comparazione degli schemi
- Generalmente si individuano dei conflitti che possono appartenere a queste categorie:
	- **Conflitti di eterogeneità**: discrepanze dovute all'utilizzo di formalismi con diverso potere espressivo negli schemi sorgenti (es. ER e UML)
	- **Conflitti sui nomi**: causate da differenze nelle terminologie, come omonimie e sinonimi
	- **Conflitti semantici**: quando due schemi sorgenti modellano la stessa porzione di mondo reale a un diverso livello di astrazione e dettaglio.![[Pasted image 20261027101025.png]]
	- **Conflitti strutturali**: causate da scelte diverse nella modellazione di uno stesso concetto, oppure dall'applicazione di differenti vincoli di integrità.
		- **Conflitti di tipo**: lo stesso concetto è modellato con due costrutti diversi
		- **Conflitti di dipendenza**: due o più concetti sono correlati con dipendenze diverse in schemi diversi
		- **Conflitti di chiave**: lo stesso concetto utilizza identificatori diversi in schemi diversi
		- **Conflitti di comportamento**: diverse politiche di cancellazione/modifica dei dati vengono adottate per uno stesso concetto in schemi diversi

## Allineamento degli schemi
- Con *allineamento* si intende l'eliminazione dei conflitti negli schemi tramite l'applicazione di trasformazioni agli schemi sorgenti o allo schema riconciliato temporaneamente definito applicando:
	- *Cambio dei nomi*
	- *Cambio degli attributi*
	- *Cambio delle dipendenze funzionali*
	- *Cambio dei vincoli esistenti sugli schemi*
- Qua si deve fare caso anche unità di misura e percentuali
- La soluzione dovrà essere fatta in modo da evitare perdite di informazioni
- In caso di incertezza è preferibile avvantaggiare gli schemi si ritengono più centrali nella definizione dello schema riconciliato
- Strategia migliore: *binaria a scala*, in quanto consente di partire con gli schemi ritenuti più importanti
- Non sempre i conflitti possono essere risolti poiché possono anche derivare da inconsistenze presenti di base nel sistema informativo; in tal caso discutere con gli utenti del dominio applicativo


## Fusione e Ristrutturazione degli schemi
- Si mirano ai seguenti principi
	- **Completezza**: rendere le proprietà inter-schema evidenti
	- **Minimalità**: evitare ridondanze e duplicazioni
	- **Leggibilità**: facilita le successive fasi di progettazione

## Esempio
![[Pasted image 20261027103247.png]]
![[Pasted image 20261027103303.png]]
![[Pasted image 20261027103315.png]]
![[Pasted image 20261027103330.png]]
Per minimalità:
![[Pasted image 20261028103839.png]]
# Definizione del mapping
- Una volta definito lo schema riconciliato, si passa alla definizione del *mapping*, ossia l'insieme di corrispondenze tra gli elementi presenti negli schemi sorgenti e quelli dello schema globale.
- Le funzioni di mapping vengono utilizzate ogni qualvolta si debba eseguire sui database locali una interrogazione espressa sullo schema rinconciliato.
- Ne esistono di due tipi:
	- **Global-As-View (GAV)**
	- **Local-As-View (LAV)**

## Global-As-View (GAV)
- A ogni concetto dello schema globale deve essere associata una vista il cui significato è definito in base ai concetti che risiedono sugli schemi sorgenti.
- **Riduce l’estensibilità** dello schema riconciliato poiché l’aggiunta di una nuova sorgente richiederà la modifica di tutti i concetti dello schema globale che la utilizzano
- *Unfolding*: Facilita la modalità di definizione delle interrogazioni poiché per capire quali concetti degli schemi sorgenti sono coinvolti sarà sufficiente sostituire a ogni concetto dello schema globale la definizione della vista che lo definisce rispetto ai concetti sugli schemi locali.
![[Pasted image 20261027111110.png]]
- Al posto dell'inner join bisognerebbe fare un full outer join :)
## Local-As-View (LAV)
- Lo schema globale è espresso indipendentemente dalle sorgenti, i cui concetti saranno invece definiti come viste sullo schema globale
- Interrogare lo schema globale è difficile perché il sistema non sa direttamente da quali tabelle locali pescare i dati. Richiede algoritmi complessi di riscrittura (**query rewriting** / _answering queries using views_) per capire quali viste locali combinare insieme al fine di rispondere all'interrogazione globale.
- È ideale in sistemi aperti ed evolutivi. Se si aggiunge una nuova sorgente, basta definire una nuova vista locale che mappa i suoi campi sullo schema globale, senza dover toccare né lo schema globale né le altre sorgenti già configurate.
![[Pasted image 20261027111305.png]]
- La definizione delle sorgenti locali è semplice ma come faccio a esprimere le interrogazioni dallo schema globale a quello locale ? Questo è possibile ma molto difficile
- DICO: software di presentazione di data warehouse
# Data Cleaning
- Data Cleaning o Data Cleansing o Data Scrubbing si intende l'insieme delle operazioni atte a garantire la consistenza e correttezza dei dati presenti nel livello riconciliato
- Le principali cause di inconsistenza sono:
	- Errori di battitura
	- Differenze di formato dei dati dello stesso campo
	- Inconsistenza tra valori e descrizione dei campi
	- Inconsistenza tra valori di campi correlati
	- Informazioni mancanti
	- Informazioni duplicate
- Esistono tecniche per verificare la correttezza dei dati
## Tecniche basate su dizionari
- Si verifica la correttezza dei valori di un campo tramite tabelle di riferimento (*look-up table*) e dizionari che contengono abbreviazioni e sinonimi.
- Applicabili quando il dominio è conosciuto e limitato
- Le tecniche basate su dizionari possono essere applicate anche a più campi contemporaneamente al fine di verificare eventuali inconsistenze tra due campi correlati.

## Tecniche basate sulla similarità
- Si utilizzano quando in due database identificano diversamente lo stesso concetto e di conseguenza non esiste un identificatore primario che permetta di fare join e di conseguenza si prosegue con identificatori comuni.
- Si esegue un *join approssimato* se si utilizzano identificatori che non sono primari (ex: cognome, nome, indirizzo cliente)
- **Purge/merge problem**: due istanze non disgiunte dello stesso schema devono essere fuse.![[Pasted image 20261027114158.png]]
- **Funzioni di similarità**: si comparano le sottostringhe di $A_i \in A$ e $B_i \in B$, calcolando la similarità in base alla formula ricorsiva, dove la comparazione tra i sottocampi elementari vengono basati sul confronto di pattern ben definiti. $$affinity(A,B) = \frac{1}{|A|} \sum_{i=1}^{|A|}max_j (affinity(A_i,B_j))$$
- **Tecniche basate su gruppi di regole**: normalmente vengono valutate caratteristiche quali la edit-distance tra due stringhe o la differenza di valore tra due campi numerici.
- **Similarità vettoriale**: Un DBMS vettoriale consente la ricerca di somiglianza semantica su dati complessi. Risulta utile in caso di descrizioni. I vectorDB sono oggi alla base dei sistemi RAG. Ogni record viene convertito in un vettore chiamato “*embedding*”.![[Pasted image 20261027115250.png]]
- **Reranking con LLM**: Un prompt adeguato può permettere di codificare delle regole di business che permettano di disambiguare i candidati più simili.![[Pasted image 20261027115331.png]]

# Master Data Management
- *Master data*: insieme dei dati che identificano e descrivono entità fondamentali per il business di un'azienda
- **Master Data Management**: insi

## Scelta dei master data
- Le anagrafiche sono le più importanti perché permettono la circolarità dell'informazione
- Per esempio esiste il brand e il marchio che possono tirare avanti il business dell'azienda. Ex: Technogym
- Si basa su diversi criteri:
	- Tipologia dei dati
		- *Non strutturati*: blocchi testuali
		- *Transizionali*: ordini, fatture
		- *Metadati*: glossario aziendale, attributi
		- *Gerarchici*: relazioni tra concetti
		- *Master*: persone, cose, luoghi e concetti
	- Caratteristiche
		- *Quantità*: in un'architettura per la gestione dei MD deve essere elevata
		- *Lifetime*: i MD sono meno volatili dei dati transazionali
		- *Valore*: maggiore il valore dell'informazione, più tende a essere un MD
		- *Complessità*: informazioni complesse non favoriscono la costruzione di un meccanismo di MDM
		- *Riusabilità*: un concetto riusato in più contesti aziendali con elevata probabilità avrà bisogno di una gestione di un sistema MDM
		- *Centralità*:  i MD sono centrali a più applicazioni e vengono da essi creati, modificati, letti e cancellati
![[Pasted image 20261028093233.png]]
## Gestione dei master data
- I MD hanno un forte impatto sui processi aziendali e ne determinano politiche e organizzazione, inoltre quelli basati su MDM devono essere mantenuti nel tempo
- E' iterativo il processo?
- Le principali fasi di un progetto di MDM sono:
	1. **Identificare le sorgenti dati**: ovvero i vari DB che memorizzano MD
	2. **Specificare produttori e consumatori**: tipo e numero di operazioni fatti dagli attori che operano sui MD incidono sulla complessità della gestione
	3. **Raccogliere metadati sui MD**: definire uno standard, sia sintattico che semantico
	4. **Identificare gli esperti dei MD**: in particolare chi è in grado di descrivere i MD e renderli compatibili con le altre versioni (solitamente chi sta in alto nella gerarchia)
	5. **Definire il processo di data-governance e il relativo gruppo di lavoro**: il gruppo deve avere le competenze e l'autorità per la gestione dei MD
	6. **Sviluppare un modello di gestione dei MD**: la fase più importante, si decide architettura, formato dei MD, quali record saranno esclusi/inclusi
	7. **Scegliere uno strumento/tool**: nel caso in cui il sistema non viene sviluppato internamente, occorre trovare un tool in base al tipo di MD gestiti e le scelte fatte negli step precedenti
	8. **Progettare e implementare l'infrastruttura di gestione**: l'infrastruttura dovrà essere in grado di collezionare e rendere disponibili gli MD alle applicazioni già esistenti
	9. **Generare e testare i MD**: parte iterativa in cui si fa evolvere la soluzione grezza per coprire tutte le specificità delineate negli step precedenti
	10. **Modificare le applicazioni basate su MD**: modificare le interfacce con cui si espongono le varie applicazioni
	11. **Definire il processo di manutenzione**: il processo deve mettere gli esperti nelle condizioni di verificare la correttezza e la qualità degli MD. Inoltre si deve definire il processo di attuazione in caso di future manutenzioni. Si scelgono anche dei tool in grado di:
		- Mostrare inconsistenze
		- Mostrare sorgente e utente responsabile dell'incoerenza
		- Suggerire possibili correzioni

## Architetture per MDM
### A registro
- L'hub centrale non memorizza tutti gli attributi del dato, ma solo un **indice/registro** che collega le chiavi e i riferimenti ai record equivalenti distribuiti sui sistemi sorgente
- La proprietà del dato rimane interamente alle applicazioni sorgenti, che gestiscono e modificano i propri dati in autonomia.
- Nessun dato o correzione viene riversato verso le sorgenti.
- L'impatto sui sistemi è minimo, non richiede modifiche strutturali ai processi operazionali.
- Il miglioramento effettivo della qualità del dato è limitato e la logica per ricostruire una visione complessiva è onerosa, poiché richiede interrogazioni distribuite su più basi di dati.
![[Pasted image 20261028102103.png]]

### Consolidamento
- I dati provenienti dalle sorgenti vengono estratti, puliti, riconciliati e materializzati fisicamente in un *hub centrale* che contiene la versione integrata di riferimento (**golden record**).
- Questo permette alle applicazioni di mantenere la proprietà dei dati
- L'aggiornamento dei MD nel hub è **asincrono** agli eventi di modifica che avvengono nelle altre sorgenti
- Lo scopo principale è alimentare l'analisi direzionale e il data warehousing (sistemi analitici/BI), garantendo una vista unificata a posteriori.
- I MD possono essere scaricati periodicamente sulle applicazioni che li utilizzano come versione di riferimento
![[Pasted image 20261028102044.png]]


### Coesistenza
- Evoluzione del consolidamento; L'hub centrale memorizza la versione integrata dei master data, ma il flusso diventa **bidirezionale**: i dati ripuliti e consolidati nell'hub vengono periodicamente riversati di ritorno sui database sorgente.
- L'ownership del dato è *condivisa*: le sorgenti possono ancora inserire e modificare dati localmente, ma devono poi armonizzarsi con le rettifiche dell'hub.
- La sincronizzazione è *asincrona*: L'hub non garantisce l'allineamento istantaneo in tempo reale, ma un allineamento periodico basato sulle policy delle singole applicazioni.
![[Pasted image 20261028102117.png]]
### Transazionale
- L'hub centrale diventa il punto unico e autoritativo per la creazione, lettura, aggiornamento e cancellazione (CRUD) dei Master Data. Le applicazioni operazionali non gestiscono più i master data in locale in modo disgiunto, ma interagiscono direttamente con l'hub (tramite API/web services e pattern Publish-Subscribe).
- L'ownership del dato appartiene al hub centrale
- La sincronizzazione è **sincrona**: regolata da politiche transazionali (meccanismi di locking e validazione di qualità in tempo reale prima del commit) per garantire consistenza assoluta e circolarità dell'informazione.
- L'impatto sui sistemi è elevato: richiede la riscrittura o l'adattamento profondo delle logiche applicative e assume che i nodi siano sempre connessi in rete.
![[Pasted image 20261028102131.png]]

## Esempio AUSL


# Evoluzione dell'integrazione dati e Nuove Metodologie
- Dagli anni '70 si persegue l'integrazione per garantire qualità del dato, ottimizzazione dei processi e capacità di analisi
- **Complessità moderna**: Con il procedere della digitalizzazione aumenta il numero di sorgenti dati e diventa sempre più complesso mantenerle integrate e sincronizzate
- Limiti delle piattaforme dati centralizzate:
	- Difficoltà ad armonizzare e aggiornare modelli monolitici al proliferare delle sorgenti.
	- Proliferazione e sovrapposizione di pipeline di elaborazione difficili da gestire e da mantenere aggiornate per un team IT centrale.
	- Team IT organizzati per _competenza tecnologica_ anziché di _business_, con conseguente divario di comprensione e sfiducia nei dati.
	- Ambiguità sulla reale _ownership_ del dato.

## Data Fabric
![[Pasted image 20261031181331.png]]
- *Framework architetturale* unificato che consente accesso e condivisione dei dati in ambienti distribuiti/multi-cloud superando la frammentazione a silos.
- Funzionamento:
	- Consente un quadro di gestione dei dati unico e coerente, che permette l'accesso ai dati e l'elaborazione senza soluzione di continuità a sorgenti che altrimenti sarebbero a silo
	- Identifica e collega le sorgenti sul posto o tramite consolidamento dove opportuno, scoprendo relazioni rilevanti per il business tra le sorgenti dati disponibili
- Combina le *tecnologie* chiave di *gestione dei dati*, come il catalogo dei dati, la governance dei dati, l'integrazione dei dati, il pipeline dei dati e l'orchestrazione dei dati

Gartner, 2019 https://www.gartner.com/en/newsroom/press-releases/2019-02-18-gartner-identifies-top-10-data-and-analytics-
technolo
Gartner, 2021 https://www.gartner.com/smarterwithgartner/data-fabric-architecture-is-key-to-modernizing-data-management-and-
integration
K2View Whitepaper: What is a Data Fabric? The Complete Guide, 2021

- Esempio: sistema bancario basato su ambienti ibridi e multi-cloud
	- Invece di costruire manualmente pipeline ETL punto a punto per ogni nuova richiesta, l'architettura Data Fabric opera collegando e orchestrando i dati attraverso il proprio stack a più livelli:
		- **Rilevamento e catalogazione continua (_Augmented Data Catalog_):**
		    - I connettori del Data Fabric scansionano costantemente le sorgenti distribuite senza dover spostare fisicamente tutti i dati a priori.
		    - Il catalogo indicizza schemi, formati, log di accesso e statistiche di utilizzo, generando metadati dettagliati in modo automatizzato.
		- **Rappresentazione semantica delle relazioni (_Knowledge Graph_):** 
		    - Il sistema correla entità provenienti da sorgenti eterogenee (es. l'`ID_Cliente` del CRM con il `Codice_Fiscale` del sistema mutui e il `Device_ID` dei log web) arricchendole di significato.
		    - Il grafo semantico identifica automaticamente che questi record appartengono allo stesso soggetto, rilevando relazioni di business nascoste tra i diversi silos.
		- **Attivazione dei metadati e raccomandazione (_Metadata Activation & Recommendation Engine_):**
		    - Il motore analizza il modo in cui i dati vengono interrogati e rileva schemi operativi ricorrenti.
		    - Quando un data scientist o un'applicazione richiede il profilo finanziario del cliente, il sistema analizza i metadati attivi e raccomanda la pipeline o la query federata ottimale per recuperare l'informazione, garantendo al contempo le policy di sicurezza e privacy.
		- **Integrazione e fornitura del dato (_Data Preparation, Integration & DataOps_):**
		    - Se i dati transazionali recenti risiedono on-premise, il Data Fabric può decidere se accedervi direttamente sul posto (_in situ_) tramite virtualizzazione oppure consolidarli temporaneamente laddove la latenza o il volume lo richiedano.
		    - Il dato pulito, normalizzato e arricchito viene esposto all'applicazione di scoring creditizio nel formato e con la frequenza richiesta, supportando sia le decisioni analitiche che i flussi operazionali.
## DataOps
- Estensione del paradigma DevOps (Dev + QA + Ops) al ciclo di vita del dato. Interseca _Data Engineering_, _Data Integration_, _Data Quality_ e _Data Security/Privacy_.
- **Scopo:** Pratica collaborativa per migliorare comunicazione, integrazione e automazione dei flussi tra produttori e consumatori di dati.
- **Principi cardine:**
    - Stabilire metriche e misure di avanzamento/prestazione in ogni fase.
    - Automatizzare il maggior numero possibile di fasi del flusso di dati
    - _Governance-as-code_ integrata e continua.
    - Progettazione modulare orientata alla scalabilità e all'estensibilità.


## Data Mesh
- Approccio socio-tecnico **decentralizzato** per gestire e accedere a dati analitici su larga scala.
- Sposta l'enfasi dalla centralizzazione tecnica al cambiamento organizzativo, di persone e di processi (strutturazione a domini), indirizzando le organizzazioni a diventare maggiormente Data Driven e cambiando il modo in cui queste organizzano i propri team e le proprie architetture dati
- Data Architect e Data Science
- Principi del Data Mesh:
	- **Domain Ownership**: La responsabilità delle pipeline (ETL) di ingestione, pulizia e trasformazione è delegata ai team di **dominio**, che possiedono le competenze analitiche e di contesto (data scientist decentralizzati) e possono utilizzare i dati a fini analitici e operazionali.
	- **Data as a product**: I dati esposti da ogni dominio sono trattati come prodotti per consumatori interni/esterni. Inoltre si cerca di ridurre il numero di *dark data*, ovvero tutti quegli asset informativi che non vengono mai utilizzati e sono costosi da mantenere
		- _Discoverable & Addressable_ (es. reperibili via marketplace tramite endpoint univoci).
		- _Understandable, Trustworthy & Accessible_ (semantica chiara, verifiche di qualità, contratti di dati e lineage).
		- _Interoperable & Secure_ (formati aperti, policy globali di sicurezza/privacy).
	- **Federated Computational Governance**: Equilibrio tra l'autonomia decentralizzata dei singoli domini e la definizione globale di standard di interoperabilità, sicurezza e policy automatizzate a livello computazionale.
		- La decentralizzazione in domini e l’ampia autonomia dei team nella definizione di Data Product
		- La definizione di un set di **regole globali** da applicare a tutti i Data Product e alle loro interfacce di comunicazione.
	- **Self-Serve Data Platform**: una Data Platform che espone servizi e funzionalità che abilitano i team di dominio a gestire il ciclo di vita dei Data Product in autonomia.
- Quando NON adottare una Data Mesh:
	- Se trattato come mero software/prodotto da acquistare a scaffale anziché trasformazione organizzativa.
	- In assenza di una Data Strategy chiara o di reali use-case a valore nei domini.
	- Non emergono business-case (data driven) che portano valore alla business unit (dominio)
	- In contesti senza cultura decisionale bottom-up o con grave carenza di figure specializzate distribuite (data engineer/analyst/scientist).
- Differentemente da Data Fabric:
	- Decentralizza e mantiene l'autonomia del dominio dei dati
	- Si basa sul processo, mentre Data Fabric si basa sui metadati
- Data Fabric e Data Mesh sono framework architetturali, quindi non si escludono a vicenda e devono essere adattati alle esigenze, dati e terminologie.

## Integrazione di basi di dati in contesti di silos #opt (Da sapere solo le cose più importanti)
![[Pasted image 20261126201403.png]]
- Problema principale: lo stesso cliente compare in sistemi con chiavi diverse, scritto in modo diverso l'uno dall'altro e aggiornati in momenti diversi
- *Data silos*: sono raccolte isolate di informazioni conservate in sistemi o reparti separati, che non comunicano tra loro e restano poco accessibili al resto dell'organizzazione. -> spesso sono dovuti a isolamenti tecnologici(database incompatibili o limiti software/hardware), isolamento organizzativo e crescita rapida/fusioni
![[Pasted image 20261126204343.png]]

- **ETL(Extract, Transform, Load)**:  un flusso di lavoro automatizzato che estrae dati grezzi dai sistemi di origine, li rielabora e li pulisce, per poi scriverli in un sistema di destinazione, come un data warehouse o un data lake -> preferibile per dati sensibili o per applicare regole rigide
![[Pasted image 20261126205207.png]]
- **ELT (Extract, Load, Transform)**: un flusso automatizzato che estrae i dati dalle sorgenti, li carica subito in un repository centrale (come un data warehouse o un data lake) e li trasforma solo in un secondo momento, direttamente nella destinazione
![[Pasted image 20261126210044.png]]
- Come verranno integrati i dati? Tramite schema
	- **Materializzato:** I dati integrati vengono fisicamente salvati in tabelle persistenti (approccio usato in questo laboratorio, tipico dei Data Warehouse).
	- **Virtuale:** I dati restano sulle sorgenti e vengono integrati al volo tramite viste (es. approcci GAV/LAV).
![[Pasted image 20261126211554.png]]
- Nel laboratorio è stato usato *DuckDB*: 
	- database analitico compilato tramite Python
	- Legge CSV, JSON, Parqurt
	- Può essere interrogato tramite SQL
	- Utilizza pandas 
	- Open source (MIT)

| **Caratteristica**       | **SQLite**                 | **DuckDB**                      | **PostgreSQL / SQL Server** |
| ------------------------ | -------------------------- | ------------------------------- | --------------------------- |
| **Workload**             | OLTP (transazionale)       | **OLAP (analitico)**            | Entrambi                    |
| **Storage**              | Per righe (_Row-oriented_) | **Per colonne (_Columnar_)**    | Per righe                   |
| **Architettura**         | In-process (nessun server) | **In-process (nessun server)**  | Client-server               |
| **Lettura file esterni** | No                         | **Nativa (CSV, JSON, Parquet)** | Tramite tool/estensioni     |
- **Architettura Medallion:** Pattern standard per strutturare le pipeline a livelli progressivi di qualità:
	- **Bronze (Staging):** Dati grezzi ingeriti dalle sorgenti (`stg_crm`, `stg_web`, `stg_fid`, `stg_pos`).
	- **Silver (Integrazione):** Dati puliti, normalizzati, deduplicati e riconciliati (`vendite_ok`, `scarti`, `match_clienti`, `xref`).
	- **Gold (Presentazione):** Modello dimensionale pronto per l'analisi (`dim_cliente`, `fatti_vendite`), spesso esportato in formato colonnare (Parquet).
### Livello Intensionale (Integrazione dello Schema)
Riguarda i metadati e la struttura delle tabelle:
- **Sinonimie:** Nomi diversi per lo stesso concetto (`qta` vs `quantita`; `comune` vs `citta`).
- **Omonimie:** Stesso nome/sigla con significati diversi (`BO` indica sia la provincia di Bologna che il punto vendita).
- **Conflitti di chiave:** Identificativi incompatibili tra sistemi (`W10242` nel CRM vs `F20072` nelle carte fedeltà).
- **Conflitti di tipo/struttura:** Strutture gerarchiche annidate (`indirizzo {...}` in JSON) contro strutture piatte (colonne CSV).
- **Schemi locali diversi:** File della stessa categoria con colonne in ordine diverso o attributi extra (es. la cassa di Ravenna ha la colonna `commesso`, Modena non ha `ora`).
### Livello Estensionale (Pulizia dei Dati / Data Cleaning)
Riguarda i valori delle istanze:
- **Errori di battitura:** Refusi nei campi testuali (`MZZA CHIARA`).
- **Formati eterogenei:** Rappresentazioni multiple dello stesso dominio (5 formati diversi di numeri di telefono, `Bologna` / `BO` / `Bologna_`, separatore decimale `,` vs `.`, date ISO vs `gg/mm/aaaa`).
- **Violazione di dipendenze tra campi correlati:** Incoerenze tra attributi legati funzionalmente (es. `comune` non coerente con `provincia`).
- **Informazioni mancanti:** Valori `NULL` o assenti (email mancante in metà delle tessere).
- **Informazioni duplicate:** Stessa entità reale registrata più volte nello stesso sistema o su canali diversi.


### Pipeline operativa
#### Profilazione ed Extract/Staging
- **Data Profiling:** Uso di `DESCRIBE` (schema sniffing) e `SUMMARIZE` per ottenere in un solo passaggio min, max, valori distinti e percentuale di `NULL` su ogni colonna.
- **Ingestione multi-file con schemi eterogenei:**
	- Un `glob` ingenuo (`read_csv('negozi/*.csv')`) fallisce se l'ordine o il numero di colonne cambia.
	- Soluzione: `read_csv('negozi/*.csv', union_by_name=true, filename=true)` allinea le colonne per nome (inserendo `NULL` dove assenti) ed espone il nome del file per derivare l'attributo `negozio`.
- **Gestione tipi e strutture annidate:** In DuckDB, le `STRUCT` JSON si appiattiscono con la dot-notation (`indirizzo.*`). I codici con zeri iniziali (es. `CAP = 00184`) vanno forzati esplicitamente come stringhe (`VARCHAR`) per evitare il cast automatico a intero.

#### Lookup e Gestione dei Record Orfani
- **Mapping GAV (Global-as-View):** Unificazione dei flussi di vendita di canali diversi (Web e POS) tramite `UNION ALL BY NAME`.
- **Lookup su dizionario (Catalogo Master):**
	- `INNER` / `LEFT JOIN` su `catalogo_prodotti` per isolare le vendite valide (`vendite_ok`).
	- `ANTI JOIN` per intercettare i **record orfani** (`scarti_vendite`, righe con `sku` non trovato in catalogo).
- **Distinzione degli orfani:**
	- **Orfani "sporchi":** Errori sintattici sulla chiave (`ac-103`, `RC-107` , `EB102`). Si recuperano tramite normalizzazione preventiva con regex (`regexp_replace(upper(trim(sku_raw)), '^([A-Z]{2})-?([0-9]{3})$', '\1-\2')`).
	- **Orfani veri:** Prodotti effettivamente assenti dal master (`EB-099`, `KIT-PROMO-25`). Vanno isolati in **quarantena** (tabella scarti) e segnalati a monte.

#### Qualità, Standardizzazione e Deduplica
- **Standardizzazione tramite Macro SQL:** Incapsulamento di regole di pulizia riutilizzabili (`norm_txt` con `strip_accents`, `token_sort`, `norm_tel` per uniformare i prefissi internazionali e rimuovere spazi/trattini).
- **Controllo campi correlati:** Join su tabella di riferimento (`province.csv`) per validare la coerenza `comune` $\rightarrow$ `provincia`.
- **Fuzzy Lookup (Join approssimato):** Per valori sporchi non catturabili con match esatto (es. `REGGIO E.`, `REGGIO NELLEMILIA`), si usa una metrica di similarità tra stringhe come `jaro_winkler_similarity` combinata con una soglia e una tabella di `alias` per i casi ambigui o sigle (`BO`).
- **Purge / Merge Problem (Deduplica intra-sorgente):**
	- Identificazione dei duplicati nel CRM tramite window function e filtro diretto con `QUALIFY row_number() OVER (...) = 1`.
	- **Regola critica di integrità referenziale:** Quando si elimina un record duplicato, occorre mantenere una tabella di mapping (`crm_alias`) per **rimappare i fatti** (gli ordini collegati al vecchio ID non devono diventare orfani).

#### Entity Resolution / Record Linkage
Riconciliazione probabilistica/euristica tra anagrafiche prive di chiave comune (Clienti Web vs Carte Fedeltà):
1. **Blocking:** Riduce la complessità quadratica del prodotto cartesiano ($N \times M = 78.600$ coppie) confrontando solo i record che condividono una chiave di blocco (es. `provincia OR tel OR email`, scendendo a $12.113$ coppie mantenendo il $98.5\%$ dei veri match).
2. **Confronto:** Calcolo delle feature di similarità sui singoli attributi (nome, telefono, email, CAP).
3. **Classificazione (Regole di Hernandez / Punteggio pesato):** Combinazione lineare pesata delle similarità confrontata con una soglia $\tau$ (es. $0.5 \cdot \text{nome} + 0.2 \cdot \text{tel} + 0.2 \cdot \text{email} + 0.1 \cdot \text{cap} \ge 0.55$).
4. **Valutazione su Gold Standard:** Un campione verificato manualmente (130 coppie) serve _esclusivamente_ per misurare le prestazioni, mai nelle regole di decisione:
    - $\text{Precision} = \frac{TP}{TP + FP}$ (affidabilità dei match trovati).
    - $\text{Recall} = \frac{TP}{TP + FN}$ (completezza rispetto ai match reali).
    - $F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}$ (media armonica).

#### Master Data Management (MDM) e Caricamento Incrementale
- **Tabella Cross-Reference (`xref`):** Cuore del sistema MDM; associa un identificativo univoco globale (`id_master`, es. `C00001`) alle diverse chiavi locali dei sistemi sorgente (`id_cliente`, `cod_tessera`).
- **Golden Record (`dim_cliente`):** Rappresentazione consolidata dell'entità ottenuta applicando **regole di sopravvivenza** (_survivorship rules_) campo per campo (es. nome dal CRM perché più accurato; telefono e comune dalla fonte con timestamp più recente tramite `CASE WHEN ... THEN coalesce(...)`).
- **Aggiornamento Incrementale:** Uso di `MERGE INTO` per garantire l'**idempotenza** della pipeline nei caricamenti periodici successivi (aggiornando i record esistenti e inserendo solo i nuovi senza duplicare).



#opt Integration Patterns
- ... Guarda slide salesforce
- Data Virtualization
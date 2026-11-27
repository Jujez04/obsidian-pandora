- Uno **studio di fattibilità** (o _feasibility study_) è un'analisi preliminare e strutturata volta a valutare se un'idea, un progetto o un investimento sia concretamente realizzabile, sostenibile e vantaggioso prima di allocarvi tempo, risorse e capitali.
- Si cerca di non fare uno studio di fattibilità in quanto comporta costi realizzativi molto onerosi
- Solitamente al posto di uno studio di fattibilità si sviluppa un *Proof of concept*
- Non ha una forma generale, solitamente la struttura che possiede è questa:
	1. Analisi della situazione attuale
	2. Progetti di massima
	3. Analisi dei rischi
	4. Il progetto proposto
	5. Analisi costi benefici
	6. Raccomandazioni per le fasi realizzative
- Un'alternativa è ottenuta per esempio invertendo il punto 4 con il 5
- Risulta indispensabile delineare con esattezza i margini decisionali:
![[Pasted image 20261014093425.png]]
- Il quanto alto deve essere questa visione dipende dal fattore incertezza

# Analisi della situazione attuale (as-is)
- L’analisi della situazione attuale evidenzia le caratteristiche del sistema informativo su cui deve essere realizzato il progetto relativo allo studio di fattibilità e si articola sui seguenti punti:
	1. **Il contesto dello studio:** Inquadra il progetto nella visione strategica di sviluppo del SI (servizi, organizzazione, tecnologia) e nel piano di informatizzazione.
	2. **Descrizione della problematica:** Illustra i problemi o le opportunità che hanno generato il progetto, definendone rilevanza, confini ed esigenze degli utenti interni ed esterni.
	3. **Descrizione della situazione attuale del SI:** Rappresenta i processi coinvolti, i flussi informativi, la struttura organizzativa, l'utenza e l'attuale livello di automazione.
	4. **Analisi e diagnosi della situazione attuale:** Esamina criticamente i processi per individuare le cause dei problemi, collocarle sulle componenti di servizio e misurare lo stato attuale tramite apposite metriche.
	5. **Identificazione dei vincoli:** Descrive le condizioni non modificabili, come il quadro normativo e i vincoli temporali, economici o organizzativi.
	6. **Definizione degli obiettivi di progetto:** Esprime gli obiettivi in forma quantitativa (costi, tempi, caratteristiche del servizio) mettendoli in relazione con le cause dei problemi individuate.
- Parte di queste informazioni devono già essere presenti in azienda poiché elaborate durante la fase di pianificazione

# Progetto di massima (to-be)
- I progetti di massima consistono in una descrizione generale del sistema informativo previsto e comprendono:
	- La *definizione dei requisiti*, ossia delle condizioni che il sistema deve soddisfare,
	- Le *specificazioni del sistema*, ossia una descrizione del sistema proposto in termini di proprietà,
	- L’indicazione delle principali *modalità di realizzazione*
- Deve essere incluso nel sistema informativo
- Caratteristiche principali
	- Il livello di dettaglio dovrà essere astratto in maniera tale da mantenere un *elevato livello di aggregazione*, in quanto alcune parti del SI non saranno scomposte e descritte
	- Dovranno essere studiate le *funzionalità principali*
	- Lo studio deve *riguardare la porzione più importante del SI che presenta criticità*  
- Uno studio di fattibilità può comprendere più progetti di massima, ciascuno dei quali rappresenta un’alternativa progettuale per la soluzione del problema o per il miglioramento del servizio.
- Cosa deve essere indicato:
	- **Requisiti della soluzione**: Evidenziano il dettaglio del processo reingegnerizzato, gli interventi sulle componenti non informatizzate, le eventuali modifiche normative e i requisiti del SI (informazioni trattate, funzioni informatizzate, modalità di lavoro, requisiti architetturali e di qualità).
	- **Specifiche generali del sistema:** Definiscono, valutando eventuali alternative, le specifiche applicative (architettura dei dati, architettura applicativa, interfaccia utente) e le specifiche tecnologiche (architettura tecnologica, ambienti e strumenti di sviluppo).
	- **Modalità di realizzazione:** Indicano le scelte su realizzazione interna o acquisizione sul mercato, riuso di componenti esistenti, avvio del sistema, esercizio/manutenzione e formazione/assistenza agli utenti.
- **Architettura funzionale**: dove stanno le varie funzioni (non fisicamente ma in maniera organizzativa) -> deve essere all'interno delle specifiche del sistema, comprendendo: 
	- Sequenza delle operazioni
	- La visione statica: quali informazioni porta
	- La visione architetturale: come viene integrata nell'infrastruttura
- Subito dopo c'è il *deployment* e a quel punto viene realizzata un'**architettura tecnologica** -> deve essere incluso nelle modalità di realizzazione
# Analisi del rischio
- Il rischio di progetto è legato a eventi che possono causare mancata conclusione, prodotti difettosi, lievitazione dei costi, allungamento dei tempi o difficile integrazione con il resto del SI. 
- Si articola in tre fasi:
	- **Individuazione dei fattori di rischio:** Si dividono in fattori legati alla *complessità* (complessità gestionale, dimensione del progetto, altri fattori specifici) e all'*incertezza* (incertezza sui requisiti, innovazione tecnologica).
	- **Definizione del rischio di progetto:** Per ogni fattore si definiscono parametri quantificabili (es. numero di persone, mesi-uomo, impegno economico, utilizzo di nuovo hardware/software, stabilità dei requisiti) e si assegna un livello di rischio (alto, medio, basso) per determinare la classe di rischio delle varie fasi e dell'intero progetto.
	- **Modalità di gestione del rischio:** Definisce le azioni per mitigare i rischi attraverso l'individuazione degli elementi critici (su cui concentrare risorse e controlli), la segmentazione del progetto, la fissazione di punti di controllo in corso d'opera e la definizione di punti di arresto (per interrompere il progetto prima che causi danni maggiori).
- Per esempio se il progetto richiede una conoscenza più approfondita di una tecnologia nuova, in quel caso conviene affidarsi a un'azienda di consulenza informatica.
- In generale bisogna avere metodi per esplicitare i fattori di rischio

## Definizione del rischio di progetto
- L’esito di tale quantificazione consiste nell’attribuzione alle varie fasi del progetto di un’appropriata **classe di rischio**.
- L’esecuzione dell’analisi prevede le seguenti fasi:
    1. Individuazione dei fattori di rischio
    2. Definizione tramite parametri quantificabili
    3. Quantificazione del livello di rischi ![[Pasted image 20261014173619.png]]


## Modalità di gestione del rischio
- Definito il rischio di progetto, è necessario definire le *modalità di gestione del rischio*, ossia gli accorgimenti adottabili per ridurre la probabilità di insorgenza dei rischi e/o i loro effetti negativi.
- Questa fase prevede:
    - l’**individuazione degli elementi critici**, ossia delle parti del progetto più soggette a rischio, su cui concentrare risorse e controlli;
    - la **segmentazione del progetto** in sotto-progetti o fasi, in modo da ridurre l’impatto dei rischi sulle fasi successive;
    - la **fissazione di punti di controllo** in corso d’opera, per verificare che il progetto stia procedendo secondo i piani;
    - la **definizione di punti di arresto**, per interrompere il progetto prima che causi danni maggiori.

## Integrazione dell'analisi del rischio nel progetto di massima
- Una volta eseguita l'analisi del rischio, si specifica la soluzione scelta da adottare nel progetto di massima indicando *tempi* e *modi di realizzazione*
- Questa fase si articola in:
	- **Segmentazione del prodotto**: suddivisione del progetto in moduli ognuno finalizzato al completamento del sottosistema specifico. Dipende dalle scelte realizzative.
	- **Riepilogo delle acquisizioni e realizzazioni previste**: indica le principali acquisizioni e realizzazioni previste per ciascun modulo, con riferimento alle specifiche del sistema e ai requisiti della soluzione.
	- **Modello organizzativo**: definisce la struttura organizzativa finalizzata alla gestione del progetto e le modalità di interazione fornitore-cliente
	- **Piano del progetto**: esplicita sequenza e dipendenza tra le principali attività

## Segmentazione del progetto
- #recall software engineering
- Ce ne sono diversi:
	- **Realizzazione in soluzione unica** (o "a cascata"): il SI viene realizzato, e collaudato in un’unica versione, generalmente con una attività continuativa.
	- **Realizzazione incrementale**: la realizzazione e il collaudo avvengono per parti successive, ciascuna delle quali contiene un sottoinsieme delle funzionalità e dei servizi previsti.
	- **Realizzazione evolutiva**: la realizzazione e il collaudo avvengono per parti successive, ciascuna delle quali contiene un sottoinsieme delle funzionalità e dei servizi previsti, ma con la possibilità di modificare le specifiche del sistema in corso d’opera tramite feedback. In genere si consegna un core di funzionalità e successivamente si aggiungono funzionalità in base alle esigenze degli utenti.
	- **Approccio Agile**: 
        - Il progetto viene realizzato in brevi cicli di sviluppo (da 1 a 4 settimane) che producono versioni incrementali del sistema, ciascuna delle quali contiene un sottoinsieme delle funzionalità e dei servizi previsti.
        - Ogni ciclo prevede la definizione dei requisiti, la progettazione, lo sviluppo, il collaudo e la revisione del ciclo precedente.
        - Il cliente partecipa attivamente al processo di sviluppo, fornendo feedback continuo e contribuendo alla definizione dei requisiti e delle priorità.
## Modello organizzativo
- Definisce la struttura organizzativa preposta al progetto per il committente (figure chiave, responsabilità, impegno richiesto agli utenti) e quella richiesta al fornitore (Responsabile di Progetto, responsabili di servizio), oltre alle modalità e agli strumenti software di interazione e controllo (es. riunioni, documenti, SharePoint, Microsoft Project, ErWin).

# Piano del progetto proposto
- Esplicita sequenza e dipendenza tra le principali attività, indicando tempi, risorse e costi previsti per ciascuna attività. 
- In particolare, il piano del progetto deve indicare:
	- **Piano di rilasci** (requisiti, specifiche, prototipi, installazione finale)
	- **Punti di controllo**
	- **Piano di attività**:
		- WBS (Work Breakdown Structure): scomposizione gerarchica del progetto in attività elementari
		- Diagramma di Gantt: rappresentazione grafica delle attività e delle loro dipendenze nel tempo
        - Diagramma di PERT: rappresentazione grafica delle attività e delle loro dipendenze, con indicazione dei tempi minimi e massimi di completamento
## WBS
- La WBS costituisce una rappresentazione del progetto, in forma grafica e/o descrittiva che, suddividendo le attività in livelli, consente un’analisi di dettaglio indispensabile per una corretta identificazione delle attività elementari la cui esecuzione integrata conduce alla realizzazione dell’intero progetto
- Collegamento tra *deliverables* e attività
- Nessuno sviluppo a livello temporale e di precedenze/ordine
- Permette di:
	- Evitare duplicazioni
	- Fare chiarezza e trasparenza
	- Facilitare la comunicazione con gli stakeholders
	- Permette a tutti di riferirsi in maniera omogenea ed inequivocabile al lavoro da eseguire
	- Operare aggregazioni di dati elementari (tempi, costi, ricavi,...) sulle parti di lavoro
- Ex:
![[Pasted image 20261014181251.png]]

## Diagramma di Gantt
- Il diagramma di Gantt consiste in una tabella nella quale le righe servono per indicare le attività previste dalla WBS, mentre le colonne si utilizzano per indicare i tempi necessari per realizzarle.
![[Pasted image 20261014181443.png]]
- Tempo solare: tempo effettivo in giorni
- Giorni lavoro: tempo effettivo in giorni/ore lavorativi
- In questo caso viene indicato il tempo solare
- Serve a:
	-  Pianificare la tempistica delle attività di progetto
	- Verificare la fattibilità temporale del progetto
	- Permettere a tutti gli interpreti del progetto di avere un quadro generale ed integrato delle date di inizio e fine delle attività di progetto
	- Ufficializzare a livello strategico le date desiderate/imposte di inizio, fine ed eventuali milestone intermedie (Master Schedule)
	- Ufficializzare a livello operativo le date di inizio e fine attese di ciascuna attività elementare (Gantt di dettaglio)
	- Controllare durante l’avanzamento del progetto gli scostamenti temporali (ritardi/anticipi) rispetto alle date pianificate
	- Verificare continuamente le nuove stime a finire del progetto o di sue parti a fronte dei consuntivi e per facilitare le relative correzioni
## Diagramma di PERT
- Un diagramma di Gantt non modella l'interdipendenza delle attività, caratteristica invece della programmazione reticolare, cioè del diagramma PERT (Project Evaluation and Review Technique). Ad ogni attività possono essere in generale associati una serie di attributi: durata (o data di inizio e fine), predecessori, risorsa, costo.
- Il diagramma di PERT è la rappresentazione grafica del progetto sulla quale sono indicate:
	- le *attività* elementari previste dalla WBS
    - le *milestones*
    -  condizionamenti reciproci temporali tra le attività (*legami di precedenza*).
- Il primo nodo è sempre quello di inizio progetto, mentre l'ultimo è quello di fine progetto.
- **Legami di precedenza**:
	- *Finish to start*: B non può iniziare se non è terminata l’attività A (A: Raccolta dati, B: Analisi dati)
	- *Start to start*: B non può iniziare se non è iniziata l’attività A (A: Scrittura codice, B: Testing)
	- *Start to finish*: B non può terminare se non è iniziata l’attività A (A: Inizio del nuovo sistema di backup automatica, B: fine del vecchio sistema di backup manuale)
	- *Finish to finish*: B non può terminare se non è terminata l’attività A (A: Sviluppo del software, B: Scrittura della documentazione tecnica che descrive il funzionamento del software)
- Ad ogni legame può essere associato un *ritardo* (lag) o un *anticipo* (lead) rispetto al legame di base.
![[Pasted image 20261015142332.png]]
- Il diagramma di PERT consente di:
    - evidenziare le attività critiche del progetto (***Critical Path***)
    - calcolare i tempi minimi e massimi di completamento del progetto
    - stimare la probabilità di completamento del progetto entro una certa data
- L'identificazione del percorso critico si effettua in due fasi:
	1. *Forward pass* (cammino in avanti):  si parte dall’inizio del progetto e si sommano le durate di ciascuna attività fino a determinare la data di fine progetto. alla fine di questa fase per ogni attività sono determinate le date:
		1. *Early start*: data di inizio al più presto
		2. *Early finish*: data di fine al più presto ![[Pasted image 20261015142550.png]]
	2. *Backward pass* (cammino all'indietro): si parte dalla fine del progetto e si  sottrae la durata di ciascuna attività. Alla fine di questa fase per ogni attività sono determinate le date: 
		1. *Late start*: data di inizio al più tardi 
		2. *Late finish*: data di fine al più tardi ![[Pasted image 20261015142537.png]]
- La differenza tra le date di inizio e fine al più presto e al più tardi determina il *Total Float* (o margine) dell’attività: se il float è pari a zero, l’attività appartiene al percorso critico$$
Total Float = Late Start - Early Start
$$
- La *catena* formata dalle *attività critiche* è il *percorso critico*
- Le fasi tipiche di gestione di un PERT includono: 
	- **Pianificazione e sviluppo del modello** sulla base della WBS e considerando la relativa sequenza logica di ogni attività
	- **Stime dei tempi e analisi dei percorsi**
	- **Programmazione operativa**, ovvero sulla base dei risultati ottenuti dall'analisi, si definiscono le risorse da attuare e costi
- Di solito qua si possono trovare problemi di ricerca operativa
## Analisi Costi-Benefici
- L’analisi costi-benefici consiste in una valutazione dei costi e dei benefici del progetto, al fine di determinare se il progetto è economicamente sostenibile e vantaggioso.
- L’analisi costi-benefici si articola in tre fasi:
    1. **Valutazione dei benefici attesi:** dettaglio dei benefici dei attesi, monetizzabili e misurabili secondo criteri oggetivi, individuando anche correlazioni con gli obiettivi.
    2. **Stima dei costi:** Si misura l'impatto dei benefici sul budget, soprattutto esplicitando le metriche utilizzate in precedenza
    3. **Analisi dell'investimento**: analisi delle giustificazioni economiche sull'investimento da effettuare, valutando anche alternative
- I costi possono essere classificati basandosi su diversi criteri (o uno o l'altro)
	- **Classificazione per tipologia di risorsa**: costo tecnologie, personale, servizi esterni e altri costi non legati alle tecnologie informatiche(immobili, materiali di consumo,...)
	- **Classificazione per missione**: costi di sviluppo (costruzione e avviamento) e di esercizio (funzionamento dei sistemi)
	- **Classificazione in interni ed esterni**: 
        - costi interni: *diretti* (costi del personale tecnico informatico impegnato nelle attività di sviluppo e di esercizio del sistema) e *indotti* (costi legati alle attività svolte dagli utenti finali per attività connesse al progetto, tipo formazione o caricamento dei dati nell'archivio)
        - costi esterni: sostenuti dall’organizzazione per l’acquisizione di beni e servizi da fornitori esterni (hardware, software, consulenze, ecc.)
- I procedimenti di stima e di valorizzazione dei costi varia a seconda del tipo di costo.
![[Pasted image 20261015155349.png]]
![[Pasted image 20261015155408.png]]
- I prezzi medi aggiornati e classificati per voce di costo e per tipologia di tecnologia sono pubblicati da analisti e osservatori del mercato informatico quali ad esempio Gartner Group, Metagroup, Sievers Consulting

## Outsourcing
- L’outsourcing ha luogo quando un’organizzazione affida tramite un **accordo contrattuale a un fornitore esterno** la responsabilità di una o più funzioni o servizi specializzati precedentemente svolti internamente.
- Ci sono diverse motivazione, solitamente il sistema IT costa molto di più mantenerlo che affidarlo a qualcun altro
- Bisogna valutare impatti economici e organizzativi sull'andare sul cloud, tuttavia in genere aziende che non basano il proprio business sull'IT ottengono diversi vantaggi:
	- Riduzione dei costi di gestione
	- Benefici finanziari, in particolare evitare investimenti sui beni immobili
	- Aumento del livello qualitativo del servizio
	- Accesso a tecnologie avanzate
	- Possibilità di incrementi di capacità *a richiesta*
- L'outsourcing opera su tutti i livelli e può essere applicato a tutte le componenti di un sistema informativo. Questo però non implica anche la pianificazione infatti la progettazione del SI deve essere sempre fatta all'interno dell'azienda mentre all'esterno vengono gestite le problematiche tecnologiche.
- Questa scelta prese piede negli anni \'80, ma trovarono una reale attuazione con gli **ASP (Application Service Provider)** e successivamente con il **Cloud Computing** negli ultimi anni.

## Cloud Computing
- Indica un insieme di tecnologie che permettono, tipicamente sotto forma di un servizio offerto da un provider al cliente, di memorizzare/archiviare e/o elaborare dati (tramite CPU o software) grazie all'utilizzo di risorse hardware/software distribuite e virtualizzate in rete.
- Ci sono diversi tipi di servizi offerti dal Cloud Computing:
	- **SaaS (Software as a Service)**: utilizzo di programmi installati su server remoti, deriva dall'ASP
	- **DaaS (Data as a Service)**: vengono messi a disposizione via web i dati ai quali gli utenti possono accedere tramite qualsiasi applicazione come se fossero in un disco locale
	- **PaaS (Platform as a Service)**: viene messa a disposizione una piattaforma di sviluppo in cloud provvista di componenti base in maniera tale che il programmatore dell'azienda possa lavorare solo sulla logica applicativa.
	- **IaaS (Hardware as a Service)**: l'utente acquista un'intera piattaforma hardware comprensiva di sistema operativo e programmi *on-the-shelf*
- Principali fornitori: Microsoft Azure, Amazon Elastic Computing Cloud (EC2), Google App Engine, IBM
- E' possibile l'impiego di *cloud pubblico*. Sebbene sia maggiormente vulnerabile agli attacchi, lo si fa per:
	- Testare l'applicazione che si sta sviluppando
	- Utilizzare un SaaS che si ritenga resiliente agli attacchi
	- E' forte l'esigenza di scalabilità
	- Esiste una collaborazione con più aziende
- Il **multi-cloud** è una scelta nella quale si coinvolgono più *cloud provider* con l'obiettivo di evitare il *vendor lock-in*, sfruttare le soluzioni migliori (Gartner) e ridurre i rischi in caso di guasto. Tuttavia risulta complicato definire appropriate policy per:
	- Security e trust: perimetro di sicurezza sfumato dovuto all'eterogeneità dei fornitori, problemi di conformità
	- Deploying, Balancing e Provisioning: API diverse, bilanciamento del carico (costi, latenza) dovuto al traffico di dati, costi di trasferimento dati, visibilità dello stato dell'infrastruttura ridotta
- Vantaggi del cloud:
	- Economico: diminuzione dei costi, capacità di ottimizzare risorse computazionali e scalare le proprie applicazioni in maniera tale da ridurre il carico di lavoro, ridurre gli investimenti a fronte di maggiori spese
	- Operativo: drastica riduzione di tempi di realizzazione e messa in esercizio di nuovi servizi, capacità di scalare in base alle esigenze, rapido provisioning delle risorse, ottimizzazione dei costi energetici
- Svantaggi del cloud:
	- Mancanza sul rispetto della normativa della privacy: manutenzione dei dati gestita all'esterno dell'azienda, mancanza di figure tecniche previste dalla normativa (titolare, responsabile e incaricato)
	- Compromissione della sicurezza di rete e dei dati: gestione dell'identita dei dati, compromissione di interfacce di management, problemi di sicurezza interna

## Appliance
- **Appliance**: hardware integrato con il software necessario a fornire una specifica funzionalità, solitamente senza richiedere ulteriori configurazioni, parametrizzazioni o tuning. 
- Il modello di business basato su appliance si è ampiamente diffuso nel mondo dei database (DBMS) per gestire carichi di lavoro massivi. Alcuni modelli sono:
	- SAP HANA Appliance: Ottimizzato sia per l'analisi dei dati in tempo reale (**OLAP**) sia per la gestione delle transazioni quotidiane (**OLTP**).
	- Oracle Exadata: Progettato per offrire altissime prestazioni sia in ambito **OLAP** (Data Warehousing) sia **OLTP**.
	- IBM Netezza: Un'appliance storicamente focalizzata e ottimizzata esclusivamente per l'analisi dei dati su larga scala (OLAP) e il Data Warehousing.
- Questo paradigma è applicabile solo a grandi realtà visto il suo costo elevato.
- Questa soluzione riduce i costi e permette performance così tanto elevate da poter permettere di creare un cloud privato.
## Total Cost Ownership
- Il TCO rappresenta il costo totale legato all’acquisto, all’implementazione, all’uso e alla manutenzione di un asset per tutto il suo ciclo di vita
- E' composto da tre costi differenti:
	- **Costi di acquisto (CAPEX)**: pagati in *UpFront*(investimento iniziale), coinvolgono hardware, licenze software, installazioni, cablaggio,...
	- **Costi operativi (OPEX)**: spese ricorrenti/gestione, include energia elettrica, stipendi del personale IT, contratti di supporto, connettività
	- **Costi indiretti/nascosti**: spese variabili/impreviste, come perdite da downtime(sistema non operativo), formazione del personale, migrazione dati, sicurezza e backup.

# Individuazione dei benefici
- **Benefici monetizzabili**: sono quelli a cui è possibile associare direttamente un valore di tipo monetario: maggiori guadagni, minori costi. 
- **Benefici misurabili**: pur non essendo possibile associargli direttamente un valore economico è necessario individuare dei misuratori (KPI) tramite i quali si possa quantificarne il loro valore
- **Benefici intangibili**: possono essere valutati solo in base a considerazioni qualitative, aziendali, sociali o politiche.
- Dato che costi e benefici non si realizzeranno nello stesso momento è necessario definire l’istante temporale a cui fare riferimento attualizzando costi e benefici rispetto a tale data.
- Ovviamente avere dati a disposizione permette un'analisi comparativa.

## Benefici misurabili nella Pubblica Amministrazione
- Gli indicatori chiave delle prestazioni (KPI), esprimono (o sono connessi) al raggiungimento degli obiettivi e, anche nelle Amministrazioni pubbliche, sono il riferimento per la gestione nel suo complesso, secondo criteri di responsabilità ed autonomia decisionale.
- Tra questi indicatori assume particolare rilevanza a livello esogeno il livello di servizio reso agli utenti interni o esterni (altre Amministrazioni, Imprese, categorie sociali, singoli cittadini, ecc.), che fornisce una misura della qualità (percepita) del prodotto/servizio erogato mentre, a livello endogeno resta alta l’attenzione per le iniziative in grado di migliorare la qualità di vita e del lavoro degli operatori.
- La variazione indotta dal progetto su queste variabili (o su di un loro sottoinsieme) rappresenta una differente chiave di valutazione della validità del risultato atteso. Si tratta di una valutazione ovviamente più ampia rispetto a quella condotta tenendo conto delle sole variabili monetizzabili e che pertanto richiede l’adozione di un differente approccio metodologico.
![[Pasted image 20261018215317.png]]
# Raccomandazioni per le fasi realizzative
- Sulla base degli approfondimenti effettuati durante tutto lo svolgimento dello studio di fattibilità presenta un insieme di indicazioni da tener presenti per risolvere o minimizzare le problematiche emerse.
- Questa sezione si articola su due punti:
	- **Indicazioni per l’approvvigionamento**: tipologia di fornitore, selezione delle offerte, modalità di approvvigionamento
	- **Indicazione per la gestione del progetto**: piano di qualità e gestione del rischio, project management, esigenze di negoziazione delle varianti
- Dopodiché si fa un riepilogo degli elementi utili alla stesura del **capitolato**.

# Ciclo di vita di un progetto SI

![[Pasted image 20261101151550.png]]
- Attuazione del ciclo di Deming
- Viene avviato una volta fatta la definizione dei *margini*, ovvero obiettivi, requisiti, budget che vengono di solito analizzati durante la stesura dello studio di fattibilità
- Immagine:
	- In alto troviamo le fasi del ciclo di vita di un progetto
	- Sotto troviamo cosa viene fatto
- Ex: erogazione -> sistema HR di Trevi
# Strategie di Acquisizione e Sourcing
- Prima di tutto è necessario distinguere due termini:
	- *Acquisto sul mercato di un bene o di un servizio*: accezione generica riferita alla scelta di acquisire da terzi (Buy) invece che realizzare internamente Make) un bene o un servizio, appartenente alla logica **Make or Buy** -> In-house/*Insourcing*
	- Esternalizzazione (**outsourcing**): presuppone una qualche forma di stabilità del rapporto di "collaborazione" tra l'impresa e il terzista con una prospettiva di medio termine
- Nella realtà:
	- Quasi nessuna organizzazione realizza il proprio stack informativo in-house
	- L'affidamento totale all'esterno è altrettanto raro.
	- Si può partire anche da un Buy e fare tante implementazioni per renderlo specifico e farlo diventare un Make, tuttavia bisogna tenere conto di costi e tempi
## Outsourcing

>[!Definizione]
>L’outsourcing ha luogo quando un’organizzazione affida tramite un accordo contrattuale a un fornitore esterno la responsabilità di una o	più funzioni o servizi specializzati
>precedentemente svolti internamente.
- Benefici che può portare:
	- Focalizzazione sulle attività core dell'organizzazione.
	- Riduzione dei costi di gestione: in quanto i profitti dell'outsourcer saranno più date dalle competenze specialistiche e dall'economia di scala
	- Benefici finanziari: evita la necessità di investire su immobili
	- Aumento del livello qualitativo del servizio
	- Accesso a tecnologie avanzate
	- Flessibilità e scalabilità della capacità a richiesta.
- Rischi principali:
	- Perdita di controllo sui processi e sul *know-how* interno
	- Riduzione progressiva del potere negoziale nel medio periodo (rischio di *lock-in* o di dipendenza dal fornitore).
	- Demotivazione del personale IT interno, che può sentirsi marginalizzato o spogliato delle proprie mansioni.
- L'esternalizzazione viene formalmente classificata lungo due assi cardinali:
```
                       AMPIEZZA DEL MANDATO
                 Selective                Full (Global)
            +------------------------+------------------------+
            |                        |                        |
        ITO |  Outsourcing selettivo |  Outsourcing globale   |
            |  di servizi IT         |  dell'intera funzione  |
MISSIONE    |  (infrastrutture, app) |  IT aziendale          |
            +------------------------+------------------------+
            |                        |                        |
        BPO |  Esternalizzazione di  |  Delega end-to-end     |
            |  singoli sotto-processi|  di un'intera area     |
            |  di business           |  funzionale            |
            +------------------------+------------------------+
```
- Coordinata 1: In base alla missione affidata
	- **Information Technology Outsourcing (ITO)**:  Esternalizzazione di attività operative, di sviluppo, esercizio e manutenzione dei Sistemi Informativi e delle risorse informatiche. Tipi di servizi ITO affidabili:
		- _Application Management:_ Manutenzione (adeguativa, correttiva, evolutiva) e conduzione del parco applicativo software.
		- _Application Service Provisioning (ASP) / Cloud SaaS:_ Fruizione di applicazioni ospitate da terzi.
		- _Facility Management / Data Center:_ Gestione delle infrastrutture fisiche HW (presso il cliente o il fornitore), inclusi business continuity e disaster recovery.
		- _Desktop Management:_ Installazione, manutenzione e assistenza del parco macchine e delle postazioni utente.
		- _Network Outsourcing:_ Servizi di connettività, sicurezza perimetrale e gestione degli apparati di rete.
		- _System Integration, Help Desk e Consulenza:_ Direzione lavori, supporto tecnico e monitoraggio.
	- **Business Process Outsourcing (BPO):** Esternalizzazione di interi processi operativi aziendali, tradizionalmente strumentali (non-core), come Risorse umane (payroll e reclutamento), finanza e contabilità, customer care/help desk di primo livello, approvvigionamenti (procurement) ed e-commerce. Nello scenario tradizionale il fornitore IT interagisce con il responsabile del servizio interno, che poi eroga l'attività all'utente; nel BPO il fornitore gestisce direttamente il processo operativo interagendo in prima persona con l'utente finale.
		- Esempio: Accenture è un'azienda di consulenza informatica che ha deciso di dedicare un business basato su outsourcing
- Coordinata 2: In base all'ampiezza del mandato, ovvero la quantità, la portata e la varietà delle attività o servizi IT che l'organizzazione decide di delegare all'esterno rispetto all'intero patrimonio tecnologico aziendale.
	- **Full (Global) Outsourcing:** Delega dell'intera funzione IT (o della stragrande maggioranza di essa) a un unico fornitore esterno tramite un solo contratto a lungo termine (tipicamente 5-10 anni).
		- Vantaggi: Interfaccia e interlocutore unico; forte integrazione architetturale; semplificazione nella gestione formale del contratto.
		- Svantaggi: Scelta limitata sui singoli componenti (_one size fits all_); rischio sistemico elevato (insuccesso del fornitore = blocco aziendale); perdita severa di controllo e rischio lock-in
	- **Selective Outsourcing (Multisourcing):** La gestione IT è spacchettata e delegata a più fornitori specializzati tramite contratti multipli e di durata più contenuta (3-5 anni).
		- _Vantaggi:_ Creazione di un clima competitivo sui costi e sulla qualità tra fornitori; adozione delle migliori soluzioni specialistiche per ogni ambito; mitigazione del rischio di insuccesso globale.
		- _Svantaggi:_ Elevata complessità gestionale di coordinamento e integrazione a carico della committenza; rischio di "scaricabarile" tra fornitori diversi in presenza di anomalie.
## Modelli Organizzativi e Forme societarie di Outsourcing
- Le strategie di acquisizione si distribuiscono su una matrice a due dimensioni: **Mercato (Aperto vs Chiuso)** e **Realizzazione (Fare in Casa vs Acquistare sul Mercato)**
![[Pasted image 20261101205944.png]]
- **Funzione IT Interna:** La struttura appartiene all'organigramma aziendale e risponde gerarchicamente alla direzione; implementa progetti internamente o commissiona specifiche parti operative.
- **Insourcing (Fornitore Interno):** La funzione IT viene trasformata in una società di servizi separata, interamente posseduta dall'azienda committente (o dall'amministrazione pubblica, es. Consip http://www.consip.it/on-line/Home/Chisiamo/Lanostramissione/Modello.html). Non opera sul mercato libero ed è regolata da contratti di servizio interni con tariffari determinati. Opera principalmente tramite contratti informali (controllo di gestione come centro di ricavi) o contratti di servizio(definizione di tariffe di servizi). Nel BPO assume la denominazione di **Captive Company** (es. FGA Capital per la gestione crediti del Gruppo Fiat).
- Selective e Full Outsourcing
- **Società Partecipata (Joint Venture):** Creazione di una società indipendente costituita in cooperazione tra l'azienda committente e un fornitore di mercato. Le quote societarie vengono ripartite a seconda che si voglia privilegiare il controllo del committente o la flessibilità/responsabilità dell'outsourcer. Opera esclusivamente tramite contratti di servizio.
- **Consorzio e Raggruppamento Temporaneo di Imprese (RTI):** L'incarico viene assegnato a un'aggregazione di più aziende fornitrici in partnership tra loro coordinate sotto un unico contratto. Permette di unire competenze verticali diverse (approccio _Best of Breed_), ma genera complessità organizzativa dovuta alla disomogeneità culturale, tecnica e metodologica delle imprese consorziate.
- **Finanza di Progetto (Project Financing):** Modello di mercato aperto in cui la realizzazione e gestione dell'opera o servizio IT viene finanziata e gestita da soggetti privati, che ne recuperano i costi tramite i proventi della gestione operativa del servizio stesso.
- Non esiste una soluzione migliore e le soluzioni adottate possono essere rivisitate al mutare per esempio di condizioni interne
- I servizi IT sono sempre più una combinazione di attività interne ed esterne
- Esternalizzare i servizi informatici non significa sopprimere la funzione IT interna, anzi la responsabilità finale del management rimane al committente
## Impatto Organizzativo ed Evoluzione della Funzione IT
- La funzione di IT in Make (a sinistra) e in Outsourcing (a destra)
![[Pasted image 20261101211739.png]]
- Quindi l'IT con l'outsourcing diventa più un'attività di governo che di servizio, quindi implica meno personale ma più di alto livello: Diminuiscono le figure a bassa specializzazione operativa (programmatori, addetti all'hardware interno); cresce la richiesta di ruoli ad elevata specializzazione manageriale e di governo (Project Manager, Analisti di business, Architetti dei dati, Security Officer, esperti legali e contrattuali).
- Processo di *Software Selection* e del mercato (Trevi ha chiesto ad Amadori un RPA - Robotic Process Automation)
- In outsourcing si definiscono nuove attività in caso di outsourcing:
	- Progettazione: studi di fattibilità, rappresentazione dei requisiti, stima investimenti, analisi costi/benefici e contratti
	- Pianificazione e Controllo: controllo coerenza, definizione di priorità e verifica raggiungimento obiettivi
	- Relazione con gli utenti: acquisizione dei requisiti, VoC/UAT
	- Osservatorio sul mercato IT: controllo soluzioni proposte dal fornitore e sul rischio di perdita del controllo
	- Project Management: gestione rapporti con il fornitore, controllo budget, verifica avanzamento dei lavori, accettazione e collaudo prodotti, segnalazione, proposte di azioni correttive o preventive, monitoraggio
	- Data Administration & Data Architecture: sorveglianza qualità dei dati
	- Security Management: verifica applicazione delle politiche di sicurezza, verificare il rispetto della norma vigente
- Fattori di successo:
	- Approccio progressivo, in particolar modo basandosi sulle condizioni interne, nell'applicare l'outsourcing
	- Fornitore scelto in base alle garanzie che può fornire per contenere i rischi
	- Definizione del contratto, come responsabilità Cliente-Fornitore, applicazione tariffe, pariteticità, correttezza, funzionalità, flessibilità
- GDO (Grande Distribuzione Organizzata) non così tanto sviluppata in Italia e quindi ha bisogno di "ristrutturazioni" e manutenzioni in grado di soddisfare la nuova domanda
## Strategie di realizzazione di Software Applicativo
- La necessità di nuovo software deriva dall'automazione di processi non coperti o dalla manutenzione (MAC: correttiva/adeguativa, MEV: evolutiva). Le opzioni strategiche disponibili sono quattro:

| **Strategia**                                                                                                      | **Vantaggi Principali**                                                                                    | **Svantaggi e Criticità**                                                                                                                                                                                                                                             | **Contesto di Elezione**                                                             |
| ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Sviluppo ad hoc ex-novo**                                                                                        | Massima personalizzazione; perfetta aderenza a logiche aziendali proprietarie uniche.                      | Alti costi; forte incertezza su tempi e budget; richiede competenze elevate.                                                                                                                                                                                          | Processi core esclusivi dell'azienda non coperti da standard di mercato.             |
| **Riuso di software (Modello PA - Pubblica Amministrazione)** (Esiste un sito tutti software sviluppati per la PA) | Abbattimento dei costi di analisi/sviluppo; riduzione del rischio di fallimento.                           | Vincoli di compatibilità; richiede interventi di personalizzazione/adattamento.                                                                                                                                                                                       | Enti pubblici (per legge obbligati a condividere soluzioni sviluppate per altre PA). |
| **Sistemi Open Source**                                                                                            | Basso costo di licenza iniziale; accesso al codice sorgente; assenza di lock-in.                           | Supporto non garantito da SLA (Service Level Agreement, cioè il contratto che formalizza l'adesione di certi standard per essere un servizio) stabili; rischio obsolescenza se la community decade. - (Inoltre devono essere certificate per avere maggior sicurezza) | Componenti infrastrutturali standard, middleware e librerie aperte.                  |
| **Sistemi proprietari COTS (Licenza d'uso)**                                                                       | Elevata affidabilità; certificazioni di performance; supporto strutturato e roadmap di rilascio garantite. | Costi ricorrenti di licenza/manutenzione elevati; rigidità dei processi proprietari; vendor lock-in e bisogna valutare il *TCO*(Total Cost Ownership - stima finanziaria dei costi durante l'intero ciclo di vita)                                                    | Soluzioni aziendali orizzontali consolidate (ERP, CRM, DBMS relazionali).            |
- Questi sono modelli di business, quindi devono trovare un modo per sopravvivere.
# Definizione e Governo del Contratto
- Lo strumento fondamentale per la gestione di una **fornitura**, l’oggetto materiale o immateriale (bene, servizio, opera o infrastruttura ICT) che un soggetto terzo (_fornitore_) eroga o trasferisce all'organizzazione committente a fronte di un corrispettivo economico.
- Avviene fra il responsabile del servizio (“committente”) e il fornitore ICT (Information and Communication Technology) (Art 1321)
- Tipologie:
	- **Contratto d’appalto** art. 1655 C.C. -> l'appalto è il contratto con il quale una parte assume con organizzazione dei mezzi necessari e con gestione a proprio rischio, il compimento di una opera o di un servizio verso un corrispettivo in denaro
	- Contratto d’opera art. 2222 C.C. : Basato sulla prestazione di lavoro prevalentemente personale dell'obbligato senza vincolo di subordinazione (tipico del singolo professionista o consulente).
	- Contratto di compravendita art. 1472 C.C. : Ha per oggetto il mero trasferimento della proprietà di una cosa (es. acquisto hardware a scaffale) verso il pagamento di un prezzo.
- Ogni *classe di fornitura ICT* sul mercato può appartenere a uno di questi tre tipi di contratti e ciascuna di esse viene gestita in modo diverso dal Codice Civile
- Le forniture reali sono spesso una combinazione di queste forme (es. fornitura apparecchiature, sistemi chiavi in mano, locazione HW/SW, licenze d'uso con personalizzazione, sviluppo ad hoc, outsourcing di servizi).
- Il contratto non deve essere inteso come un adempimento burocratico:
	- Ha l'obiettivo di **soddisfare entrambe le parti**: se gli obiettivi non vengono raggiunti, entrambe le parti hanno fallito.
	- Deve definire prestazioni (contenuti, costi, qualità, responsabilità) in modo cooperativo ed eliminare le ambiguità (quindi il contratto viene fatto quando non si va d'accordo).
	- *Non potendo prevedere a priori* ogni singola evoluzione tecnologica, deve stabilire **regole di relazione, procedure e meccanismi di governo** per tutta la durata del ciclo di vita.
	- **Organi di governo previsti:** Comitato guida (paritetico), Direzione dei lavori (rappresentante del committente), Monitoraggio (controllo di processo continuativo), Collaudo (verifica dei prodotti) e Certificazioni di qualità.
	- Altrimenti si va in tribunale e si interpellano le seguenti figure giuridiche:
		- CTU (Consulente Tecnico d’Ufficio): figura tecnica indipendente nominata dal giudice per valutare la correttezza tecnica di un contratto o di una fornitura.
		- CPU(Consulente Tecnico di Parte): figura tecnica indipendente nominata da una delle parti per valutare la correttezza tecnica di un contratto o di una fornitura.
- Il ciclo di vita contrattuale attraversa 4 fasi: **Impostazione** $\rightarrow$ **Negoziazione** (o gara pubblica) $\rightarrow$ **Stipula** $\rightarrow$ **Attuazione/Governo**
## Soggetti coinvolti
Nelle attività contrattuali intervengono figure con competenze legali, commerciali e tecniche:
- **Per il Committente:** Dirigenti strategici (responsabili delle strategie d'acquisto e dei sistemi informativi), Funzione Acquisti (gestione gare, contratti), Funzione Legale, Funzione Sistemi Informativi (studi di fattibilità, direzione lavori/project manager, monitoraggio, collaudo) e Personale Utente (definizione requisiti ed espressione del gradimento).
- **Per il Fornitore:** Dirigenti strategici delle offerte, Funzione Commerciale/Marketing (Account Manager), Funzione Legale, Struttura operativa ICT (Project Manager del fornitore, Project Controller, responsabili d'erogazione) e Assicurazione Qualità (Quality Manager, analisti di Customer Satisfaction).
- Gli aspetti che devono essere coperti sono: Commerciale, Legale, Tecnico e Operativo
## Struttura di un Contratto
- Un contratto per servizi SI complessi è articolato in due macro-blocchi documentali:
```
+---------------------------------------------------------------------------------+
|                              STRUTTURA DEL CONTRATTO                            |
+---------------------------------------+-----------------------------------------+
|      PARTE NORMATIVA                  |      PARTE OPERATIVA                    |
+---------------------------------------+-----------------------------------------+
| - Corpo del Contratto                 | - Capitolato Tecnico (redatto dal       |
|   (aspetti tecnici, tempi, pagamenti, |   committente: requisiti e vincoli)     |
|   relazioni cliente-fornitore)        | - Offerta Tecnica (redatta dal          |
| - Parte Generale (clausole standard)  |   fornitore in risposta al capitolato)  |
| - Parte Speciale (clausole puntuali)  | - Allegati: Piano Progetto/Qualità      |
+---------------------------------------+-----------------------------------------+
```
- Spesso ci sono sovrapposizioni fra le varie parti (con anche incoerenze!)
- In caso di gara, corpo e capitolato fanno parte della documentazione predisposta dal committente, mentre l'offerta è la "risposta" (coerente) del fornitore -> il capitolato è l'espressione della volontà del cliente -> l'offerta è l'espressione della volontà del fornitore
- La struttura varia in base al suo oggetto

### Corpo del contratto
- Definisce e correla:
	-  aspetti tecnici (con la definizione dei beni e servizi oggetto del contratto)
	-  modalità, tempi e condizioni
	-  relazioni fra cliente e fornitore
	-  corrispettivi (pagamenti)
### Capitolato tecnico
- Predisposto dal cliente (insieme al bando, in caso di gara)
 - Recepisce le indicazioni dello studio di fattibilità
 - Fornisce al fornitore le informazioni di dettaglio utili per preparare l’offerta (di solito anche l’indice dell’offerta stessa)
- Allegato tecnico al corpo del contratto assieme all’offerta del fornitore

### Offerta tecnica
- Redatta dal fornitore in risposta a quanto richiesto dal cliente nel Capitolato Tecnico, corrisponde ai criteri di valutazione indicati nel bando di gara e/o nella lettera di invito
- Scopo: Dimostrare al cliente la capacità del fornitore di soddisfare quanto richiesto nel Capitolato Tecnico (indicando "come")
- Possibili allegati: piano di progetto e piano della qualità
### Parte Generale (clausole standard e vincoli di legge)
Include le condizioni valide per tutti gli appalti dell'ente:
- _Variazioni quantitative:_ Possibilità di aumento/diminuzione delle prestazioni (standard 20%, detto "quinto d'obbligo" o sesto quinto - per esempio se ci metto l'80% per l'attività ricevo l'80% del compenso, se ci metto il 120% ricevo comunque il 100%, è una flessibilità tra cliente e fornitore).
- _Cauzione:_ Deposito cauzionale a garanzia dell'adempimento (tipicamente pari al 10% del valore contrattuale, ridotta se l'azienda è certificata per la qualità).
- _Riservatezza, sicurezza sul lavoro e diritti d'autore/brevetti_.
- *Garante sull'attività operativa del cliente*: cioè il fornitore non può intervenire su l'attività del cliente troppe volte o provocando troppi danni.
- _Recesso:_ Facoltà concessa al committente con preavviso, non al fornitore (salvo giusta causa). RIGUARDA SU CONSIP
- _Subappalto e cessione:_ Divieto di cessione del contratto e vincoli rigidi sul subappalto, con responsabilità finale sempre in capo al fornitore.
- _Clausole risolutive espresse:_ Sanzioni antimafia, violazione riservatezza, accertamenti su autocertificazioni.

### Parte Speciale (disciplina specifica della fornitura)
Negli appalti complessi (quindi con peculiarità specifiche, es. manutenzione ed evoluzione SI) vengono normati articoli specifici:
1. **Oggetto e servizi:** Distinzione dettagliata tra Sviluppo/Manutenzione Evolutiva (MEV), Gestione Applicativa, Manutenzione Adeguativa e Correttiva (MAC) e Supporto specialistico, con dettagli relativi a riferimenti al capitolato e offerta tecnica(migliorativa o dettagliato), trasferimento di know-how (l'obbligo contrattuale del fornitore di condividere, documentare e trasmettere al committente (o a terzi designati) l'insieme delle conoscenze tecniche, architetturali e operative sviluppate o impiegate durante l'esecuzione del contratto) in cui viene versato una percentuale dell'importo contrattuale.
2. **Durata e affiancamento:** Copre l'arco temporale (es. 60 mesi), prevedendo mesi di affiancamento iniziale del fornitore uscente, mesi finali dedicati solo alla garanzia e cessione/trasferimento di know-how al committente o al subentrante.
3. **Obblighi e adempimenti a carico dell'impresa**: Oneri e rischi a carico dell'impresa, inclusi viaggi e missioni se necessari, Esecuzione a regola d'arte con rispetto di regole, norme di sicurezza,
4. **Proprietà dei prodotti:** L'amministrazione/committente acquisisce la piena proprietà del software sviluppato e della documentazione, con facoltà di riuso e modifica. Possibilità di uso di prodotti terzi, quali quelli del fornitore tramite licenze, prodotti open-source e possibilità di riuso in altre amministrazioni in caso di medesime condizioni e richiesta
5. **Dimensionamento massimo:** Fissazione dei tetti massimi di fornitura, tipicamente espressi in **Function Points (FP)** per il software nuovo/corretto e in **giorni-persona** per la gestione applicativa e il supporto.
6. **Piano della Qualità:** Strumento redatto dal fornitore e validato dal committente per descrivere l'organizzazione, i processi di sviluppo/test, gli standard e gli indicatori di qualità del codice e dei servizi (e tutto ciò che riguarda l'ingegneria del software). Il committente verifica e può richiedere modifiche e descrive un servizio in termini di struttura organizzativa, responsabilità, risorse e procedure; definendo inoltre il ciclo di vita (base, ridotto, urgente e a fase unica, iterativo,...) GUARDA ESEMPIO (Collegabile a GANTT)
7. **Garanzie**: Il fornitore garantisce l'eliminazione di eventuali difetti riscontrati (con opportune modalità nell'ultimo anno)
8. **Subappalto**: Regola la facoltà del fornitore di delegare una parte delle attività ad altre aziende terze (subappaltatori). Nel contratto devono essere esplicitamente indicate le **prestazioni subappaltate** e l'identità dei **subappaltatori** autorizzati.
9. **Pianificazione delle attività**: Stabilisce il modo e le scadenze con cui committente e fornitore concordano lo svolgimento del lavoro nel tempo attraverso formali **Piani di lavoro**.
10. **Produttività e risorse impiegate**: Fissa gli standard di resa lavorativa del team del fornitore e ne governa la composizione qualitativa e quantitativa.
	- **Indice di produttività:** Viene espressa come resa minima garantita, tipicamente calcolata in **punti funzione realizzati per giorno-uomo** (FP/giorno-uomo), conformemente a quanto dichiarato nell'offerta tecnica.
	- **Controllo dei profili:** Il committente valuta i curricula delle persone assegnate al progetto e si riserva la facoltà di richiederne la sostituzione qualora le competenze risultino inadeguate.
	- **Regole di turnover:** Disciplina le modalità e i preavvisi obbligatori nel caso in cui il fornitore debba sostituire membri del proprio gruppo di lavoro.
11. **Consegna dei prodotti**: Definisce le condizioni formali e temporali con cui il fornitore rilascia i deliverables (codice, moduli, architetture, documentazione tecnica e utente).
	- Il fornitore ha l'obbligo di rispettare sia le tempistiche pattuite sia gli **standard qualitativi di sviluppo e documentazione** definiti nel capitolato o migliorati nell'offerta.
	- La consegna materiale non equivale a pagamento immediato: è sempre subordinata all'**accettazione formale** da parte del committente.
	- Eventuali ritardi nella consegna dei deliverable espongono il fornitore a contestazioni e all'applicazione di penali.
12. **Collaudo e accettazione**: Costituisce il **controllo di prodotto**, ossia la verifica tecnica formale e definitiva che accerta la piena rispondenza di quanto realizzato rispetto alle specifiche contrattuali.
	- È condotto da una commissione di esperti incaricata dal committente (soggetti diversi da chi ha diretto i lavori), con il coinvolgimento degli utenti finali e alla presenza del fornitore. 
	- Verifica che software e infrastrutture siano esenti da vizi e in grado di svolgere tutte le funzionalità operative previste.
	- Se i test evidenziano **anomalie** o **difetti**, il collaudo viene ripetuto entro un termine pattuito; in caso di ulteriore esito negativo si arriva alla risoluzione del contratto, all'incameramento della cauzione e all'azione per risarcimento danni.
13. **Monitoraggio**(UAT - User Acceptance Test): Costituisce il **controllo di processo**, ovvero una vigilanza continua in corso d'opera parallela all'esecuzione del contratto.
	- A supporto della direzione lavori del committente, verifica costantemente lo Stato Avanzamento Lavori (SAL), la conduzione delle attività e la qualità dei processi produttivi e dei servizi del fornitore.
	- Nei contratti pubblici di rilievo è affidato per legge a un terzo indipendente qualificato dal CNIPA (il **Monitore**) e ha finalità preventive e diagnostiche (identificare tempestivamente le cause delle anomalie prima di arrivare al collaudo).
	- **Obblighi del fornitore:** Ha il dovere contrattuale di consentire le verifiche ispettive, permettere l'accesso a locali e documentazione, inviare i report periodici(di riscontro, di consuntivo e di supporto alla misura dei livelli di servizio) e designare un proprio referente dedicato per interfacciarsi con il monitore e partecipare a sedute di riesame congiunto delle attività.
	- Le azioni che si possono adoperare in questa fase sono: *prevenzione*, *diagnosi* e *consuntivo*(sui dati, una raccolta dati relativi al contratto e alle prestazioni erogato)
14. **Penali**: Determina sanzioni economiche pecuniarie a carico del fornitore a fronte di inadempimenti o mancato rispetto dei livelli di servizio (definite in Art. 14 CC) -> Solitamente quantificati dalle KPI e se vengono violate si procede con ai rilievi/le penali, oppure se superate si procede alle penali positive o ai compensi (indicati in un appendice del capitolato tecnico)
	- La loro funzione primaria **non è far risparmiare l'amministrazione/committente**, bensì creare un vincolo coercitivo e un deterrente per spingere il fornitore a mantenere la qualità pattuita.
	- Sono proporzionate alla gravità del disservizio e associate a soglie oggettive (es. ritardo nella consegna dei piani di lavoro o del piano qualità, slittamento delle scadenze, superamento del tetto massimo di difetti in collaudo o in esercizio, ritardi nel ripristino dell'operatività di sistema, turnover eccessivo del personale).
	- Si scala partendo dalla cauzione e successivamente è il fornitore a pagare
15. **Corrispettivo**: Stabilisce il modello economico con cui viene calcolata la remunerazione dovuta al fornitore per le prestazioni svolte. Ci sono principalmente due modelli:
	- **A corpo (prezzo fisso / forfeit):** Si stabilisce un importo globale predeterminato per l'intera commessa; il rischio di sottostima e di imprevisti grava per intero sul fornitore (richiede specifiche chiare e stabili). -> può essere saputo a priori e può essere fatta una stima (il rischio è sul fornitore, stima dell'impegno e imprevisti sono a suo carico)
	- **A misura (a consuntivo):** Il corrispettivo finale si calcola moltiplicando quantità consuntivate per tariffe unitarie entro un tetto massimo: -> (il rischio è sul cliente, il fornitore a più manovre di recupero della marginalità)
	    - _A misura su prodotti realizzati:_ Tariffa fissa per unità di software erogata (Function Point o LOC). 
	    - _A misura su risorse utilizzate (Time & Material):_ Tariffa giornaliera per figura professionale impiegata (giorni-persona), dove il committente remunera l'impegno temporale.
	- Sono possibili soluzioni intermedie: a corpo con correttivi, parte a corpo e parte a misura, con indici di prestazioni
16. **Fatturazione**: Fissa l'articolazione temporale, la periodicità e le condizioni amministrative necessarie per emettere fattura ed erogare i pagamenti.
	- I pagamenti avvengono tipicamente a scadenze prefissate (es. trimestrali o semestrali) oppure a tranche subordinate al raggiungimento di specifici **Stati di Avanzamento Lavori (SAL)** o al superamento positivo dei collaudi intermedi.
	- L'emissione della fattura è vincolata all'approvazione formale delle rendicontazioni delle attività e al computo delle eventuali penali da decurtare dall'importo dovuto.
17. **Risoluzione**: Prevede lo scioglimento anticipato del vincolo contrattuale a causa di gravi inadempimenti o del verificarsi di precise condizioni ostative previste per legge o per contratto.
	- **Risoluzione per inadempimento:** Scatta in situazioni critiche quali fallimento reiterato delle operazioni di collaudo, mancato rispetto prolungato degli standard di servizio, superamento del tetto massimo delle penali cumulabili o violazione sistematica degli obblighi contrattuali.
	- **Condizioni particolari di risoluzione:** Coinvolgono violazioni della riservatezza, motivi legati a provvedimenti antimafia, riscontro di falsità nelle autocertificazioni o applicazione di sanzioni interdittive a carico del fornitore.
	- **Conseguenze:** Interruzione immediata del rapporto, escussione della cauzione (10% del valore del contratto) a titolo di indennizzo e diritto del committente a richiedere il risarcimento per eventuali danni ulteriori.


Subappalto: Il **subappalto** è un contratto derivato con il quale l'appaltatore affida a un terzo (il subappaltatore) l'esecuzione di parte delle prestazioni o delle lavorazioni oggetto del contratto principale d'appalto, assumendosi il subappaltatore la propria organizzazione di mezzi e i relativi rischi. Il subappaltatore non ha, di norma, un rapporto diretto con il committente.
Rendicontazione: La **rendicontazione** è il processo attraverso il quale un soggetto (un'azienda, un ente o il beneficiario di un finanziamento) registra, organizza e presenta un resoconto formale delle spese sostenute e dei risultati raggiunti.
## Livelli di servizio (SLA - Service Level Agreement)
Il contratto stabilisce parametri quantitativi per garantire l'affidabilità del servizio:
- **Livelli di Servizio tecnici:** Indicatori oggettivi (es. disponibilità del server al 99%, disponibilità della rete, tempi di ripristino).
- **Requisiti di Servizio utente:** Indicatori dal punto di vista dell'utente finale (es. disponibilità del portale, tempo massimo di attesa o di disservizio).
- **Penali pecuniarie:** Non mirano a far risparmiare il committente, ma servono a disincentivare gli inadempimenti e ad assicurare il rispetto degli SLA (es. applicate per slittamenti di consegna, difetti in collaudo o in esercizio, turnover ingiustificato del personale, mancato ripristino tempestivo).

## Verifica della Prestazione: Collaudo vs Monitoraggio
Il controllo sull'adempimento contrattuale si sdoppia lungo due direttrici complementari:
- **Collaudo (Controllo di Prodotto):**
    - È la verifica formale, ultima e definitiva dell'esatto e completo adempimento.
    - Accerta che il software o l'infrastruttura siano conformi alle prescrizioni e privi di vizi funzionali.
    - Se l'esito è negativo, le prove vengono ripetute entro un termine; in caso di ulteriore fallimento si procede alla risoluzione per inadempimento, incameramento della cauzione e richiesta di risarcimento danni.
- **Monitoraggio (Controllo di Processo / Assicurazione Qualità):**
    - È un'attività continua e parallela all'esecuzione contrattuale svolta a supporto della Direzione Lavori.
    - Nei contratti PA di rilievo è obbligatoriamente affidata a una terza parte indipendente qualificata (il "Monitore").
    - Ha una funzione **preventiva e diagnostica**: controlla i SAL, i processi di sviluppo del fornitore, il rispetto degli standard documentali e l'accuratezza delle misure SLA, prevenendo i disallineamenti prima che si arrivi al collaudo finale.
    - Just Enough Quality; la qualità giusta è quella sufficiente

## Corrispettivo e Modelli di remunerazione

La scelta del modello di remunerazione è uno dei nodi più critici della contrattualistica SI:

| Modello                                                                     | Come funziona                                                                                                                                                                     | Applicazione                                                                                                               | Modelli di controllo                                                                                                | Vantaggi (Pro)                                                                                                                                                                    | Limiti e Rischi (Contro / Criticità)                                                                                                                                                                                                                      |
| --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A corpo** (_prezzo fisso / a forfait / rischio d'impresa_)<br>            | Il corrispettivo è determinato a monte come valore globale per l'intera opera. I pagamenti avvengono su SAL predefiniti.<br>                                                      | Specifiche ben definite, qualitativamente e quantitativamente oppure vincolo stretto sulle risorse, usate "ad esaurimento" | Verifica finale, collaudo del SW prodotto                                                                           | Gestione amministrativa semplice; certezza della spesa per il committente; il fornitore si accolla i rischi di stima errata o imprevisti.                                         | Richiede specifiche e requisiti stabili a priori; rigidità di fronte alle modifiche; scarsa garanzia di corrispondenza tra prodotto ottenuto e spesa sostenuta; necessario fare uno studio di fattibilità; specifiche voltatili e scarsa flessibilità<br> |
| **A misura su prodotti realizzati** (_FP / LOC_)<br>                        | Si fissa una tariffa unitaria per unità di software prodotto (Function Points e Lines of Code) e un tetto massimo di spesa. Il saldo è a consuntivo sul volume consegnato.<br>    | Consigliato per sviluppo e manutenzione evolutiva del SW                                                                   | Verifica finale, collaudo del SW prodotto<br>Misura (critica) della dimensione di SW sviluppato                     | Correlazione diretta tra spesa sostenuta e prodotto ottenuto (possibile riduzione dei costi); flessibile ai cambi normativi; incentiva il fornitore ad alzare la produttività<br> | Misurazione dei Function Points complessa, critica e da definire; non facilmente applicabile a interventi di manutenzione correttiva/adeguativa.<br>                                                                                                      |
| **A misura su risorse utilizzate** (_Time & Material / Giorni-persona_)<br> | Si pagano le giornate o le ore lavorate in base a tariffe giornaliere per figura professionale (analista, programmatore, project manager) e calcolo in base al consuntivo<br><br> | Utilizzato quando non si riesce ad applicare il modello relativo ai prodotti                                               | Verifica finale, collaudo del SW prodotto<br>Misura dell’impegno sostenuto (time report) e valutazione di congruità | Modello flessibile e semplice da contrattualizzare; ideale quando i requisiti sono volatili o per la manutenzione ordinaria.<br>                                                  | Il rischio di inefficienza e dilatazione dei tempi ricade interamente sul committente; assenza di legame diretto tra costo sostenuto e valore del software prodotto.<br>                                                                                  |
- Sono possibili soluzioni intermedie:
	- A corpo con correttivi
	- Parte a corpo e parte a misura
	- Con indici di prestazioni
- Esempio a corpo:
![[Pasted image 20261102114151.png]]
![[Pasted image 20261102114312.png]]




# Esempio piano di qualità
![[Pasted image 20261102103004.png]]

RIGUARDA ESEMPIO
![[Pasted image 20261102103019.png]]

![[Pasted image 20261102103039.png]]
# Macro esempio Consip
![[Pasted image 20261102094901.png]]
![[Pasted image 20261102094919.png]]![[Pasted image 20261102094940.png]]

# Esempio penali
![[Pasted image 20261102113824.png]]
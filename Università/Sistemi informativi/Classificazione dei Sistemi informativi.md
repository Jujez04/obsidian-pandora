- L'azienda governa i processi operativi finalizzati al raggiungimento degli obiettivi
- I SI supportano sia i processi di governo(SI direzionali e analitici), sia i processi operativi (SI operativi).

# Catena del valore di Porter

![[Pasted image 20260917143713.png]]
- La **catena del valore** (introdotta dall'economista Michael Porter nel 1985) è un modello teorico che scompone l'azienda in un insieme di **attività distinte** volte a progettare, produrre, vendere, distribuire e supportare i propri beni o servizi.
- Costituisce la base logica per mappare i processi aziendali e comprendere come le varie tecnologie software supportano le diverse fasi operative e direzionali.
- Solitamente si distingue:
	- **Organigramma**: come è strutturata l'azienda
	- **Obiettivi**: cosa fa l'azienda, distinguendo le varie attività

## Attività primarie (Flusso operativo diretto) #opt
- Sono le attività direttamente coinvolte nella creazione fisica del prodotto, nella vendita, nella consegna e nell'assistenza post-vendita. Nelle slide viene mostrato il flusso sequenziale per un'azienda manifatturiera:
- Si dividono in:
	- **Logistica in entrata (Gestione materie prime / Approvvigionamento)**: ricevimento, stoccaggio e gestione interna dei materiali.
	- **Attività operative / Produzione (Trasformazione)**: lavorazione dei componenti e assemblaggio del prodotto finito.
	- **Marketing e Vendite**: attività volte a promuovere il prodotto, fissare i prezzi, gestire i canali di vendita e incentivare l'acquisto.
	- **Logistica in uscita (Distribuzione)**: raccolta, stoccaggio e distribuzione fisica del prodotto ai clienti o alle reti di vendita.
	- **Servizi (Post-vendita)**: installazione, ricambi, manutenzione, riparazione e assistenza al cliente.

## Attività di supporto (Attività Infrastrutturali e di Governo) #opt
- Forniscono l'infrastruttura, le risorse e le tecnologie necessarie affinché le attività primarie possano svolgersi in modo efficace ed efficiente:
	- **Infrastruttura aziendale**: direzione generale, pianificazione strategica, contabilità, amministrazione e affari legali.
	- **Gestione delle risorse umane (HR)**: reclutamento, assunzione, formazione, retribuzione e sviluppo del personale.
	- **Sviluppo delle tecnologie (R&D / Progettazione)**: innovazione di prodotto, sviluppo software, ingegnerizzazione dei processi e know-how tecnologico.
	- **Approvvigionamenti (Procurement)**: funzione d'acquisto di tutte le risorse necessarie per le attività aziendali (non solo materie prime, ma anche macchinari, servizi esterni, contratti IT).

# Tassonomia dei SI
## SI operativi
- Informatizzano processi volti all’**esecuzione** di **attività** o alla loro **programmazione**, come:
	- Fabbricazione
	- Distribuzione
	- Amministrazione
	- Gestione del personale
- Un sistema informativo è efficace se il processo dipende dal Information Processing Capacity
- I SI operativi sono:
	- una tecnologia di produzione se vendono informazioni
	- una tecnologia di processo quando il prodotto è materiale, in tal caso hanno il ruolo di supportare il coordinamento delle attività
- gli investimenti tecnologici sono proporzionali al ruolo del SI(varia tra l’1% del fatturato nelle semplici manifatture e il 7% delle aziende di telecomunicazioni)
![[Pasted image 20260917144214.png]]

## SI direzionali
- Supportano il processo decisionale fornendo informazioni ai manager per aiutarli a decidere
- Il modello primario di questi sistemi deriva dal modello del ciclo di controllo:
![[Pasted image 20260917144552.png]]

- Dal punto di vista informativo, i processi direzionali si differenziano da quelli operativi poiché:
	- Si basano su **indici**, ovvero informazioni aggregate e riassuntive
	- L’elaborazione non è continua ma **periodica** (snapshot mantenuti in maniera periodica)
	- Sono costruiti sopra i SI operativi da cui **estraggono** le **informazioni** di base
- Monitora i **processi** **interni** all'azienda
## SI analitici
- Monitorano i **fenomeni** **esterni** all'azienda, come clienti, competitor,...
- Supportano la comprensione dei fenomeni di business:
	- Profilazione clienti
	- Storia del prodotto, come affidabilità e durevolezza
- Hanno elementi in comune con quelli direzionali:
	- Si basano su indici, ovvero informazioni aggregate e riassuntive.
	- L’elaborazione non è continua ma periodica
	- Sono costruiti sopra i SI operativi da cui estraggono le informazioni di base
- Tabella esemplificativa dell'utilizzo dei SI analitici per l'analisi della clientela:
![[Pasted image 20260917145359.png]]


# Mappatura dei SI

## Modulo software
-  Un **modulo** rappresenta un *blocco* di *funzionalità* software che *supportano* una *fase* di un processo aziendale, omogeneo per frequenza, attore e profilo di casi d’uso
- Descrive un'esigenza informativa con potenziale impiego di una tecnologia informatica
- Possono essere:
	- *Orizzontali*: non variano dalla tipologia aziendali
	- *Verticali*: dipendono dal settore produttivo specifico
- La suddivisione in moduli è utilizzata anche dai grandi produttori di package (SAP, Microsoft, ORACLE) per strutturare le proprie offerte:
![[Pasted image 20260917145826.png]]

## Portafoglio applicativo aziendale
- Rappresenta una possibile mappa dei SI aziendali
- *Esplosione della catena del valore di Porter*: incrocio tra le *fasi del ciclo di trasformazione* con la *tipologia delle attività* applicabili per la *pianificazione* ed esecuzione di ciascuna fase del ciclo. Ogni incrocio individua un modulo
- **Pianificazione**: determinare piani e controllare l'avanzamento; si distinguono per livello di dettaglio e orizzonte temporale
- **Attività di esecuzione**: attuano operazioni pianificate elaborando informazioni in tempo reale per migliorare prestazioni, tempi e costi, risparmiando attività burocratiche
![[Pasted image 20260917150507.png]]
![[Pasted image 20260917150518.png]]
- Esempio della mappa dei SI:
![[Pasted image 20260917150115.png]]
- Nella riga in alto ho le fasi della catena di Porter e nella colonna a sinistra ho le attività. In ogni incrocio ho il modulo progettato per quella attività nella determinata fase del ciclo di Porter
- Ogni incrocio sono informazioni sulla quale il modulo deve operare
 ![[Pasted image 20260917152054.png]]
- Guardando il portafoglio nel suo complesso, i sistemi vengono raggruppati in tre blocchi principali:
	1. **Portafoglio Operativo**: racchiude i software che supportano le attività produttive e commerciali quotidiane (_core business_), come la gestione della fabbrica, gli acquisti, le vendite e il magazzino.
	2. **Portafoglio Istituzionale**: comprende i software orizzontali necessari all'esistenza stessa dell'ente giuridico/aziendale, indipendentemente da cosa produce (contabilità/amministrazione, buste paga, gestione del personale).
	3. **Portafoglio Direzionale**: si posiziona sopra i due precedenti; non esegue operazioni quotidiane, ma estrae dati di sintesi e indici per consentire alla direzione di monitorare i risultati aziendali (ciclo di controllo, KPI, Business Intelligence).
- Un portafoglio applicativo corrisponde un insieme di moduli standard
- Si differenzia in base al tipo di azienda


## Differenziazione per tipologia di azienda
![[Pasted image 20260922103301.png]]
- **Azienda manifatturiera/chimica**: avrà un portafoglio molto sviluppato sulla manutenzione impianti, controllo di processo e schedulazione di fabbrica (SCADA/MES).
![[Pasted image 20260917152840.png]]
- **Compagnia telefonica**: ha pochissima enfasi sulla fabbricazione fisica, ma un portafoglio applicativo mastodontico per _Billing_, gestione del contatto con milioni di clienti (_CRM_) e _Pianificazione della rete_.

# Sistemi per la Produzione industriale

## Smart Manufacturing (CIM)
- Un'architettura multilivello che collega diversi livelli di un sistema di produzione ed è finalizzata all'ottimizzazione dei processi e alla gestione delle risorse
- Permette:
	- Riduzione delle scorte
	- Riduzione del tempo di commercializzazione
	- Aumento qualità del prodotto
	- Riduzione costi
- SM è organizzato in un'architettura a cinque livelli. Ogni livello esegue le proprie elaborazioni ed è collegato con gli altri.

![[Pasted image 20260917153231.png]]
## SCADA (livello 2)
- SCADA (Supervisory Control and Data Acquisition): usati dai sistemi CIM per monitora e controllare grandi impianti industriali/elettronici distribuiti nel territorio.
- Acquisiscono dati dai PLC e monitorano in tempo reale il sistema
- Permettono in generale di risparmiare tempo e denaro
![[Pasted image 20260917153619.png]]
- Hanno diversi ambiti di applicazione:
	- Grandi impianti industriali
	- Centri di ricerca (CERN)
	- Telecomunicazione (ripetitori)
	- Sistemi di gestione e produzione (acqua, energia,...)
- In questo livello trova spazio la manutenzione predittiva tramite machine learning
- Componenti
	- Sensori e attuatori
	- Controllori:
		- PLC (Programmable Logic Controller): è un qualunque dispositivo programmabile in grado di risolvere una logica che implementa un algoritmo.
		- RTU - Remote Terminal Unit: non elabora né risolve alcuna logica. E' una unità terminale che ricevuti gli "ordini" da una unità di classe superiore non fa altro che eseguirli. Si tratta quindi di un ripetitore con capacità di comunicazione, una morsettiera intelligente utile a ridurre i costi di cablaggio e a lasciare distribuiti sul campo gli I/O di un sistema di automazione e controllo.
	- Gateway: raccolgono le informazioni dai PLC/RTU e si occupano di trasferirle in tempo reale al sistema centralizzato implementando opportuni protocolli di trasmissione, crittografia, sicurezza
	- Front-end server: data lake in cui vengono inviati i dati dai diversi siti e li traducono in un formato utilizzabile dal server Scada. Sono logicamente separati da quest’ultimo a garanzia di un tempo di acquisizione in real-time
	- SCADA server: implementano le funzionalità di monitoraggio e di gestione degli allarmi. Operano in tempo reale tipicamente utilizzando uno stream dati
	- Data server: dove vengono conservati i dati storici per analisi, statistiche e tuning dei sistemi.
- Benefici:
	- **Scalabilità**:  l’incremento del numero degli impianti monitorati e dei segnali raccolti pone il problema dell’enorme mole di dati da gestire (in tempo reale), da memorizzare e da analizzare
	- **Capacità di analisi dei dati raccolti**: ai moduli per il controllo operativo dei sistemi si aggiungono moduli per la valorizzazione dei dati storici raccolti
	- **Sicurezza**: i sistemi SCADA controllano infrastrutture critiche. Il livello di sicurezza dei sistemi commerciali è basato su tecnologie che sono 5-10 anni in ritardo rispetto allo stato dell’arte dell’IT
- Oggi la sicurezza dei sistemi SCADA è basata sul funzionamento su Virtual Private Network separate rispetto a quella aziendale
- Industria 4.0: tendenza all'automazione industriale tramite tecnologie produttive per migliorare le condizioni di lavoro e la produttività
- In generale si cerca di rivoluzionare il modo con cui le cose vengono fatte.
- Le mutazioni tecnologiche devono far ripensare le ottimizzazioni del processo

## Sistemi MES (livello 3)
- Permettono la gestione della fabbrica nel suo complesso, ricevendo ordini dall'ERP, raccolgono informazioni dallo SCADA e forniscono informazioni aggiornate all'ERP
- Funzionalità
	- Assegnazione delle risorse
	- Monitoraggio dello stato
	- Pianificazione delle operazioni
	- Gestione del lavoro e della manutenzione
	- Analisi e ottimizzazione processo
	- Tracciamento del prodotto
	- Gestione della qualità
- Benefici:
	- Riduzione costi e tempi del ciclo di produzione
	- Miglioramento della qualità del prodotto
	- Maggior efficienza di produzione
- Vi è ancora un’enorme distanza tra la forma mentis di un responsabile di fabbrica e di un analista dati
- Molti progetti ottengono budget se si fa riferimento ad applicazioni all’avanguardia (manutenzione predittiva) ma visto il livello di sofisticazione attuale dei sistemi di raccolta dati dagli impianti industriali si può portare valore a un’azienda anche con sistemi di Business Intelligence descrittiva:
	- Calcolo dell'OEE - Overall Equipment Effectiveness
	- Analisi del funzionamento dei macchinari
	- Analisi del consumo dei tool
- Anche mercati tecnologicamente all’avanguardia propongono soluzioni di data analysis limitate:
![[Pasted image 20260922112915.png]]
- Per esempio nel mercato dell'aviazione trova poco spazio l'analisi predittiva dei macchinari in quanto i tempi di processo sono molto stringenti.
## Sistemi ERP (livello 4)
- ERP (*Enterprise Resource Planning*): termine coniato da Gartner Group, indica una suite di moduli applicativi che supportano tutte le fasi dei processi aziendali
- Rappresenta un evoluzione dei sistemi precedenti *MRP*, concentrati solo sull'elaborazione del materiale
- Possono essere orizzontali o verticali
- Permettono l'integrazione di tutte le informazioni di un'azienda
- Si utilizzano per evitare Silos a macchia di leopardo: in un alcuni settori dell'azienda la digitalizzazione era più attuata e non si parlavano/interagivano tra loro.
- Si utilizza in genere un database centralizzato
![[Pasted image 20260923091911.png]]
- Moduli Core: gestiscono i processi aziendali interni ed essenziali.
	- **Finanza e Contabilità (FI/CO):** Il cuore pulsante dell'ERP. Gestisce la contabilità generale, i crediti e i debiti, la tesoreria, la gestione dei cespiti e il controllo di gestione (costi e ricavi).
	- **Gestione delle Vendite e Distribuzione (SD):** Monitora l'intero ciclo attivo, dall'offerta commerciale all'ordine del cliente, fino alla fatturazione e alla spedizione.
	- **Acquisti e Approvvigionamento (MM - Material Management):** Gestisce il ciclo passivo, ovvero i rapporti con i fornitori, le richieste di acquisto, gli ordini di fornitura e il controllo delle fatture ricevute.
	- **Gestione del Magazzino e Logistica (WM):** Monitora i livelli delle scorte, la movimentazione delle merci, l'inventario e lo stoccaggio nei depositi.
	- **Pianificazione e Controllo della Produzione (PP):** Fondamentale per le aziende manifatturiere. Gestisce le distinte base (BOM), i cicli di lavorazione, la pianificazione del fabbisogno di materiali (MRP) e il calendario della fabbrica.
	- **Risorse Umane (HR / HCM Core):** Amministra l'anagrafica dei dipendenti, la rilevazione delle presenze, i turni e l'elaborazione delle buste paga (payroll).
- Moduli ERP esteso:
	- **PLM** (Product Lifecycle Management): supporto alla gestione della documentazione tecnica del prodotto
	- **SCM** (Supply Chain Management): supporto alla pianificazione e controllo delle attività interaziendali
	- **CRM** (Customer Relationship Management): supporto all'interazione con il cliente
	- **E-procurement**: permettono la realizzazione di mercati elettronici interaziendali in cui le condizioni quadro e i fornitori sono definiti dagli uffici acquisti, mentre sono i reparti utenti a seguire  gli acquisti veri  e propri, abbattendo così costi e tempi 
- Il vantaggio dell'integrazione sta nella circolarità dell'informazione
- Caratteristiche principali:
	- Unicità dell'informazione
	- Estensione e modularità funzionale
	- Prescrittività
- Federazione dei sistemi
### Unicità
- La base di dati unica e condivisa permette:
	- Sincronizzazione dati
	- Assenza di ridondanza
	- Tracciabilità dei sistemi
	- Affidabilità dell'informazione aziendale: si evita consistenza interna
- Mancanza di unicità porta inconsistenza e eterogeneità, che porta alla realizzazione di processi ad hoc con diversi giorni di sviluppo
### Estensione e modularità
- L’ampiezza della copertura dei sistemi ERP fa si che questi possano essere utilizzati come unica soluzione per il SI
- La modularità del sistema permette all’azienda di scegliere solo i moduli di interesse.
- Le strategie adottabili sono le seguenti:
	- **Incrementale**: si acquistano progressivamente moduli che nella precedente configurazione erano mancanti o erano realizzati mediante sistemi legacy inadeguati
	- **One stop shopping**: Si predilige la linearità dei moduli di un solo vendor, possibilmente di un'unica *trance*
	- **Best of breed**: vengono utilizzati i moduli di diversi vendor che meglio si prestano alle esigenze dell'azienda o che vengono considerati i migliori.
- Il problema principale è che i moduli non sempre coprono bene quel processo, per questo si passa solitamente a un best of breed, tuttavia questo si porta il problema dell'eterogeneità dell'informazione.
- Solitamente l'integrazione avviene tramite personale interno dell'azienda.
- Tipicamente:
	- Per aziende con funzioni core *di nicchia* si adotta un approccio best of breed in cui si da spazio alla specializzazione per i core e si adotta un unico fornitore per i rimanenti, privilegiando costo e integrazione
	- Per aziende core *generiche* si adotta un unico fornitore per coprire tutte le funzionalità possibili del core, mentre si adotta un best of breed per le rimanenti
### Prescrittività
![[Pasted image 20260923095507.png]]
- E’ necessario far aderire i processi aziendali a quelli definiti nell’ERP
- L’approccio a un progetto ERP è invertito rispetto a quanto avviene nella progettazione di un SI “su misura”.
- L’impatto organizzativo può essere elevato, tuttavia ci sono diversi benefici:
	- I processi ERP sono basati sulle best practices del settore
	- Garantiscono la correttezza e standardizzazione delle operazioni
	- Favorisce la razionalizzazione dei processi, facendo coincidere il progetto informatico ERP con un progetto di **BPR** (Business Process Reengineering)
- I sistemi ERP tuttavia non sono immutabili, esistono personalizzazioni che assottigliano il gap tra funzionalità del modulo e specificità dell'azienda.
- Lock in: rimanere bloccati alle personalizzazioni fatte
- Il programmatore viene destinato alla **parametrizzazione** per fare il tuning dei parametri del software a seconda delle esigenze dell'azienda
### Architettura
- Si basano su architetture client-server, anche se si stanno evolvendo verso schemi thin-client web per supportare la logica di rete
- thin-client: risorse ospitate all'interno del server con cui interagisce
- fat-client: parte delle elaborazioni vengono eseguite in locale
![[Pasted image 20260923100207.png]]

### Impiego in piccole e grandi aziende
- I sistemi ERP funzionano meglio in aziende grandi/medio grandi
- Difficilmente le aziende investono nel settore IT, quindi rimangono ancora poco usati
- Per questo motivo i fornitori si stanno attrezzando per trovare soluzioni convenienti:
	- Versioni downsized
	- Moduli orizzontali semplificati
	- Moduli di fruizione ASP (Application Server Provider): pagare un canone di accesso al posto di acquistarlo


## Sistemi CRM
- **CRM** (*Customer Relationship Management*): Indica il processo integrato e strutturato per la gestione della relazione con la clientela, il cui lo scopo è costruire relazioni con la clientela, puntando a soddisfazione del cliente e aumentare il valore dell'impresa per il cliente e viceversa.
- Il cliente è al centro della strategia commerciale
- Le aziende telefoniche rappresentando il miglior esempio dell'approccio CRM, con servici di call center, SIM e telefonia serviti via web,...
- L'approccio CRM non è possibile senza il supporto informatico
- Il ruolo varia a seconda del settore di attività e ha valore massimo nel caso di relazioni *frequenti* e *continuate* con una clientela *numerosa* che interagisce attraverso diversi canali.
![[Pasted image 20260923103348.png]]

- Esistono tre moduli principali in un sistema CRM
	- *Operativo*: informatizza i canali attraverso cui avvengono i contatti con il cliente
	- *Analitico*: informatizza l'analisi della clientela al fine di definire le politiche di promozione e di contatto
	- *Direzionale*: permette al management di valutare la performance aziendale verso il cliente
- Gli ultimi due si basano sui dati presenti sul DW aziendale
![[Pasted image 20260923103542.png]]
- Cercare di astrarre il sistema. Ex: il CRM operativo si interfaccia con l'ERP
- Se è SAP sono lo stesso sistema, mentre se sono ERP e Sales Force 
- Caratteristiche:
	- *Multicanalità*: servizio erogabile 24/7 sul canale più conveniente (omni-channel)
	- *Completezza e unicità dei dati su prodotti e clienti*: unicità dell'informazione condivisa tra i diversi sistemi
	- *Catene di servizio*: le richieste sottoposte al front-end generano una serie di attività complesse che attivano dei servizi
### Presenza
- Il CRM dà supporto al venditore nel ciclo di individuazione del cliente, di contatto, di trattativa e di ordinazione vera e propria
- Dato che l’automazione è indirizzata soprattutto al venditore si parla di applicazioni **Sales Force Automation** (**SFA**).
- Vengono utilizzati per prodotti complessi, permettendo:
	- Offerte per i clienti
	- Mostrare schede informative
	- Raccogliere ordini e informazioni
### Sistemi automatici
- Il cliente interagisce via telefono con una rete di operatori assistiti dal sistema CRM, e di sistemi automatici integrati con gli apparati telefonici (call-center)
- In generale questa componente permette di:
	- Smistare chiamate
	- Fornire risposte e servizi in maniera automatica
	- Rendere trasparente la posizione dei call-center (multilocalizzazione)
	- Supportare gli operatori

### Web
- Il canale Web permette il commercio elettronico, diventando uno dei canali principali del CRM, rendendo disponibile:
	- Vendite self-service 24/7
	- Possibilità di personalizzazione
	- Evidenziare lo stato degli ordini
	- Registrare il percorso di navigazione del cliente
- Si classificano in:
	- **Business-to-Customer (B2C)**: punti di accesso per i clienti orientati all'acquisto
	- **Business-to-Business (B2B)**: punti di accesso riservato alle aziende e orientato ai servizi
- Attualmente si stanno affermando pacchetti *web-enabled front-end* orientati alla sola fase di personalizzazione
> **SUMMARY**

+-------------+-----------------------------------------------------------+
| Project | Bonfiglioli: SAP Digital Manufacturing Implementation |
| Title | |
+=============+===========================================================+
| Responsible | > Syscons s.r.l. |
+-------------+-----------------------------------------------------------+
| File | Bonfiglioli_EVO_SAP Integrazione_Supervisore.pdf |
+-------------+-----------------------------------------------------------+

> **DOCUMENT VERSION**

+---------+------------+------------+--------------+---------------------------+
| Version | Date | Author | Organization | Notes |
+=========+============+============+==============+===========================+
| 1.0 | 11/09/2025 | Ferramosca | Syscons | |
| | | Andrea | | |
+---------+------------+------------+--------------+---------------------------+
| | | | | - |
+---------+------------+------------+--------------+---------------------------+
| | | | | - |
+---------+------------+------------+--------------+---------------------------+
| | | | | - |
+---------+------------+------------+--------------+---------------------------+

> **APPROVED**

---

Ver. Date Name Team Role Comments

---

1.0 Bonfiglioli

---

> **DISTRIBUTION OF THE DOCUMENT**

---

Name Organization

---

N.A. To the Bonfiglioli contact persons involved in the
project

---

# Summary {#summary .TOC-Heading}

[1. Introduction [5](#introduction)](#introduction)

[1.1. Purpose of the document
[5](#purpose-of-the-document)](#purpose-of-the-document)

[1.2. Abbreviations and acronyms
[5](#abbreviations-and-acronyms)](#abbreviations-and-acronyms)

[1.3. Assumptions [6](#assumptions)](#assumptions)

[2. Architecture [7](#architecture)](#architecture)

[2.1. General Overview [7](#general-overview)](#general-overview)

[2.2. Integration Flows Details
[8](#integration-flows-details)](#integration-flows-details)

[2.3. Integration Types [9](#integration-types)](#integration-types)

[2.4. Authentication [9](#authentication)](#authentication)

[3. Master Data [10](#master-data)](#master-data)

[3.1. Triggers [11](#triggers)](#triggers)

[3.1.1. Digital Manufacturing Trigger
[11](#digital-manufacturing-trigger)](#digital-manufacturing-trigger)

[3.1.2. Supervisore Trigger (refresh)
[12](#supervisore-trigger-refresh)](#supervisore-trigger-refresh)

[3.2. Orders [13](#orders)](#orders)

[3.3. Routing & Operations
[14](#routing-operations)](#routing-operations)

[3.4. Bill of Materials [15](#bill-of-materials)](#bill-of-materials)

[3.5. Materials Classification
[16](#materials-classification)](#materials-classification)

[3.6. Shop Floor Controls
[17](#shop-floor-controls)](#shop-floor-controls)

[3.7. Workcenters [18](#workcenters)](#workcenters)

[4. Transactional Data [19](#transactional-data)](#transactional-data)

[4.1. Synchronous Flows [20](#synchronous-flows)](#synchronous-flows)

[4.2. Asynchronous Flows [21](#asynchronous-flows)](#asynchronous-flows)

[4.3. SFC Start (start operation)
[22](#sfc-start-start-operation)](#sfc-start-start-operation)

[4.4. SFC Complete (end operation)
[23](#sfc-complete-end-operation)](#sfc-complete-end-operation)

[4.5. SFC Sign Off (pause operation)
[24](#sfc-sign-off-pause-operation)](#sfc-sign-off-pause-operation)

[4.6. Labor Tracking (employee movement)
[25](#labor-tracking-employee-movement)](#labor-tracking-employee-movement)

[4.6.1. Start Labor [25](#start-labor)](#start-labor)

[4.6.2. Stop Labor [25](#stop-labor)](#stop-labor)

[4.7. Data Collection (quality parameters sampling)
[26](#data-collection-quality-parameters-sampling)](#data-collection-quality-parameters-sampling)

[4.7.1. Parameters for a serial
[26](#parameters-for-a-serial)](#parameters-for-a-serial)

[4.7.2. Sample parameters [27](#sample-parameters)](#sample-parameters)

[4.8. Serials Coupling/Traceability
[28](#serials-couplingtraceability)](#serials-couplingtraceability)

[4.9. SFC in Work [30](#sfc-in-work)](#sfc-in-work)

[4.10. Order Execution Information
[31](#order-execution-information)](#order-execution-information)

[4.11. Non Conformance [32](#non-conformance)](#non-conformance)

[4.11.1. Create a Non Conformance
[32](#create-a-non-conformance)](#create-a-non-conformance)

[4.11.2. Get Non Conformance by serial
[32](#get-non-conformance-by-serial)](#get-non-conformance-by-serial)

[4.11.3. Check Non Conformance Open
[32](#check-non-conformance-open)](#check-non-conformance-open)

[4.11.4. Close a Non Conformance
[32](#close-a-non-conformance)](#close-a-non-conformance)

[4.11.5. Re-open a Non Conformance
[32](#re-open-a-non-conformance)](#re-open-a-non-conformance)

[4.12. Change Machine Status
[33](#change-machine-status)](#change-machine-status)

[4.12.1. Machine Down [33](#machine-down)](#machine-down)

[4.12.2. Machine Productive
[33](#machine-productive)](#machine-productive)

[5. Appendices & Attachments
[34](#appendices-attachments)](#appendices-attachments)

# Introduction

## Purpose of the document

Lo scopo di questo documento è di fornire una descrizione esaustiva
funzionale delle modalità di integrazione tra il sistema di execution
SAP Digital Manufacturing e il sistema di supervisione di produzione
presente nel plant Bonfiglioli EVO.

L'obiettivo è quello di elencare e descrivere tutti i flussi presenti e
fornire i dettagli riguardo trigger di attivazione, dati in scambio tra
i sistemi e i risultati di ogni integrazione.

Per i dettagli tecnici si rimanda ai documenti allegati, elencati al
paragrafo 5.

## Abbreviations and acronyms

I seguenti termini e acronimi saranno usati nel documento

---

BTP SAP Business Technology Platform

---

MES Manufacturing Execution System

SAP DM SAP Digital Manufacturing -- Soluzione MES in cloud di SAP

SAP IS SAP Integration Suite, soluzione IPaaS di SAP

webMethods Soluzione IPaaS di IBM, prodotto già di proprietà di
Bonfiglioli

IPaaS Integration Platform as a Service

Supervisore Software di supervisione/monitoraggio della produzione del
plant EVO, sviluppato da Giotto Informatica Srl

SAP S/4 Hana Soluzione ERP di SAP

ERP Enterprise Resource Planner

RFC Remote Funcion Call - standard di comunicazione che
permette ai sistemi SAP e non SAP di interagire tra loro

BOM Bill of material (distinta base)

SFC Shop floor control (seriale). Costituisce l'entità cardine
di gestione dell'execution su SAP Digital Manufacturing

ODP Ordine di produzione

---

## Assumptions

Di seguito le assunzioni/premesse su alcune componenti dell'architettura
di integrazione prevista:

- **webMethods** -- la piattaforma di integrazione webMethods è un
  prodotto già presente nello stack architetturale di Bonfiglioli, non
  costituisce quindi oggetto di fornitura di Syscons. Ogni attività di
  customizzazione/sviluppo sulla piattaforma sono a carico di
  Bonfiglioli;

- **Supervisore** -- il sistema di supervisione è già in uso attivamente
  da anni nel plant EVO di Bonfiglioli. Alcune logiche e funzionalità
  del sistema dovranno essere riviste per adattarsi alle metodologie di
  gestione dell'execution del sistema SAP DM. Tutte le attività di
  sviluppo/adattamenti sul sistema sono a carico di Giotto Informatica
  Srl;

# Architecture

## General Overview

L'integrazione tra il sistema Supervisore e il sistema SAP DM avverrà
mediante la piattaforma d'integrazione già in possesso di Bonfiglioli
webMethods.

Tutti i flussi tra i due sistemi dovranno transitare da questo
middleware.

Probabilmente, per difficoltà/complessità di implementazione,
continueranno ad esistere alcune integrazioni "dirette" tra il
Supervisore e SAP S4/Hana.

Per armonizzare il tutto alla nuova architettura software prevista, c'è
la volontà a tendere di mappare anche queste comunicazioni sul
middleware webMethods.

Questo per garantire che tutte le chiamate da shopfloor verso sistemi di
più alto livello transitino mediante il layer webMethods che garantisce
più monitoraggio e controllo.

Dal punto di vista tecnico, l'integrazione sarà effettuata mediante
chiamate HTTP verso API REST esposti dai vari sistemi in scope.

Le API esposte dal sistema Digital Manufacturing saranno descritte nei
successivi paragrafi contestualmente alle entità/funzionalità di
riferimento.

Le APIs REST esposte da webMethods saranno descritte in altri documenti
o in futuro integrata in questo stesso.

## Integration Flows Details

Di seguito uno schema a blocchi di rappresentazione dell'architettura in
essere

![](media/image1.png){width="6.6930555555555555in"
height="4.601388888888889in"}

Le tipologie di integrazione potranno quindi essere:

- DM → Supervisor - flussi il cui trigger è dato dal sistema SAP DM.
  Ricadono nella funzionalità di sincronizzazione master data;

- Supervisor → DM - flussi di carattere transazionale il cui trigger è
  dato dal sistema Supervisore. Si generano durante l'esecuzione della
  produzione. Potranno essere gestiti in modo sincrono o asincrono;

- Supervisor → Integration Suite - flussi di carattere transazionale il
  cui trigger è dato dal sistema Supervisore. Hanno una ripercussione su
  ERP ma che esulano dalle funzionalità/oneri di DM;

- Supervisor → S4 - flussi di carattere transazionale il cui trigger è
  dato dal sistema Supervisore. Sono direttamente instaurati tra ERP e
  supervisore mediante chiamate RFC. L'intento è quello di eliminare
  gradualmente questa tipologia di integrazione diretta, ma può essere
  che alcune interfacce vengano mantenute per difficoltà
  tecniche/applicative di migrazione iniziali;

## Integration Types

I flussi di integrazione possono essere classificati in due
macro-famiglie:

- master data -- integrazioni necessarie per l'allineamento e
  sincronizzazione delle anagrafiche che i sistemi in scope condividono
  per l'esecuzione e monitoraggio della produzione;

- transazionali -- integrazioni che si generano via via che la
  produzione viene eseguita sullo shopfloor. Hanno al loro interno dei
  loro messaggi dei dati che comportano l'avanzamento/aggiornamento
  delle varie entità di execution nel loro ciclo di vita;

## Authentication

Il consumo delle APIs REST esposte da DM richiede un'autenticazione
attraverso un flusso OAuth2 Client Credentials.

L'autenticazione del client da parte di DM viene effettuata mediante la
validazione di un JWT token incluso nell'header "Authorization" nelle
chiamate http fatte verso i suoi web services.

Il JWT token deve essere richiesto precedentemente ad un authorization
server attraverso una specifica richiesta http, specificando al suo
interno client id e secret da utilizzare.

Il token ha una scadenza dopo un determinato periodo di tempo, è onere
del client richiedere un nuovo token una volta scaduto.

Tutti gli endpoint, id, secret e le configurazioni necessarie ad
instaurare il flusso di autenticazione sono reperibili dalla SAP BTP,
consultando i dettagli di una specifica service key Digital
Manufacturing istanziata alla sezione "_Instances and Subscription_".

Nell'Allegato A saranno comunque fornite tutti i parametri necessari.

# Master Data

Le integrazioni descritte in questo paragrafo sono necessarie per
l'allineamento e sincronizzazione delle anagrafiche che i sistemi in
scope condividono per l'esecuzione e monitoraggio della produzione.

Le entità di master data che SAP DM e il Supervisore dovranno
condividere e sincronizzare sono:

- ordini di produzione

- cicli e operazioni degli ordini (routing & operations)

- distinte base (BOM)

- seriali (SFCs)

L'anagrafica dei centri di lavoro sarà manutenuta manualmente sul
supervisore, SAP DM comunque espone in caso di necessità l'API per avere
i workcenter configurati sul sistema (descritto al relativo paragrafo).

## Triggers

### Digital Manufacturing Trigger

Per garantire una sincronizzazione più puntuale, ottimizzata e orientata
all'evento, normalemente il trigger dei flussi di sincronizzazione
master data sarà dato da DM in fase di rilascio di un nuovo ordine di
produzione.

Tramite una Business Rule opportunamente configurata, SAP DM notificherà
al rilascio di un ordine il middleware webMethods chiamando una API REST
esposta sulla piattaforma.

Questa chiamata avrà nel payload i seguenti dati minimi:

- plant → codice identificativo del plant in cui si è rilasciato
  l'ordine

- order → codice identificativo dell'ordine rilasciato

Attraverso questi due dati di partenza, webMethods sarà in grado di
chiamare una serie di APIs REST esposte da DM per recuperare tutti i
dati di anagrafica necessari al Supervisore.

Sarà compito di webMethods formattare i vari risultati ottenuti dalle
chiamate HTTP in un formato interpretabile dal Supervisore
(JSON/XML/..).

Di seguito schematizzata la sequenzialità degli scambi tra sistemi

![](media/image2.png){width="6.6930555555555555in" height="4.6875in"}

### Supervisore Trigger (refresh)

Per garantire il flusso anche in caso di mancato trigger su rilascio
ordine da DM, il Supervisore potrà richiamare un endpoint su webMethods
che performi a richiesta il reperimento delle info di anagrafica da DM.

Un potenziale esempio di richiamo è quando mediante supervisore viene
letto l'ordine di produzione o un suo seriale dalla bolla di lavoro.

Nel caso in cui il sistema non conosca ancora il codice letto,
richiederebbe l'aggiornamento delle anagrafiche mediante questa chiamata
http.

Da valutare se l'endpoint chiamato sarà lo stesso consumato da DM in
fase di trigger su rilascio ordine o uno ad-hoc richiamato solo dal
Supervisore.

In risposta il Supervisore otterrà gli stessi dati (stesso formato) come
se il trigger fosse partito da DM in fase di rilascio.

Di seguito schematizzata la sequenzialità degli scambi tra sistemi per
questo scenario

![](media/image3.png){width="6.6930555555555555in"
height="4.679861111111111in"}

## Orders

Le informazioni relative ad un ordine di produzione possono essere
ottenute mediante l'API GET /order/v1/orders.

Doc SAP: *https://api.sap.com/api/sapdme_order/path/get_v1_orders*

Questa API ritorna gli identificativi di ciclo (property _routing_) e
distinta base (property _bom_) associate all'ordine, utilizzabili
successivamente per ottenere il dettaglio degli oggetti.

Ritorna inoltre tutti gli id dei seriali (property _sfcs_) dell'odp, nel
caso fosse l'unica info necessaria per l'anagrafica seriali è
recuperabile direttamente da qui. Invece fossero necessari i dettagli di
un determinato, seriale è possibile utilizzare l'API dedicata
specificando l'id SFC relativo.

### Order info mapping

Nella seguente tabella sono mappate le informazioni salvate nella
tabella di anagrafica del supervisore con i relativi campi sorgente di
SAP DM.

In "Nome Campo" è specificata il nome della colonna sul quale il campo è
salvato in tabella supervisore.

Per "Sorgente DM" si intende il puntamento all'ogetto JSON ritornato
dalla relativa REST API che contiene il valore corrispondente, si
rimanda all'Allegato A per esempi di struttura completa dell'oggetto.

---

Nome Campo Descrizione Sorgente DM

---

Numero Identificativo ordine order

Data plannedStartDate

DataConsegna plannedCompletionDate

CodParte Identificativo materiale di material.material
testata ordine

DesParte Descrizione materiale di material.description
testata ordine

Quantita Quantità da produrre productionQuantity
dell'ordine

CodCliente Identificativo del cliente di  
 destinazione dell'ordine

DescCliente Descrizione del cliente di  
 destinazione dell'ordine

IsRework Flag ordine di rework

SitoProd Codice plant di riferimento plant

---

## Routing & Operations

Le informazioni relative ad un ciclo standard e relative operazioni
possono essere ottenute mediante l'API GET /routing/v1/routings.

Doc SAP:
[_https://api.sap.com/api/sapdme_routing/path/findRoutingByPlantAndNameAndTypeUsingGET_1_](https://api.sap.com/api/sapdme_routing/path/findRoutingByPlantAndNameAndTypeUsingGET_1)

Oltre alle informazioni "header" del routing, questa API ritorna anche i
dettagli di tutte le operazioni che lo compongono alla property
_routingSteps_

Tutti i parametri obbligatori da fornire sono ritornati dall'API di
dettaglio ordine eventualmente chiamata in precedenza.

### Routing info mapping

Nella seguente tabella sono mappate le informazioni salvate nella
tabella di anagrafica del supervisore con i relativi campi sorgente di
SAP DM.

In "Nome Campo" è specificata il nome della colonna sul quale il campo è
salvato in tabella supervisore.

Per "Sorgente DM" si intende il puntamento all'ogetto JSON ritornato
dalla relativa REST API che contiene il valore corrispondente, si
rimanda all'Allegato A per esempi di struttura completa dell'oggetto.

---

Nome Campo Descrizione Sorgente DM

---

CDL Identificativo dentro di routingSteps\[n\].workCenter.workCenter
lavoro

CodiceFase routingSteps\[n\].stepId

NumeroConferma

Descrizione Descrizione routingSteps\[n\].description
dell'operazione

IsFaseQuantita

---

3.

## Bill of Materials

Le informazioni relative ad una distinta materiale possono essere
ottenute mediante l'API GET /bom/v1/boms.

_Doc SAP_:
<https://api.sap.com/api/sapdme_bom/path/findBomByPlantAndNameAndTypeUsingGET>

Alla property _components_, sono elencati le informazioni di tutti i
materiali che compongono la distinta base.

Tutti i parametri obbligatori da fornire sono ritornati dall'API di
dettaglio ordine eventualmente chiamata in precedenza.

## Materials Classification

Per ottenere le classificazioni e relative caratteristiche di un
determinato materiale, è possibile utilizzare l'API POST
/classification/v1.

_SAP Docs_: https://api.sap.com/api/sapdme_classification/path/post_read

## Shop Floor Controls

I dettagli relativi ad uno specifico seriale sono ritornati dall'API GET
/sfc/v1/sfcdetail.

_Doc SAP_:
[_https://api.sap.com/api/sapdme_sfc/path/get_sfcdetail_](https://api.sap.com/api/sapdme_sfc/path/get_sfcdetail)

Rilevanti potrebbero essere le informazioni di quantità (property
_quantity_), stato corrente (property _status_), informazioni del
seriale tra le varie operazioni del ciclo come quantità
confermata/scartata (_quantityDone/quantityScrapped_) e operazione
eseguita (_stepDone_).

### SFC info mapping

Nella seguente tabella sono mappate le informazioni salvate nella
tabella di anagrafica del supervisore con i relativi campi sorgente di
SAP DM.

In "Nome Campo" è specificata il nome della colonna sul quale il campo è
salvato in tabella supervisore.

Per "Sorgente DM" si intende il puntamento all'ogetto JSON ritornato
dalla relativa REST API che contiene il valore corrispondente, si
rimanda all'Allegato A per esempi di struttura completa dell'oggetto.

---

Nome Campo Descrizione Sorgente DM

---

Seriale Identificativo seriale sfc

---

Dato che l'unico campo manutenuto in anagrafica seriali dal supervisore
è il codice identificativo, esso può essere reperito direttamente dalla
chiamata effettuata per conoscere gli ordini (3.2) alla proprietà
_sfcs_.

## Workcenters

I dettagli dei workcenters (centri di lavoro) manutenuti nell'anagrafica
Digital Manufacturing sono ottenibili mediante l'API GET
/workcenter/v2/workcenters

_Doc SAP_:
https://api.sap.com/api/sapdme_plant_workcenter_v2/path/readUsingGET

# Transactional Data

Le integrazioni descritte in questo paragrafo nascono via via che la
produzione viene eseguita sullo shopfloor.

Hanno al loro interno dei loro messaggi dei dati che comportano
l'avanzamento/aggiornamento delle varie entità di execution nel loro
ciclo di vita.

Queste transazioni possono essere gestite in modo sincrono o asincrono.

Data la possibilità di consumare servizi in modo sincrono e di sfruttare
direttamente eventuali risposte/feedback nel sistema chiamante
(supervisore), ci sentiamo di consigliare questo approccio come quello
regolarmente utilizzato nella normale operatività.

Il flusso asincrono potrebbe essere un "pattern di emergenza" nel caso
in cui i sistemi nel cloud (webMethods e DM) non siano disponibili per n
ragioni.

Questo garantirebbe la continuità di servizio della produzione per gli
ordini/entità fino a quel momento presenti nelle anagrafiche del
supervisore.

Le modalità di scodamento transazioni verso il cloud dovrebbero comunque
essere approfondite per garantire una consistenza di gestione tra
supervisore e SAP DM.

## Synchronous Flows

Di seguito schematizzata la sequenzialità degli scambi di carattere
transazionale sincrono tra sistemi

![](media/image4.png){width="6.6930555555555555in"
height="4.697916666666667in"}

Essendo l'intero flusso (supervisore 🡪 webMethods 🡪 DM) racchiuso nel
contesto di una medesima chiamata HTTP, il supervisore avrebbe come
risposta direttamente il risultato dell'operazione effettuata sul
sistema remoto in cloud.

## Asynchronous Flows

Nel caso di una gestione asincrona, il supervisore accoderà su apposite
tabelle le transazioni da gestire verso webMethods (quindi di
conseguenza verso DM), contenenti tutte le informazioni necessarie per
il processamento.

Ogni transazione verrà quindi gestita in modo asincrono mediante job
schedulati a tempo.

Di seguito schematizzata la sequenzialità degli scambi di carattere
transazionale asincrono tra sistemi

![](media/image5.png){width="6.6930555555555555in"
height="4.601388888888889in"}

La risposta ritornata da DM determinerà l'aggiornamento dello stato
della transazione, in modo da determinare se andata a buon fine oppure
no e gestire eventualmente un riprocessamento.

## SFC Start (start operation)

Per segnalare l'avvio della lavorazione di un determinato seriale su una
specifica operazione del ciclo di lavoro è utilizzabile l'API POST
/sfc/v1/sfcs/start

_SAP Doc_: <https://api.sap.com/api/sapdme_sfc/path/post_sfcs_start>

## SFC Complete (end operation)

Per segnalare l'avvenuto completamento della lavorazione di un
determinato seriale su una specifica operazione del ciclo di lavoro è
utilizzabile l'API POST /sfc/v1//sfcs/complete

_SAP Doc_: <https://api.sap.com/api/sapdme_sfc/path/post_sfcs_complete>

## SFC Sign Off (pause operation)

Per segnalare la pausa della lavorazione di un determinato seriale su
una specifica operazione del ciclo di lavoro è utilizzabile l'API POST
/sfc/v1//sfcs/signoff

_SAP Doc_: <https://api.sap.com/api/sapdme_sfc/path/post_sfcs_signoff>

## Labor Tracking (employee movement)

Per segnalare l'inizio della lavorazione presso un centro di lavoro di
un operatore (nonchè la fine) è utilizzabile il set di API REST per la
Time Tracking di DM.

_SAP Doc_: https://api.sap.com/api/sapdme_timetracking/overview

Molto probabilmente il movimento utilizzato per tracciare le presenze
operatori sui cdl sarà l'Indirect Labor di DM, ma ci riserviamo una
risposta definitiva dopo un ulteriore analisi.

Attualmente questa funzionalità è fatta direttamente su interfaccia MES.
L'obiettivo di progetto è farla fare sulla UI del Supervisore, per avere
un sistema di interazione unico. Da lì il flusso delle informazioni
ricadrà nel flusso architetturale definito (Supervisore → WebMethods →
SAP DM).

### Start Labor

L'avvio di un movimento labor è effettuabile mediante API
/timetracking/v1/indirect-labor/start

SAP Doc:
<https://api.sap.com/api/sapdme_timetracking/path/indirectLaborStart>

### Stop Labor

La fine di un movimento labor è effettuabile mediante API
/timetracking/v1/indirect-labor/stop

_SAP Doc_:
<https://api.sap.com/api/sapdme_timetracking/path/indirectLaborStop>

## Data Collection (quality parameters sampling)

### Parameters for a serial

Per ottenere le misurazioni/rilevamenti da effettuare per un determinato
seriale è possibile utilizzare l'API GET /datacollection/v1/sfc/groups.

SAP Doc:
<https://api.sap.com/api/sapdme_datacollection/path/findDataCollectionGroupsUsingGET>

Alla proprietà group.parameter sono elencati tutti i parametri da
collettare per il determinato seriale.

Utili potrebbero essere le proprietà _allDataCollected_ (true se tutti i
parametri sono stati collettati) e _sfcDataCollectionStatus_, oggetto
che riporta informazioni riguardo lo stato del campionamento.

### Sample parameters

Per effettuare il campionamento di un set di parametri di qualità per un
seriale su un centro di lavoro è possibile utilizzare l'API POST
/datacollection/v1/log.

_SAP Doc_:
<https://api.sap.com/api/sapdme_datacollection/path/logDataCollectionUsingPOST>

## Serials Coupling/Traceability

Questa procedura operativa, che serve che tracciare i seriali di n
componenti utilizzati nella produzione di un determinato seriale di
prodotto finito, non è coperta da servizi standard DM.

Attualmente il fine di questa procedura è chiamare SAP ERP (mediante RFC
custom) specificando la filiazione dei seriali letti in linea. S4
persiste queste informazioni su una tabella custom per permettere la
tracciabilità a fronte di eventuali reclami/non conformità.

La proposta è quella di mantenere la gestione finale su S4 inalterata,
mappando la RFC custom da richiamare sulla Integration Suite.

Integration Suite esporrà poi a sua volta un API REST richiamabile da
middleware WebMethods in modo tale da mantenere la conformità dei vari
layers dei segnali shopfloor-topfloor.

Ipoteticamente, il payload che dovrà essere specificato per permettere
la tracciabilità di questi seriali, potrebbe essere il seguente
(necessaria l'analisi della RFC custom attualmente utilizzata)

{

\"plant\": \<string\>,

\"resource\": \<string\>,

\"operation\": \<string\>,

\"sfc\": \<string\>,

\"childrenSfcs\": \[

\<string\>,

\<string\>,

\<string\>,

\...

\]

}

In definitiva per DM dovrebbe essere un flusso completamente
trasparente, come schematizzato in figura

![](media/image6.png){width="6.6930555555555555in"
height="4.623611111111111in"}

Da valutare se far transitare il flusso anche su Integration Suite o
meno, webMethods potrebbe anche chiamare direttamente S4.

Passando da Integration Suite si rispetterebbe la "convezione
architetturale" secondo la quale tutti i flussi da/a S4 dovrebbero
passare tramite essa. Questo sicuramente andrebbe ad aggiungere un layer
di complessità in più che dovrebbe poi essere manutenuto e ad aumentare
i tempi tecnici di processamento.

## SFC in Work

Per sapere quali sono i seriali in corso di lavorazione su un
determinato centro di lavoro è possibile richiamare l'API GET
/sfc/v1/sfcsInWork.

_Doc SAP_: <https://api.sap.com/api/sapdme_sfc/path/get_sfcsInWork>

## Order Execution Information

Per avere informazioni riguardo l'esecuzione di un determinato ordine di
produzione, incluse tutti i dettagli dei suoi seriali figli, è possibile
utilizzare l'API GET /worklist/orders.

_SAP Docs_:
<https://api.sap.com/api/sapdme_sfc/path/getOrderWorkListUsingGET>

Attraverso I dati ritornati dall'API, è possibile capire a che punto si
trova l'esecuzione di ogni seriale all'interno dell'ordine ed
eventualmente calcolare quantità prodotte complessive dell'ordine per
ogni fase.

Questi dati potrebbero essere usati dal supervisore come base di
partenza per i controlli di fase.

## Non Conformance

### Create a Non Conformance

Per aprire una nuova non conformità su un seriale a fronte di un difetto
riscontrato durante la produzione e far partire il flusso di
rilavorazione/riparazione è possibile utilizzare l'API POST
/nonconformance/v1/log. La non conformità sarà creata con stato di
default "OPEN".

_SAP Doc_:
https://api.sap.com/api/sapdme_nonconformance/path/logNonConformanceUsingPOST_1

### Get Non Conformance by serial

Per conoscere le informazioni delle eventuali non conformità (aperte e
chiuse) per un determinato seriale è possibile utilizzare l'API GET
/nonconformance/v2/nonconformances.

_SAP Doc_:
https://api.sap.com/api/sapdme_nonconformance/path/getNonConformancesUsingGET_2

### Check Non Conformance Open

Per verificare se esistono eventuali non conformità ancora aperte per un
determinato seriale è possibile utilizzare l'API POST
/nonconformance/v1/checkForOpen

_SAP Doc_:
https://api.sap.com/api/sapdme_nonconformance/path/checkForOpenNonConformancesUsingPOST

### Close a Non Conformance

Per chiudere una non conformità aperta in precedenza è sufficiente
consumare l'API POST /nonconformance/v1/close.

_SAP Docs_:
<https://api.sap.com/api/sapdme_nonconformance/path/closeNonConformanceUsingPOST>

### Re-open a Non Conformance

Per ri-aprire una non conformità in precedenza già chiusa, è possibile
utilizzare l'API POST /nonconformance/v1/open,

_SAP Docs_:
<https://api.sap.com/api/sapdme_nonconformance/path/openNonconformanceUsingPOST>

## Change Machine Status

### Machine Down

Per dichiarare un fermo su un determinato centro di lavoro, che lo rende
in qualche modo indisponibile, è utilizzabile l'API POST
/resource/v2/changestatus.

SAP Doc:
<https://api.sap.com/api/sapdme_plant_resource_v2/path/changeResourceStatusUsingPOST_1>

### Machine Productive

Per dichiarare una macchina operativa, la quale magari in precedenza era
stata dichiarata in fermo, è utilizzabile sempre la stessa API l'API
POST /resource/v2/changestatus.

Ovviamente bisognerà consumarla con un payload specifico e differente
rispetto a quanto fatto per la dichiarazione del fermo macchina.

# Appendices & Attachments

La documentazione che integra dal punto di vista tecnico quanto
descritto in questo documento sono i seguenti:

- Bonfiglioli_EVO_SAP Integrazione_Supervisore_Allegato A -- si rimanda
  a questo allegato per ogni specifica tecnica di integrazione ed esempi
  pratici di consumo dei servizi REST eposti da Digital Manufacturing
  utili nei flussi di comunicazione da/a supervisore;

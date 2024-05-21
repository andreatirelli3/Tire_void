# Agile Unified Process
Deriva dal **Unified Process** ma adattato per **Agile**:
- **+ iterazioni**, da 6-8 => 18-22;
- **+ corte**, da 2-3 mesi => 2-5 settimane.
## Principi di AUP
1. Lo staff **sa** quello che fa, invece che una documentazione dettagliata si predilige una guida ad alto livello ma con sessioni di training tempo dopo tempo;
2. **Semplicità**, documentazione concisa;
3. **Agilità**, sposare i principi di #Agile;
4. **Attività di alto valore**, solo attività che contano, non ogni singola cosa che può succedere al progetto;
5. **Indipendenza degli strumenti**, usare qualsiasi strumento che il lavoratore gli aggrada;
6. **Prodotto personalizzabile**, il prodotto deve essere facilmente personalizzabile via HTML o Markdown, senza l'acquisto di tool esterni.
## Cicli e fasi di AUP
> 4 Cicli divisi in 4 fasi
1. **Inception** (1/8 del tempo), fase di studio dei requisiti;
2. **Elaboration** (1/4 del tempo), definizione dell'architettura e scelta dei requisiti;
3. **Construction** (1/4 del tempo), implementazione e vari test di integrazione e qualità;
4. **Transition** (1/8 del tempo), rilascio e documentazione.
# Scrum
> *Lo sviluppo è diviso in piccoli **sprint** dove ognuno di questi produce un #prodotto-funzionale L'interazione faccia a faccia è la forma più efficiente di interagire.*

![[Schermata del 2024-05-21 11-32-34.png]]
Ruoli:
- **Product owner**; accetta il lavoro, parla con gli *stakeholders* e prende decisioni;
- **Scrum master**; difende il team e gestisce i problemi interni (NON di sviluppo);
- **team**; composti da 5-9 membri, autonomi e non specializzati (ognuno sa fare più cose) e prendono decisioni **operative** - come sviluppare.
## Meetings
- [[#Sprint review]]
- [[#Daily scrum]]
- [[#Backlog grooming]]
- [[#Scrum of scrum]]
- [[#Sprint review]]
- [[#Sprint retrospective]]
### Sprint planning
> Obiettivo => selezionare le **funzionalità** da sviluppare nello *sprint*.

Ciò produce lo **sprint backlog**. Partecipano:
- Product owner;
- Scrum master;
- Team member.
Dura 8 ore e si pianifica per 4 settimane.
### Daily scrum
> Obiettivo => identificare i problemi, MA NON RISOLVERLI.

Partecipano *product owner*, *scrum master* e *team member*. Dura circa 15 minuti (in piedi) ed è svolto ogni giorno.
### Backlog grooming
> Obiettivo => valutare i **requisiti** e il [[#Product backlog]].

*Riservato solo per i membri del team*. Deve durare massimo il 10% di tutto lo sprint.
- [planning poker](https://en.wikipedia.org/wiki/Planning_poker), *"chiamato anche Scrum Poker, è una tecnica ludicizzata basata sul consenso per la stima, utilizzata principalmente per il timeboxing nei principi Agile."*
### Scrum of scrum
> Obiettivo => discutere **intra team**.

Svolto da **un solo membro** del team. Le domande possono essere su *overlapping*, *ingratiation* e *collaboration*.
### Sprint review
> Obiettivo => presentare i **risultati** dello sprint concluso.

Generalmente viene presentato il #prodotto-funzionale ovvero una demo. Tutti sono coinvolti (owner, scrum master, team, stakeholder, etc). Dura circa 2 ore e si discute anche su come muoversi successivamente.
### Sprint retrospective
> Obiettivo => autovalutazione del team.

Scrum master e membri del team partecipano a questa attività.
## Artifacts
Sono i #prodotto-funzionale risultato di ogni sprint o [[#Meetings]].
### Product backlog
> Lista ordinata dei **requisiti** del prodotto.

Il *product owner* assegna una **priorità** ad ogni requisito.
### Sprint backlog
> Lista degli **oggetti** che devono essere completati per il *prossimo split*.

Derivato dagli elementi in **testa** del [[#Product backlog]] e rappresenta la **quantità** di lavoro che il team può gestire per lo sprint.
> Gli **oggetti** sono divisi in #task. Ogni #task deve durare dalle **4 alle 16 ore massimo**. I task non sono assegnati da qualcuno, ma vengono presi in modo autonomo durante il [[#Daily scrum]].
### Increment
> Risultato della somma di tutti gli **oggetti** presenti nel [[#Product backlog]] terminati nello sprint corrente.

Deve produrre un #prodotto-funzionale **eseguibile**.
### Burn down
> Mostra il lavoro **rimanente** nel **tempo**. Utile per stimare la fine dei lavori.
# Feature Driven Development - FDD
![[Schermata del 2024-05-21 14-31-59.png]]
## 1 - Sviluppo del modello generale #sequenziale
*Basato sul **Domani Driver Modelling***.
> Definire un modello iniziale sufficiente per iniziare il progetto.
- **Team**: Chief architect, Domain expert e Chief programmers;
- #Artifacts: UML e note varie.
## 2 - Lista delle features #sequenziale
*Basato sugli #Artifacts prodotti.*
> Per ogni elemento, viene trasformato in **business value** e tradotto in **features**.

Sintassi <\action\><\result\><\object\> del tipo: *Calcolare il totale di una fattura.*
Il team è composto da **Chief architect** e **Chief programmers**, dove **time-boxano** ogni attività (ricordiamo essere #collaborativa).
> Viene prodotto una **lista di features**.
## 3 - Plan by features #sequenziale
> Definire la **sequenza di sviluppo** e il **piano di sviluppo**.

Il team è composto da **Development manager** e **Chief programmers**. Il Chief programmers si prendere in carico una o più features che poi assegnerà ai programmatori.
## 4 - Design by features #iterative
> Design della features selezionata.

Selezione delle features che possono essere realizzate in **2 settimane**.
- #Artifacts  [[#Design package]] di ogni features selezionata.
### Design package
- covering memo;
- **requisiti** di riferimento;
- diagramma sequenziale;
- design alternativi
- modello dell'oggetto.
## 5 - Build by features #iterative 
> Produrre il codice della feature.

Il codice deve essere **testato** e il *Chief programmer* decide quali classi sono promosse.
- #Artifacts classi testate e funzionanti della feature.
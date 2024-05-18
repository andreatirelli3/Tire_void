# Models
*Un processo produttivo sono una serie di step o fasi necessarie per lo sviluppo, esempio **[[#Specifiche]]**, **[[#Design]]**, **[[#Implementazione]]**, **[[#Verifica e Validazione]]** e **Manutenzione**.*
> #Modello è una **rappresentazione astratta** del processo.
## Specifiche
Definisce i seguenti scopi:
- **requisiti** del cliente;
- **vincoli** del sistema e di sviluppo.
\*(espliciti/impliciti)
### Processo di ingegnerizzazione delle specifiche
![[Schermata del 2024-05-18 10-14-52.png]]
## Design e Implementazione
> Dalle specifiche del sistema al sistema funzionante. 

Design e implementazione sono **correlate** tra di loro perché il #design *realizza l'architettura che soddisfa i requisiti del sistema nei vari livelli*, invece l' #implementazione *traduce in codice quest'ultima*.  Nonostante questa relazione si cerca di tenerle **separate**.
### Design
![[Schermata del 2024-05-18 10-25-24.png]]
Tipicamente il #design del sistema è documentato da modelli grafici:
- **UML** (class diagram e interaction diagram);
- **Entity Relation** (ER);
- **Data-Flow Diagram** (DFD).
### Implementazione
#implementazione Paradigma di **sviluppo** e **test**. Ogni sviluppatore testa le proprie implementazioni e corregge eventuali bug.
![[Schermata del 2024-05-18 10-29-38.png]]
\* Generalmente chi scrive i test è una persona diversa da chi scrive la funzionalità.

## Verifica e Validazione
> #verifica obiettivo, dimostrare che il sistema ***rispecchi i requisiti*** e ***soddisfi i vincoli***.

La fase di test richiede che il sistema sia eseguito su vari **casi di test** che sono derivati dalle specifiche. Le operazioni di test si possono dividere in:
- #testing in **small** - singole unità;
- #testing in **large** - intero sistema.
### Testing in the small
> #testing #obiettivo verificare la correttezza del codice.

Il focus è classi e metodi, fornendo un determinato input, si verifica la correttezza dell'output prodotto. Si può anche chiamare **White box testing**.
### Testing in the large
> #testing #obiettivo verificare il comportamento del sistema complessivo rispecchi le specifiche.

Viene definito come **Blackbox testing**.
### Processo di testing

![[Schermata del 2024-05-18 10-41-02.png]]
Tipi di #testing :
- **Unit** #component-testing , una singola unità;
- **Module** #component-testing , ogni modulo;
- **Sub-system** #integration-testing , integrazione dei moduli formando dei sotto-sistemi;
- **System** #integration-testing , sistema generale;
- **Acceptance**, insieme al cliente si verifica l'accettazione del sistema;
- Other test.
![[Schermata del 2024-05-18 10-54-48.png]]
### Regression test
Una modifica o una fix può impattare sul comportamento della singola unità o modulo, oppure su tutto il sistema. Questo tipo di #testing verifica la corretta funzionalità delle parti non modificate.
# Process models
- [[#Waterfall]];
- [[#Component based]];
- #modelli-iterativi [[#Evolutionary]];
- #modelli-iterativi [[#Incremental]];
- Transformational.
> #not-a-model code and fix, un "paradigma" più che un modello, il quale si programma prima tutto il sistema poi si pesa a fare gli opportuni fix.

#modelli-iterativi I modelli sequenziali hanno un problema di **rigidità** per i cambiamenti. Per questo può essere utile rifare la stessa fase più volte, per permettere la revisione della fase produttiva ed eventualmente sviluppare modifiche e riprodurre la fase stessa.
## Waterfall
![[Schermata del 2024-05-18 11-21-03.png]]
- Vantaggi: processi ben definiti, tempo stimabile in modo preciso e adatto a qualsiasi tipo di progetto;
- Svantaggi: cambiamenti difficili da gestire, rigido e le fasi non sono flessibili.
### Waterfall con feedback
> Questa iterazione di *waterfall* cerca di migliorare le problematiche del modello principale, aggiungendo alla fine di ogni processo una fase di feedback per ritornare alla fase precedente.
![[Schermata del 2024-05-18 11-59-41.png]]
## Component based
> Sfrutta il riuso dei #componenti software (in house oppure Commercial-of-the-Shelf) e del modello di programmazione ad oggetti #OOP
![[Schermata del 2024-05-18 12-06-34.png]]

- Vantaggi: *non c'è bisogno di reinventare la ruota* e i componenti sono testati. 
- Svantaggi: i requisiti devono essere adattati ai componenti e il codice *colla* può risultare più difficile dei componenti stessi.
## Evolutionary
![[Schermata del 2024-05-18 12-18-44.png]]
- Vantaggi, flessibile e si può adattare ai cambiamenti dei requisiti.
- Svantaggi, lo sviluppo è difficile da ispezionare e i calcolo dei tempi è difficile.
Questo modello è applicabile a sistemi grandi, sistemi a vita breve e sistemi di sviluppo sconosciuto.
## Incremental
> Lo sviluppo di tutto il sistema è diviso in #incrementi
![[Schermata del 2024-05-18 12-27-21.png]]

*Gli step dal **Develop system increment** al **Validate integration** sono iterati fino all'arrivo della Final release.*
- Vantaggi: le funzionalità sono pronte prima per il cliente, la prima release può servire per far uscire nuove funzionalità, basso rischio e le funzionalità principali sono testate più volte.
- Svantaggi: non si sa a priori il numero di iterazioni ed è difficile stimare la data di rilascio del prodotto finale.
### Differenza tra Evolutionary e Incrementa
![[Schermata del 2024-05-18 12-32-01.png]]

# DevOps
![[Schermata del 2024-06-07 09-40-36.png]]
> *Ecosistema di **strumenti** per automatizzare il ciclo di vita dello sviluppo software.*
> **Dev**elopers e **Op**eration**s**.

Aumenta l'**efficienza** riducendo il time to market e la qualità del prodotto. Sfoca le operazioni, sviluppo e tester, perché tutte le persone sono coinvolte nello sviluppo del software.
## as Methodology
> *Il suo goal è quello di accelerare la consegna del prodotto.*

- **Benefits**, riduce il time to market e rispecchia meglio le esigenze dei clienti.
- **Culture of Collaboration**, rompere i **silos** tra sviluppo e team operativi.
## as Profession
> *Ingegnere responsabile della affidabilità del prodotto che della sua velocità di sviluppo.*

- **Multidisciplinary Role**, comprendere ruoli che prima erano volti da figure singole - (amministratori, ingegneri di rilascio e sviluppatori).
- **Key Responsibilities**, Continuous Integration (CI) e Continuous Delivery (CD) per gestire il build e deployment automatico.![[Schermata del 2024-06-07 09-53-51.png]]
## Workflow
- Allineamento sugli obiettivi.
- Miglioramento della collaborazione tra gruppi.
- Miglioramento continuo.
- Delivery continuo
	- Le release delle nuove funzionalità e aggiornamenti per gli utenti sono veloci e frequenti.
## Limitazioni ruoli tradizionali
- Complicati.
- Gli sviluppatori devono conoscere l'ambiente.
- Idee innovative sono difficili da far emergere.
- I tester hanno una piccola idea delle funzionalità necessarie.
- Problemi di rilascio.
- Black box applications.

> DevOps uò essere applicato a un ciclo di vita del software tradizionale, risponde alla **collaborazione**. Riduce il **tempo di sviluppo** e la **qualità/efficienza** dei team. Si aiuta con una serie di **automazione** del processo di sviluppo software e **standardizzazione** di esso.

## Silos
> Attività specifiche che sono **separate** tra di loro.

![[Schermata del 2024-06-07 10-01-06.png]]
## Hardware provisioning
- Data center;
- HW moved;
- Hosting;

Il **cloud** ha aiutato a sfocare la linea tra "dev" e "ops"
- fornendo infrastrutture **[[#Virtualization]]** che possono essere gestite e revisionate periodicamente.
- niente più **server fisici.**
### Virtualization
> Permette agli sviluppatori di **simulare** scenari creando ambienti multipli con diverse configurazioni, con la capacità di tirarle su velocemente e romperle facilmente senza grosse ripercussioni.

- **Platform as a Service** - (PaaS), ambienti pronti all'uso con strumenti per build e deploy per l'applicazione senza dover anche gestire l'infrastruttura sotto.
## Gestione delle configurazioni
Le applicazioni possono dipendere da codice condiviso:
- JARs
- Assembly
- vari plugins
**DevOps** permette una configurazione consistenze e ripetibile su vari ambienti diversi.
## Sicurezza
> Considerata in ogni **fase** e da **tutte** le persone.

*Deve essere inclusa il prima possibile in un approccio DevOps. - aspetto chiave della qualità del software.*
## Raccolta dei requisiti
- Feedback degli utenti
- Riflettono cosa è possibile
- **Non technical** requisiti
- Requisiti riflettono le **skill** correnti
- Obiettivi **condivisi**
### Requisiti comuni del sistema
- **Automazione**, ridurre gli errori, conflitti e aumentare la velocità.
- **Scalabilità**, in particolare dei dati, essenziale in DevOps.
- **Documentazione**, il tempo salvato a non documentare può riversarsi in più tempo nel tracciare le informazioni più tardi.
## Come fare DevOps
1. Scegliere un componente
	- Inizio piccolo e componente semplice
2. Agile
	- Solitamente DevOps si accoppia con una metodologia Agile tipo Scrum
3. Version control
	- GitHub
	- GitLab
	- BitBucket
4. Integrare i controlli del sorgente
5. Pipeline CI/CD
	- Unit test
	- TTD per risolvere bug
	- Correzione di bug
6. Costruire un processo CI/CD per il deployment
	- Pipe
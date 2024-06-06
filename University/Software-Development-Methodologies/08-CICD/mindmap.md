# CI/CD

## Cos'è CI/CD?
- **Continuous Integration (CI)**
  - Automatizza l'integrazione dei cambiamenti di codice in un repository condiviso
  - Ogni integrazione viene verificata da test automatici
  - Obiettivo: Rilevare e correggere problemi precocemente
- **Continuous Deployment (CD)**
  - Automatizza il deployment dei cambiamenti di codice negli ambienti di produzione dopo l'integrazione e i test
  - Obiettivo: Rilasciare software aggiornato agli utenti appena pronto
- **Continuous Delivery**
  - Simile a CD ma con una fase di approvazione manuale prima del deployment in produzione

## Perché CI/CD?
- **Faster Time to Market**
  - Automazione del processo di delivery software
  - Rilasci frequenti e rapidi
- **Increased Reliability**
  - Test e deployment automatizzati riducono il rischio di errori
  - Validazione approfondita del codice prima della produzione
- **Better Collaboration**
  - Migliora la collaborazione tra sviluppatori, tester e team operativi
  - Cicli di feedback rapidi
- **Continuous Improvement**
  - Identificazione e risoluzione di problemi in fasi iniziali del ciclo di sviluppo
  - Iterazioni rapide e miglioramento della qualità del software
- **Cost Savings**
  - Riduzione del lavoro manuale
  - Minimizzazione dei tempi di inattività
  - Riduzione del rischio di errori costosi

## Ciclo di Vita CI/CD
- **Continuous Integration**
  - Automatizzazione della build e dei test
- **Continuous Delivery**
  - Deployment automatico in ambienti di staging
  - Approvazione manuale prima del deployment in produzione
- **Continuous Deployment**
  - Deployment automatico in produzione dopo il superamento dei test
  - Nessuna necessità di approvazione manuale

## Continuous Delivery vs Continuous Deployment
- **Continuous Delivery**
  - Build, test e deployment automatico in staging
  - Approvaizone manuale prima del deployment in produzione
  - Processo consistente e affidabile per il rilascio degli aggiornamenti software
- **Continuous Deployment**
  - Deployment automatico in produzione dopo il superamento dei test
  - Eliminazione della necessità di approvazione manuale
  - Richiede alta confidenza nei test automatici e nei processi di deployment per garantire la stabilità della produzione

## CI/CD vs DevOps
- **CI/CD**
  - Automazione del processo di delivery software, dall'integrazione al deployment
  - Migliora la collaborazione tra sviluppo, test e operazioni
  - Aumenta la qualità, la velocità e l'affidabilità del software tramite l'automazione dei test e del deployment
- **DevOps**
  - Cultura e insieme di pratiche che enfatizzano la collaborazione e la comunicazione tra team di sviluppo e operazioni
  - Include pratiche CI/CD ma si estende anche ad aspetti culturali come responsabilità condivisa, trasparenza e cicli di feedback
  - Obiettivo: Eliminare i silos organizzativi e ottimizzare l'intero ciclo di vita della delivery software per rilasci più rapidi e affidabili

## Strumenti CI/CD
- **Version Control Systems (VCS)**
  - Es. Git
- **Continuous Integration Tools**
  - Es. Jenkins, Travis CI
- **Continuous Deployment Tools**
  - Es. Spinnaker, Argo CD
- **Automated Testing Tools**
  - Es. Selenium, JUnit
- **Monitoring Tools**
  - Es. Prometheus, Grafana


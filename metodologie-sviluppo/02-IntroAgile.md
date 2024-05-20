# Approccio Agile
> #problema *: i metodi tradizionali sono **rigidi**, tanta **documentazione**, i risultati sono solo alla fine ma i requisiti sono necessari all'inizio del progetto e non sempre il risultato soddisfa il cliente*.

Lo scopo principale era ingegnerizzare un approccio che evitasse il **fallimento** dei progetti, dovuti a i #problema citati.
- Risultato => **Sviluppo software Agile**.
## Manifesto Agile
- *Individui e interazioni prima dei processi e strumenti;*
- *Lavorare sul software rispetto a documentazione troppo dettagliata;*
- *Collaborare col cliente in favore di negoziazioni di contratti;*
- *Risolvere le sfide in favore di seguire forzatamente il piano.*
### 12 principi
>Il manifesto espone 12 principi fondamentali per un approccio Agile appropriato.
>Si parla di come costruire il progetto attorno a individui **motivati** dal progetto e dall'ambiente di lavoro, un ambiente dove le *informazioni* sono scambiate **faccia a faccia** per evitare malintesi e che un **ambiente** **sostenile** per il progetto e necessario per far vivere e coinvolgere le varie parti in modo *pacifico*.
>*Continua attenzione* alle **eccellenze tecniche** e di design ma mantenendo la **semplicità** per non aumentare il carico di lavoro. Perciò, per questi due punti precedenti è necessario che i team si **auto organizzano** il lavoro ed a intervalli regolari si **riflette** su come si può migliorare l'efficienza.
# Tecniche Agile
- [[#Test Driven Development]]
- [[#Pair Programming]]
- [[#Refactoring]]
- [[#Cross functional team]]
- [[#Timeboxing]]
- [[#User stories]]
## Test Driven Development
> #testing => Test scritti prima del codice.
![[Schermata del 2024-05-19 17-43-43.png]]
Stimola il programmatore a pensare alle *condizioni di fallimento* del codice. Aiuta anche a scrivere il minor codice possibile per passare i test. Un grosso downside è che viene scritto **molto più codice** (circa  400%) dovuto a tutti i test, però aiuta a **trovare i buchi** velocemente.
## Pair Programming
> Codice scritto in coppia.
- driver, scrive il codice;
- navigatore, controlla e fornisce consigli.
\**ogni 30 min swap dei ruoli*.
Questo aumenta la qualità del codice e i costi di esso, studi parlano di un -15% di velocità a vantaggio di -50% di bug nel codice. Non è sempre applicabile con personalità contrastanti tra  di loro o tra senior e junior.
## Refactoring
> Modifica della **struttura interna** senza modificarne il **comportamento**.

Può migliorare le #4chiavi:
- Leggibilità;
- Riusabilità;
- Estensibilità;
- Prestazioni.
*Buono programmatori scrivono programmi che gli umani possono capire.*
Attenzione alla due forme di **Refactoring** e **Refactor**:
- #Refactoring, cambiamento interno del codice per migliorare le #4chiavi;
- #Refactor, ristrutturazione del codice applicando dei #Refactoring senza cambiare il suo comportamento osservabile.
Regola del 3:
1. La prima volta lo fai e basta;
2. La seconda volta che fai qualcosa di simile, provi a non duplicare ma lo duplichi
3. La terza volta che fai qualcosa di simile, #Refactor.
Fare il #Refactor è utile quando si **aggiunge una funzione**, **aggiusta un bug** o in **code review**. Non è utile fare #Refactor quando il codice è un disastro o si è vicini a una scadenza.
### TDD + Refactoring
![[Schermata del 2024-05-19 18-25-19.png]]

## Cross functional team
> Ogni team deve essere composto da persone che insieme possono fare i task.

*Ogni persona sa fare più di una mansione.*
## Timeboxing
> Forzare il completamento del lavoro in un certo **tempo prestabilito**.

I vantaggi sono processi di sviluppo più veloci e di consegna del software, però a discapito della gestione del progetto più difficile e progetti complessi non possono essere divisi in iterazioni più semplici.
## User stories
> Descrivono cosa l'utente può fare o ha bisogno che faccia il sistema.

Utili per *definire i requisiti* in maniera informale. Generalmente le storie derivano dalle **epiche** - descrizioni generali.
Le storie come vantaggio principale possono raccogliere velocemente i requisiti del sistema, però generalmente le storie risultano difficili da scalare e molto vage sul problema.
### Esempio
- Epica: "Come utente voglio fare il backup completo del mio disco"
- Storia:
	1. "Come utente privilegiato, voglio specificare le file e directory basandomi su criteri come spazio, data, etc.";
	2. "Come utente normale, voglio indicare quali directory non fare il backup così non esaurisco lo spazio con dati che non voglio salvare.".
# Conclusioni finali su Agile
I modelli Agili sono accettati dalla comunità; però non sono sempre la soluzione. Infatti funzionano peggio in contesti di progetti larghi e di teams distribuiti.
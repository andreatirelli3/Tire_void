2 tipi di crittografia
- Simmetrica, (chiave condivisa), ovvero la chiave di cifratura è uguale per entrambe le parti (Alice e Bob) - generalmente usata per cifrare le comunicazioni
- Asimmetrica, (chiave privata e chiave pubblica, o chiave segreta e chiave pubblica - sono due modi uguali per caratterizzare la cifratura asimmetrica) - generalmente usata per cifrare e condividere la chiave condivisa che verrà usata per cifrare le comunicazioni o in contesti tipo mail o certificati digitali

*La cifratura classica non è un tipo di cifratura, ma vedila come uno scenario di cifratura simmetrico dove si usavano altre fonti per determinare la chiave k, esempio cifrario di Cesare era noto che la chiave k era 3.*

**Sicurezza di uno schema**, aggiungerei la definizione si **sicurezza crittografica** (spero che il termine sia corretto, dopo provo a vedere anche nelle slide di ferretti), ovvero che l'algoritmo si definisce perfetto a livello crittografico, esempio (e anche l'unico) è OTP - One Time Pad, dove è impossibile rompere l'algoritmo perché per ogni messaggio viene usato un pad diverso e di lunghezza == messaggio. Dalla sicurezza crittografica deriva la **sicurezza computazionale**, ovvero anche se l'algoritmo non è perfetto, ma prima che venga rotto devo passare N anni che superano di molto la durata media di una persona umana anche usato più fonti computazionali per rompere l'algoritmo (brute force).

! Aggiungi nella parte simmetrica dei cifrari a blocchi, il padding, perché nei cifrari a blocchi è richiesto come requisito che il messaggio in chiaro sia un multiplo della lunghezza del blocco scelta: https://en.wikipedia.org/wiki/Padding_(cryptography)
In particolare **PKCS#5 and PKCS#7**

*Nei cifrari a flusso, la sezione **Robustezza dei cifrari a flusso** la eliminerei, perché i cifrari a flusso non sono un miglioramento di quelli a blocco per avvicinarsi a OTP, ma sono necessari in quei contesti dove i cifrari a blocchi non possono essere usati, esempio comunicazioni telefoniche.*

**Funzioni HASH**, non ho aperto le slide, ma anche se giusto come accenno, parlare delle tecniche di **salt** nel hashing (soprattutto delle password) secondo me è utile. Praticamente il salt e un valore che viene introdotto quando si vuole fare hash di un test (in molti casi usati proprio per hashing delle password) questo per introdurre un layer in più di sicurezza, visto che in caso di un databreach, un attaccante può effettuare una attaccato a **rainbow table** ovvero pre-calcolare gli hash per certi testi (password) note - aka wordlist. Successivamente all'attaccante basta fare una compare degli hash presenti nel databreach con quelli da lui pre calcolati e ha vinto il gioco di sicurezza.
Il salt può essere in chiaro (senti l'audio su telegram)

RSA e DH, migliorare la caratterizzazione dei contributi pubblici e privati.

---
# Crittografia
## Settings crittografici
- Simmetrico, Bob e Alice usano la stessa chiave (shared key) per cifrare i messaggi. *Gli algoritmi simmetrici sono implementati in HW nelle moderne CPU, questo ne giova per le performance.*
- Asimmetrico, Bob e Alice hanno rispettivamente una coppia di chiavi, pubblica o privata (anche chiavate secret key - sk, public key - pk). sk e pk vengono usate rispettivamente per cifrare e decifrare in base alle garanzie crittografiche del messaggio che si vogliono ottenere nel contesto di utilizzo.
	- Garantire **confidenzialità**, Bob cifra il messaggio con la pk di Alice (destinatario). Alice decifra con la sua sk. Questo garantisce che solo il destinatario del messaggio possa decifrare il contenuto di esso.
	- Garantire **autenticità** (di conseguenza anche *integrità*), Bob cifra il messaggio con la sua sk, Alice (destinatario) decifra con la pk di Bob. Questo garantisce ad Alice che il messaggio sia effettivamente di Bob, perché è l'unico che è in possesso della sua sk.

> **Cifratura classica**, precursore del settings simmetrico, perché viene usata 1 chiave (shared key) - esempio cifrario di Cesare, sk = 3.

## Sicurezza di uno schema
Si definisce come **Sicurezza crittografica** quando uno schema crittografico non può essere rotto; un esempio (e anche l'unico) è One-Time Pad - OTP, ovvero per ogni messaggio si una una chiave (pad) diverso di lunghezza == len(plaintext). Computazionalmente questo schema non è implementabile perché richiederebbe che ad ogni messaggio venga condiviso ad Alice e Bob il nuovo pad. Per questo si parla di **Sicurezza computazionale** di uno schema; ovvero l'unico attacco noto che affligge lo schema è il *brute force* e il tempo computazionale supera gli ordini di grandezza di una vita umana e impiego di ulteriori macchine per velocizzare il calcolo di esso.

> Se per uno schema viene scoperto un attacco che non è il brute force, al 90% lo schema potrebbe risultare vulnerabile e quindi non più sicuro computazionalmente.

## Cifrari a blocchi & a flusso
Requisito dei cifrari a blocco:
- plaintext multiplo del len(block) scelto.

Se questo vincolo non può essere rispettato, si usano tecniche di **[Padding](https://en.wikipedia.org/wiki/Padding_(cryptography)**:
- **PKCS#5**
- **PKCS#7**

Aggiungono appunto dei bit alla plaintext per raggiungere il primo multiplo della len(block).

> Non sempre è possibile usare i cifrari a blocco, esempio comunicazioni telefoniche o streaming di dati. In questi contesti la lunghezza del plaintext non è nota a priori; per superare questo limite in questi contesti si utilizzano i **cifrari a flusso**.

## Password Hashing
Un attacco noto in contesti di *password hashing* è il **rainbow table**. L'attaccante può pre-calcolare gli hash di password note/deboli, usando wordlist dedicate. Questo permette all'attaccante di calcolare in pre o in parallelo mentre effettua un data breach gli hash di possibili password che si possono trovare al suo interno. Una volta ottenuto il db delle password all'attaccante basta fare una *compare* degli hash calcolati con quelli presenti e vincere il gioco di sicurezza perché è in possesso dell'indicizzazione password_plain <=> password_hashed.

Una possibile mitigazione è l'utilizzo di **salt**, un valore randomico (proveniente da una fonte sicura random) che viene aggiunto in fase di hash di essa (sia lato client che lato server!). L'obiettivo è quello di mitigare un attacco a **rainbow table** e (almeno) rallentare il processo di indicizzazione password_plain <=> password_hashed per l'attaccante. Tale scopo serve per far guadagnare tempo per chi ha subito il databreach di informare i suoi utenti nel processo di cambio password e altre possibili attività di recovery.

## RSA e DH
RSA e DH generano contributi privati e pubblici, che tramite opportune operazioni modulari permettono alla fine di ottenere la cosiddetta **chiave condivisa** - shared key. Che verrà utilizzata successivamente in settings simmetrico.
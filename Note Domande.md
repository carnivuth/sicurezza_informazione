## TRNG 
### contro
- bassa frequenza di generazione 
- flusso non riproducibile 
### caratteristiche
- basati su fenomeni naturali 
### scenari di utilizzo 
- generatori di semi per PRNG
## PRNG crittograficamente sicuri 
flusso riproducibile, usati nei cifrari a flusso
## cifrari a flusso
### sincroni
generano un flusso di chiave della stessa lunghezza del testo e cifrano facendo la somma modulo 2 bit a bit tra il testo in chiaro e la chiave, un attaccante in grado di fare attacchi attivi sul canale può modificare la lunghezza del cifrato e rendere indecifrabile il cifrato aggiungendo o rimuovendo dei bit al flusso di testo
### autosincronizzanti
presentano uno shift-register di dimensione variabile cosi in caso di de-sincronizzazioni queste sono limitate a un numero di bit pari alla lunghezza dello shift register utilizzato

## funzione hash crittograficamente sicure
### caratteristiche

- da un dato output deve essere computazionalmente difficile risaslire all' input che lo ha generato 
- ## resistenza alle collisioni 
- resistenza debole 
	- dato un valore x deve essere difficile trovare un valore y per cui sia vero `h(x)==h(y)`
- resistenza forte
	- deve essere computazionalmente difficile trovare 2 valori x e y per cui  `h(x)==h(y)`
- ### efficienza 
	- data una x deve essere computazionalmente facile ricavare la sua h(x)
## CIFRARI A BLOCCHI
- ### ECB
	- electronic code block
	- vengono cifrati i singoli blocchi, no influenza tra un blocco e l'altro, no propagazione dell'errore, malleabilità e predicibilità dell'output
- ### CBC
	- vettore IV random iniziale non predicibile e utilizzato una sola volta, generato con PRNG crittograficamente sicuro
	- viene eseguito lo xor di un blocco con il precedente cifrato 
	- propagazione dell'errore da un blocco ai successivi 
	- no parallelizzazione dell'operazione di cifratura
- ### CFB
	- come la cifratura a flusso, registro a scorrimento di n bit inizzializzato con un vettore IV che scorre e gli s bit vengono messi in xor con il testo in chiaro 
	- ![[Pasted image 20230711133713.png]]
- ### OFB 
- ![[Pasted image 20230711133728.png]]
- ### CTR
	- viene utilizzato un contatore inizializzato a un certo valore, compatibile con la dimensione del blocco da cifrare e lo si incrementa a ogni passaggio, lo si cifra e lo si mette in xor con il testo in chiaro 
	- ![[Pasted image 20230711133943.png]]
- ## applicazioni tipiche
	- ![[Pasted image 20230711134012.png]]

## ATTACCO CON ESTENSIONE
- attacco perpetrato all'autenticazione di un messaggio costruita con una funzione hash a compressione iterativa
-  dato un messaggio m autenticato con `h(s||m)` l'attaccante è in grado di autenticare un messaggio `m||m'` dove `m'` è una parte di messaggio decisa dall'attaccante 
- strategia per prevenire questo attacco è concatenare il segreto s dopo il messaggio nella procedura di firma `h(m||s)`

## SISTEMA KDC
![[attachments/Pasted image 20230711153430.png]]



- struttura robusta dal punto di vista della riservatezza a patto che
	- il db sia inviolabile 
	- i numeri random e la chiave di sessione siano randomici imprevedibili e indeducibili
- è sempre possibile un attacco DoS in quanto un attaccante in grado di effettuare attacchi attivi sul canale può sempre modificare i messaggi in modo da non consetire le operazioni di decifratura
- l'attacco con replica non è possibile se l'attaccante non conosce la chiave di sessione k
- soluzione anche se molto costosa: far tenere traccia a b di tutte le chiavi di sessione fino a quel momento 
## scambio di chiavi simmetriche
### modalita con key agreement
- ### KDC
	- entita centrale che detiene i segreti master dei partecipanti alle comunicazioni
- #### MASTER KEY
	- le entita si sono gia scambiate un segreto condiviso su un canale sicuro 
### modalita senza key agreement
- diffie-hellman
- cifrario asimmetrico
## gestione dei certificati 
- ### certificate revocation list
	- contengono le informazioni relative alla validita dei certificati detenuti dalla CA
	- crescono in lunghezza e diventano un collo di bottiglia da utilizzare, richiedendo sempre piu banda per la trasmissione in rete e potenza computazionale da parte del client per l'ananlisi
	- partizionare la CRL in modo da fornire solo una parte della CRL quando richiesta dal client, la crl viene partizionata nei vari nodi di un sistema a directory, quando un certificato viene revocato la crl conosce la entry dove è contenuto il certificato perche ne viene salvato l'indirizzo nel certificato stesso
	- eliminare le voci che fanno riferimento a certificati scaduti
	- gestire le CRL a **update incrementali detti delta**, dove si aggiornano i client solo con le differenze della CRL dal precedente aggiornamento in modo da ottimizzare il consumo di banda
- ## ottenimento di un certificato approvato da CA
	- autenticazione dell'utente interessato a richiedere il certificato 
	- generazione del certificato da parte dell'utente nella seguente forma:
	- `X || PX || SSX (H(X || PX))`
	- la RA verifica la firma dell'utente, se la verifica a successo viene comunicato il tutto alla CA che firma con la sua chiave privata e una copia viene fornita all'utente e un'altra viene mantenuta nel database dei certificati della CA

- ## meccanismi di revoca 
- possibili classificazioni:
- classificazione per metodo di aggiornamento:
	- pull, al momento del bisogno il client interroga il server per sapere se è presente un aggiornamento delle CRL
	- push, la CA aggiorna gli utenti al momento di una modifica alle CRL
- classificazione per stato 
	- **offline**: la verifica di un certificato può essere fatta anche senza la comunicazione con una CA
	- **online**: la verifica deve essere fatta comunicando con la CA
### meccanismo CRL
- modello **pull offline** la CA emette le liste di revoca dei certficati sulle repository e al momento del bisogno gli utenti possono scaricare le liste e consultarle
- possibile perdita di aggiornamenti in caso di consultazione di lista locale non aggiornata
- la dimensione delle CRL cresce senza strategie di gestione particolari
### meccanismo OCSP
- modello **pull online**, protocollo client-server che prevede la richiesta alla CA da parte degli utenti nel momento in cui è necessario verificare un certificato
- necessario essere online
## protocollo diffie-hellmanù
- ### anonimo
	- non vengono autenticati gli enti che prendono parte alla conversazione
	- scambio di un numero grande p sul canale insicuro
	- definizione di g generatore di p
		- `modp, g^2modp, …, g^pmodp restituisce tutti numeri distinti e compresi nell’intervallo [1, p-1]`
	- g e p possono essere scambiati nel canale insicuro
	- i due utenti generano due numeri random `Xa e Xb` contenuti in `1, p-1`
	- entrambi eseguono l'esponenziale modulare ` A calcolerà g^Xa modp = Ya e B calcolerà g^Xb modp = Yb`
	- vengono scambiati i valori Ya e Yb
	- A esegue `Yb^(Xamodp)` e B esegue `Ya^(Xbmodp)`
	- questo ultimo valore calcolato sara la chiave K di sessione che risulta uguale per entrambi 
	- nonostante i parametri scambiati sul canale insicuro questo non compromette la sicurezza del protocollo in quanto l'attaccante dovrebbe risalire a Xa o Xb che non puo ottenere da Ya o Yb a meno di non riuscire a compiere il calcolo del logaritmo discreto che è computazionalmente difficile
	- questo protocollo non fornisce garanzie sull' identita di chi genera Y
- ### variante fixed
	- il valore Y viene incapsulato in un certificato insieme a p e g
	- presente una certification autority
	- il valore Y non può esere cambiato
	- se qualcuno dice di essere chi non è le parti non accordano lo stesso segreto
- ### variante ephemeral
	- i due client generano a ogni sessione degli X diversi partendo dagli stessi p e g e di conseguenza anche degli Y che saranno firmati e certificati  anche in questo caso non si ha autenticazione delle parti
- ### autenticazione 
	- introdurre protocollo di sfida risposta in cui la sfida è la master secret concordata 
## cifrario RSA
- cifrario asimmetrico basato sul problema della fattorizzazione e di estrazione della radice iesima da un numero
- necessita la frammentazione del messaggio in input perche si basa su operazioni di modulo, inoltre non devono esserci collisioni tra messaggi
- l'algoritmo RSA è deterministico quindi necessario rendere l'input aleatorio
- nel caso un messaggio sia molto maggiore della lunghezza della chiave è necessario frammenteare il messaggio in pezzi di dimensione inferiore alla dimensione della chiave per evitare collisioni
- nel caso contrario necessario aggiungere padding
- ### proprieta moltiplicativa 
- si ha che per RSA `E(m1*m2)=E(m1*)*E(m2)` quindi è possibile farsi autenticare un messaggio facendosi autenticare il prodotto di quel messaggio con un mascheratore R e questo poi potrà essere usato come autenticatore per M
## PGP 
- modello completamente decentralizzato 
- sono presenti due portachiavi uno per le propie chiavi private e un altro per le chiavi pubbliche delle quali si ha fiducia
- campo owner trust: fiducia che si pone in quella determinata chiave
- signature memorizza l'eventuale certificato
- owner trust della chiave pubblica che ha firmato il certificato
- Key Legitimacy viene calcolato in base hai valori precedenti e contiene il livello di autenticita di una certa chiave
## compressione post firma 
- casistica applicata alla firma di mail tramite pgp questo per evitare di dover ricomprimere il messaggio per ricontrollare la firma che ha distanza di tempo potrebbe vedersi alterato l'algroritmo di compressione utilizzato o costringerebbe a inserire nel messaggio anche le informazioni dell'algoritmo di compressione utilizzato
## identificazione passiva
- viene utilizzato un segreto condiviso dalle due parti che non varia nel tempo (password)
## identificazione attiva
- viene utilizzato un segreto condiviso dalle due  parti che varia in ogni sessione  (password)
![[attachments/Pasted image 20230712163855.png]]
attacco vulnerabile a reflection e interleaving per via della simmetria dei messaggi scambiati (attaccante apre due sessioni una con  A e una con B e risponde a A con i messaggi di B e viceversa)
(attaccante apre due sessioni con A e nella seconda sessione invia ad A la stessa sfida che A ha lanciato nella prima sessione )

## IPSEC 
- ## SECURITY ASSOCIATION
	- connessione unidirezionale tra due interlocutori
- ### SECURITY POLICY DATABASE 
	- politiche di controllo del traffico in entrata e uscita 
- ### SECURITY ASSOCIATION DATABASE
	- database delle SA strutturato a entry
## BLOCKCHAIN 
- struttura dati distribuita decentralizzata a registro only append gestita ad albero
- ogni nodo che partecipa alla blockchain possiede una copia dell registro e ogni nodo puo fare modifiche
- le modifiche sono considerate valide se accettate da tutti i membri della blockchain gestiti tramite opportuni protocolli di consenso 
- l'immutabilità dei dati viene garantita da primitive crittografiche forti 
- dati aggiunti sotto forma di blocchi che possiedono un hash che identifica il blocco stesso e un hash che identifica il blocco precedente nella catena 
- 
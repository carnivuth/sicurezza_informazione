---
id: IPSEC
aliases: []
tags: []
index: 11
---

# IPSEC

È un servizio di sicurezza a livello di rete che nasce per risolvere alcuni problemi che altrimenti non possono essere risolti:

- **Packet sniffing**: lettura dei pacchetti in transito.
- **IP spoofing**: falsificazione dell’indirizzo del mittente, ciò permette di effettuare attacchi sofisticati.
- **Connection hijacking**: inserimento di dati nei pacchetti in transito.

## PROTOCOLLI

I protocolli utilizzati sono i seguenti

- **Authentication Header**: per autenticare pacchetti, garantisce autenticità e integrità.

- **Encapsulating Security Payload**: può essere configurato per garantire solo confidenzialità o anche autenticazione dei pacchetti.

- **Internet Key Exchange**: negoziare parametri di sicurezza, autenticazione e distribuzione delle chiavi utilizzate.

## ELEMENTI ARCHITETTURALI

IPSEC non è solo un insieme di protocolli,  ma prevede una serie di elementi architetturali

### SECURITY ASSOCIATIONS (SA)

Struttura dati che stabilisce quali sono i parametri di sicurezza che i due interlocutori devono utilizzare per la comunicazione seguente, composto da diversi elementi:

- **Security parameter index (SPI)** : valore che indica quali sono i pacchetti che sono sottoposti alla **SA**
- **Ip destination Address**
- **Security Protocol Identifier**: Tipo di protocollo applicato

### SECURITY POLICY DATABASE (SPD)

Database in cui vengono mantenute le security policy, queste vengono applicate alla comunicazione in base a differenti parametri della connessione stessa (*destination address, source address*)

## INCAPSULAMENTO DEI MESSAGGI

I messaggi possono essere scambiati secondo 2 modalità:

- **trasporto** a essere cifrato e solo il payload del pacchetto IP, gli indirizzi utilizzati devono essere instradabili nella rete
- **tunnel** utilizzato per costruire vpn, tutto il pacchetto IP viene cifrato e inserito all'interno di un altro pacchetto IP

## SERVIZIO ANTI-REPLY

Servizio per impedire attacchi di replay, si basa sul concetto di finestra a scorrimento:

Una volta stabilita la dimensione della finestra: i nodi comunicanti alla ricezione di un pacchetto lo marcano e controllano la finestra:

- se il pacchetto e gia presente nella finestra viene scartato
- se il pacchetto non e presente nella finestra viene marcato e inserito nella finestra
-  se il pacchetto ha un numero di sequenza inferiore all' ultimo pacchetto in finestra viene scartato

-------------------------------------------------

### **Trasporto vs Tunnel**

![](file:///tmp/lu1538623ccdy.tmp/lu1538623cckz_tmp_480c7f6.png)

Se uso la modalità di trasporto vuol dire che se ci sono due nodi terminali comunicanti non ci sono problemi, perché prendo il datagramma originale, mantengo inalterata l‘intestazione, aggiungo intestazione ipsec e incapsulo il payload e il pacchetto originario dentro, andando a garantire la confidenzialità e/o autenticità.

Mentre nella modalità tunnel si prevede di avere il parchetto originario IP che viene cifrato e incapsulato in un nuovo pacchetto che ha un’intestazione diversa da quello originale, un’intestazione ipsec e tuto questo incapsulato. La si utilizza quando sono interessata ad esempio a nascondere a chi è indirizzato il pacchetto.




A seconda della modalità di incapsulamento adottata possiamo avere dei servizi diversi:

S![](file:///tmp/lu1538623ccdy.tmp/lu1538623cckz_tmp_194559f8.png) cegliamo che per una SA adottiamo AH, se scelgo come modalità quella di trasporto assicuro l’autenticità del payload originario e assicuro l’autenticità e integrità di porzioni selezionate dell’intestazione ip originaria (es. ip del mittente).

Se scelgo la modalità tunnel autentico l’intero pacchetto interno e porzioni selezionate del pacchetto esterno. Ovvero autentico tutta l’intestazione e payload del pacchetto originario ma anche porzioni dell’intestazione creata.

E così via…

Per evitare IPspoofing devo quindi utilizzare AH, se non mi interessa ma voglio solo avere autenticazione del payload e confidenzialità uso ESP con autenticazione in modalità di trasporto.

Se voglio autenticazione di tutto il pacchetto orginario e la cifratura di tutto il pacchetto ip uso ESP con autenticazione in modalità tunnel.

IPsec, come SSL, per garantire l’autenticazione dell’origine dei dati usa HMAC, stesso meccanismo e motivazione, grande efficienza. Il segreto usato in AH è il segreto che deriva dalla negoziazione del segreto master previsto dal protocollo. ESP cifra e con protocollo di negoziazione potrà decidere i termini. Se ho ESP con autenticazione si ha che IPsec, a differenza di SSL, applica prima la cifratura e poi l’autenticazione. Applico in maniera diversa le trasformazioni, prima cifra e poi autentica.




Campi comuni intestazione

Nelle intestazioni sia di AH che di ESP è contenuto l’elemento Security Parameter Index (SPI), elemento fondamentale, previsto nell’intestazione di tutti i protocolli, altrimenti non è possibile in fase di ricezione sapere a quale SA appartine quel pacchetto IPsec, e non si saprebbe neanche come trattarlo il pacchetto. Questo campo di 32 bit, insieme all’indirizzo IP di destinazione ed all’indentificatore di protocollo, identifica in maniera univoca la SA relativa al datagramma.

Altro campo comuni: Sequence number field che espleta il servizio di anti-replay tramite la numerazione progressiva dei datagrammi.

Infine c’è l’Autentication data, contiene l’CV che consente di verificare l’integrità del pacchetto in fase di ricezione.

## **Costruzione AH**

Slide 25

## **Costruzione ESP**

Slide 25



[PREVIOUS](DIFFIE_HELLMAN.md) [NEXT](SSL.md)

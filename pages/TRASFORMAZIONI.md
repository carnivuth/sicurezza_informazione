# TRASFORMAZIONI PER LA SICUREZZA

Per consentire comunicazioni sicure, due soggetti utilizzano metodologie di manipolazione dei dati dette trasformazioni per comunicarli su un canale insicuro, queste possono essere di due tipi:

- **Algoritmi**: la sorgente utilizza un algoritmo per trasformare il dato in qualcosa di diverso. Spesso però un algoritmo non è sufficiente, ma occorre che sorgente e destinazione, per proteggere il dato, debbano ricorrere a un protocollo


- **Protocollo**: insieme, sequenza di trasformazioni, che secondo un ordine ben preciso deciso dal protocollo stesso, non arbitrario, deve essere eseguito. Questo perché i partecipanti stessi possono essere malintenzionati, esempio la sorgente vuole ripudiare il messaggio inviato.

```mermaid
flowchart LR
A[Bob]
B[Alice]
A --informazioni trasformate--> B
```

Le trasformazioni possono all'occorrenza ricorrere a una terza parte affinche il dato sia protetto, il compito della terza parte diventa quello del garante

```mermaid
flowchart LR
A[Bob]
B[Terza parte]
C[Alice]
A --informazioni trasformate--> B
B --richiesta convalida--> C
A --registrazione comunicazione--> C
```

## TRASFORMAZIONE $E$

Questa trasformazione prevede che la sorgente alteri le comunicazioni in maniera da renderle irriconoscibili se non dalla sorgente. Il suo compito e quello di mantenere la **riservatezza della comunicazione**


```mermaid
flowchart LR
A[Source]
B{{E}}
C[Destination]
A --plaintext--> B --cyphertext--> C
```

La proprietà fondamentale e che lo sforzo per ripristinare il `plaintext`  dal `cyphertext` deve essere insostenibile per un attaccante

## TRASFORMAZIONE $H$

La trasformazione $H$ ha il compito di preservare **l'integrita di una comunicazione**, la sorgente allega al messaggio un riassunto che consente alla destinazione di comprendere se il messaggio e stato alterato

```mermaid
flowchart LR
A[stringa di lunghezza\n variabile]
B{{H}}
C[stringa di lunghezza\n fissa]
A --> B --> C
```

La trasformazione $H$ deve ridurre al minimo la possibilità di collisioni ovvero:

*se $x,y$ sono stringhe diverse deve essere vero che $H(x) \neq H(y)$*

questo non e sempre garantito in quanto il dominio di partenza e più grande del dominio di destinazione,Inoltre una funzione $H$ sicura e tale se **risulta molto difficile risalire a due testi che generino una collisione**

### CONTROLLO DI INTEGRITA

```mermaid
flowchart LR
subgraph source
A((S))
B{{H}}
end
subgraph destination
C{=?}
D{{H}}
E((D))
end
A --> B
B --h su canale sicuro--> C
D --> C
C --risultato--> E
A --M--> E & D
```
## TRASFORMAZIONE $S$

Trasformazione $S$ ha il compito di assicurare **l'autenticità di un messaggio**, la sorgente allega al messaggio informazioni non imitabili e la destinazione verifica che il documento ricevuto sia di chi ha dichiarato di averlo mandato

```mermaid
flowchart LR
A(A)
B{{Sign}}
C{{Verify}}
D(B)
A --> B --> C --> D
```

## COMBINAZIONI DI TRASFORMAZIONI RISERVATEZZA + INTEGRITÀ

Per far si che una comunicazione presenti multiple delle caratteristiche di sicurezza e necessario combinare le trasformazioni, per ottenere riservatezza e integrità si può ricorrere alla seguente combinazione

$$C = E(m|H(m))$$

in questo scenario l'attaccante non e in grado di minare ne la riservatezza ne l' integrità della comunicazione dato che dal testo cifrato non e in grado di risalire ne al contenuto del messaggio ne al valore della funzione $H$ ammesso che sia a conoscenza della struttura del messaggio

Un'altra possibile combinazione puo essere la seguente

$$C = E(m)|H(m)$$

In questo caso la proprietà di riservatezza può essere violata perché l'attaccante può fare ipotesi su possibili $m^{*}$ tale che $H(m^{*})=H(m)$.

Questo attacco, essendo un attacco di forza bruta ha però un costo computazionale: fino a 128 bit si considera un attacco possibile, oltre si considera un attacco difficile.

## COSA SERVE PER RENDERE LE TRASFORMAZIONI SICURE?

Tutte le funzioni di trasformazione devono essere **one way functions** ovvero:

- essere invertibili
- facili da calcolare
- $\forall \space a \in X$ deve essere difficile risolvere il problema $y=f(x)$ (*e.g. deve essere difficile risalire all input che ha generato un certo valore*)

Nella pratica, tali funzioni possono essere solo approssimate sfruttando problemi difficili della teoria dei numeri o manipolando i dati a livello di bit, in questo caso si parla di funzioni **pseudo-direzionali** (funzioni one-way per chiunque non sia in possesso di un dato segreto)

## FUNZIONI SEGRETE

Per rendere segreta una funzione si possono sfruttare diversi approcci:

- segretare la macchina
- segretare l'algoritmo
- segretare un parametro

Segretare un parametro e la scelta vincente in quanto gli algoritmi di cifratura sono complessi e necessitano di essere condivisi dalle due parti e segretare la macchina e estremamente costoso si fa dunque uso di [chiavi](CHIAVI.md)

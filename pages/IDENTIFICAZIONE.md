# IDENTIFICAZIONE

Esistono applicazioni che richiedono l'anonimato, altre che necessitano dell'anonimato. Nella maggioranza dei casi è però **richiesto che l'erogatore di un servizio conosca l'identità di chi ne usufruisce e che il fruitore sia certo dell'identità dell'erogatore** 

Il protocollo di identificazione e una **soluzione real-time** e l'informazione che ne scaturisce ha **validità istantanea**

## MODALITÀ DI IDENTIFICAZIONE

Le possibili dimostrazioni di identita che un utente puo fornire sono 3:

-   **conoscenza di un segreto** (*pin, psw, key*)
-   **possesso di un segreto** (*token, banda magnetica, smart card*)
-   **conformità** (*riconoscimento fisiologico o comportamentale*)

## PROTOCOLLO DI IDENTIFICAZIONE

Per un'identificazione sicura vale la seguente regola:

*tramite il solo scambio di messaggi, l'Entità che vuole farsi riconoscere deve fornire informazioni non imitabili, atte ad individuarla univocamente in quel preciso istante di tempo, e l'Entità che effettua il riconoscimento deve potersi convincere della loro genuinità*

In un protocollo di identificazione le entità coinvolte sono sempre due:
- identificando
- verificatore

Due sono i momenti topici, o fasi, di un processo d'identificazione:

- **registrazione**: in questa fase le due parti concordano un segreto S
- **riconoscimento**: in questa fase l'identificando richiede all verificatore di essere identificato

### FASE DI RICONOSCIMENTO

```mermaid
flowchart TD
subgraph identificatore
D{{T12}}
E{{T21}}
F{{T32}}
end
subgraph identificando
A{{T11}}
B{{T22}}
C{{T31}}
end
A --dichiarazione --> D
E --interrogazione--> B
C --dimostrazione--> F
D --> E
B --> C
identificando ~~~ identificatore
```

Il verificatore sa se il verificando è chi dice di essere solo al termine della terza fase.

### CONSIDERAZIONI

La robustezza del protocollo si basa sul fatto che la dimostrazione e una procedura computazionalmente difficile che non deve permettere a un attaccante di risalire al segreto utilizzato
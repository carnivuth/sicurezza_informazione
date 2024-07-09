# ATTACCO DI REFLECTION

Attacco attuabile nei protocolli di sfida risposta di tipo simmetrico della forma seguente

```mermaid
sequenceDiagram
participant alice 
participant bob
bob ->> alice: RB
alice ->> bob: RA || H(RB|S)
bob ->> alice: H(RA || S)
```

In questa tipologia di attacco l'utente malevolo apre due sessioni con alice e sfrutta la seconda sfida come risposta alla prima riproponendo il numero random fornito da alice alla prima sessione


```mermaid
sequenceDiagram
participant alice 
participant eve
eve ->> alice: RB
alice ->> eve: RA || H(RB|S)
rect rgb(255, 0, 0)
note over alice,eve: eve riusa il nounce della prima sessione
eve ->> alice: RA
alice ->> eve: RC || H(RA|S)
end
eve ->> alice: H(RA|S)
```
# Appunti interrogazione
## Semantica operazionale di IMP?

* logica di descrizione comandi
* Big step e small step (configurazioni e stato)

## Big step -> t è unica ed esiste?

* se esiste è unica
* se non termina non esiste

## Esempio di non terminante? E come lo prova?

* WHILE True DO c
* Per contraddizione come lo ha fatto lui
* altro modo è relazione tra small step e big step (teorema equivalenza)
    * ((c, s)) => t  <-> ((c, s)) ->* ((SKIP, t))
* la variabile booleana è costante a true e non cambia lo stato
    * => non diminuisce in complessità
* In realtà in quel modo diamo per scontato che small step sia deterministico

## A cosa riduce ((WHILE True DO c,s )) in small step?

* ((IF true DO (c :: (WHILE True DO c)) ELSE skip, s))
* riduco con iftrue -> ((c :: WHILE True DO c, s))
* riduco con comp2 (?) -> ((WHILE True DO c, s)) con s che non cambia
* Da questo mi ricollego alla parte prima

## Logica di Hoare

* precondizione, comando, postcondizione -> tripla (con 2 asserzioni)
    * asserzione = predicato sullo stato (State -> Set  se  semplificassi "tipo" State ->  Bool), con P s "qualcosa di vero o di falso"
    * Set è universo dei tipi, o in logica universo proposizioni
* validità P s ->  ((c, s))=>((c', t))  -> Q t

## Definisco \[\[ C \]\] s ={ t se  (c, s)=> t    \bot altrimenti.

1. t |= P  se P t
2. \bot |= sempre
3. State_\bot ={\bot}\cup State


## Definisco quindi validità Logica di Hoare?

* |={P} C {Q} se s |= P  ->  [[C]]s |= Q

## Sistema di Hoare è corretto? Come dimostrare?

* |- {P} C {Q} -> |= {P} C {Q}
* |- {P} C {Q} -> P s -> ((c, s))=>t -> Q t (di cui i primi 3 sono ipotesi)
* Definisco come induzione strutturale sulla derivazione della tripla (primo argomento)

## Correttezza caso while? Albero

* H-While -> {P \land lift b} c {P} (senza s e con lift b = \lambda s . bval b s = true)
* P è l'invariante
``` plain
               {P \land b} c {P}
    --------------------------------------- while
       {P} WHILE b DO c {P \land \neg b}      |= {P \land \neg b} -> {Q}
    -----------------------------------------------------------------------[1] weak
           {P} WHILE b DO c {Q}
```
* [1] \all s,t  -> |- [P]c[Q] -> (Q' t -> Q t) -> [P]c[Q] (indebolimento)
    * Quindi |= {P \land \neg b} -> {Q} (implicazione)
* Questa è definizione invariante che rimane dentro con guardia vera e quando esce perchè la guardia è falsa allora l'invariante è vero ancora (è vero a inzio, in mezzo e alla fine)

## Domanda su esonero (differenza tra i due comp)? E quale vantaggio ha?

* concatenazione di comandi come liste vs comando già creato a destra e resto programma

## Domanda su esonero C1 C1 Add?

* lo metto in quel ordine perchè devo prima valutare le due espressioni e una volta che sono sullo stack posso fare la somma.

## E' valida {0=1} x=3 {x=7}? E perchè? Quale è il minima precambiamento?

* è valida a vuoto (non esiste stato in cui 0=1)
* {1=1} x= 3 {x=3} o meglio {3=3} x=3 {x=3} perche sarebbe {wlp (x=3) {x=3}} x=3 {x=3}

## WLP tipo? Validità di tripla ?

* Com -> Assn -> Assn   mentre wlp C Q : Assn
* Se vale Ps e termina allora vale Qt

## WLP perchè liberal?

* liberal perchè non tiene conto della terminazione se non termina ma le precondizioni sono soddifatte è comunque soddisfatta la tripla
* altrimenti sarei in grado di decidere problema di halt
    * per farlo dovrei scrivere metodo per determinare terminazione

## VCC vs IMP?

* comando commentato con invarianti di ciclo

# H-While
``` plain
     {P \and lift b} c {P}
  -----------------------------------
   {P} WHILE b DO c {P and lift b}
```
lift = \all -> bval b s == true

# NOTE

1. non sono perfettamente accurati questi appunti
2. mentre si parla si deve scrivere
3. è sciallo e parla di altro per rilassarlo e scherza
4. fa parlare e da suggerimenti con il tono della voce
5. usare linguaggio non di agda ma di matematica
6. okay con i voti
7. chiede all'inizio domande su ultimo esonero (visto che non l'ha corretto)

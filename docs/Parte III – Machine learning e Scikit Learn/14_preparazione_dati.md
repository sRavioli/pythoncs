# 14 – Preparazione dei dati

> Corso di Python per il Calcolo Scientifico
>
> Appunti redatti da Simone Fidanza, s.fidanza1@studenti.uniba.it

Angelo Cardellicchio, angelo.cardellicchio@stiima.cnr.it

<details>
    <summary>Outline</summary>

<a name="top"></a>

<!-- TOC -->

1. [14 – Preparazione dei dati](#14--preparazione-dei-dati)
   1. [Il campionamento (⮨)](#il-campionamento-)
      1. [Dimensione del dataset (⮨)](#dimensione-del-dataset-)
      2. [Qualità del dataset (⮨)](#qualità-del-dataset-)
   2. [Preparazione dei dati (⮨)](#preparazione-dei-dati-)
      1. [Pulizia dei dati (⮨)](#pulizia-dei-dati-)
      2. [Sbilanciamento del dataset (⮨)](#sbilanciamento-del-dataset-)
         1. [Influenza dello sbilanciamento (⮨)](#influenza-dello-sbilanciamento-)
         2. [Sottocampionamento e upweighting (⮨)](#sottocampionamento-e-upweighting-)
      3. [Trasformazione dei dati (⮨)](#trasformazione-dei-dati-)
         1. [Trasformazione dei dati numerici (⮨)](#trasformazione-dei-dati-numerici-)
            1. [Scaling (⮨)](#scaling-)
            2. [Clipping (⮨)](#clipping-)
            3. [Trasformazione logaritmica (⮨)](#trasformazione-logaritmica-)
            4. [Z-score (⮨)](#z-score-)
         2. [Trasformazione dei dati categorici (⮨)](#trasformazione-dei-dati-categorici-)
      4. [Suddivisione dei dati (⮨)](#suddivisione-dei-dati-)

<!-- /TOC -->

</details>

Nella lezione precedente è stato evidenziato che uno dei passi fondamentali per
il machine learning sia quello di determinare i dati sui quali il modello deve
essere addestrato. Per ottenere delle buone predizioni, è necessario _costruire_
_un dataset_ ed eventualmente effettuare delle opportune _trasformazioni_ sui
dati. Queste operazioni sono, salvo eccezioni, riassumibili nei concetti di
_campionamento_ e _preparazione dei dati_.

## Il campionamento ([⮨](#top))

Il primo problema da affrontare è la raccolta dei dati, che servirà ovviamente
a generare il nostro dataset. In questa fase, dovremo partire affrontando due
aspetti: la _dimensione_ e la _qualità_ dei dati che abbiamo raccolto.

### Dimensione del dataset ([⮨](#top))

Non vi è una regola vera e propria per determinare il quantitativo di dati
_sufficiente_ per addestrare adeguatamente un modello. In generale, potremmo
dire che il modello deve essere addestrato su un quantitativo di dati che sia
maggiore di almeno un ordine di grandezza rispetto al numero dei parametri
dello stesso. A scopo puramente esemplificativo, usando una rete neurale con
$100$ neuroni dovremmo indicativamente avere almeno $1000$ campioni a
disposizione.

### Qualità del dataset ([⮨](#top))

Parafrasando un vecchio adagio, _la quantità è niente senza qualità_.
Nell'ambito della data science, ciò implica che avere a disposizione grandi
quantità di dati non basta se questi non sono anche _significativi_ nella
caratterizzazione del fenomeno sotto osservazione.

Per comprendere questo concetto, possiamo usare un approccio empirico.
Immaginiamo di voler creare un modello di predizione delle precipitazioni e di
dover scegliere per addestrarlo tra due dataset. Il dataset $A$ contiene i
campionamenti ogni $15\\,{\rm min}$ dei valori di temperatura e di pressione
degli ultimi $100\\,{\rm a}$, soltanto per il mese di luglio, mentre il dataset
$B$ contiene un unico valore giornaliero, ma preso per tutti i mesi dell'anno.
È facile calcolare che il numero di campioni del dataset $A$ è pari a:

$$4 \cdot 24 \cdot 31 \cdot 100 = 297,600$$

valori, mentre quello per il dataset $B$ è pari a:

$$365 \cdot 100 = 36,500\text{.}$$

Tuttavia, la qualità del dataset $B$ è migliore rispetto a quella del dataset
$A$. Infatti, nonostante quest'ultimo abbia quasi il decuplo dei dati
($\times 8.15$), sarà praticamente inutile per la stima delle precipitazioni in
inverno, primavera o autunno.

## Preparazione dei dati ([⮨](#top))

Completata la procedura di campionamento, è necessario effettuare la
preparazione dei dati. Il primo step è spesso trascurato, ma è di vitale
importanza nel caso in cui si stia lavorando con delle informazioni sensibili,
come informazioni legate alle condizioni sanitarie di diversi pazienti. In
questi casi è strettamente necessario provvedere all'_anonimizzazione_ dei
dati, rimuovendo tutte le informazioni definite come _Personally Identifiable_
_Information_ (PII).

Una volta completato questo passaggio, potremo passare alle azioni maggiormente
rilevanti dal punto di vista scientifico.

### Pulizia dei dati ([⮨](#top))

L'affidabilità del dataset è essenziale per garantire performance ottimali del
modello addestrato. Occorre determinare diversi fattori, tra cui:

- **errori nel labelling**: bisogna valutare a grandi linee se il lavoro svolto
  dall'essere umano nell'etichettatura è accettabile, o se questa procedura è
  stata soggetta a errori di natura grossolana;
- **rumorosità del dataset**: è importante valutare se i dati sono affetti da
  rumore. Ad esempio, le letture di un sensore potrebbero essere tutte quante
  affette da offset o bias o, nel caso peggiore, essere causate da lettori non
  più tarati e quindi inutilizzabili;
- **dati mancanti**: è possibile che i valori di alcune feature non siano
  disponibili per alcuni campioni;
- **valori contrastanti o duplicati**: ci potrebbero essere parti di dataset in
  cui una lettura di temperatura avviene in Kelvin, e altre in cui la lettura
  avviene in gradi Celsius, oppure ci potrebbero essere valori duplicati a
  causa di errori nell'I/O del sensore.

In tutti questi casi, va scelta una strategia di pulizia: in certe situazioni
potrebbe essere sufficiente eliminare un campione, oppure effettuare
un'operazione di _filling_ a partire dalla restante parte del dataset, o
ancora, in casi estremi, si potrebbe eliminare _completamente_ la feature
interessata da rumore.

### Sbilanciamento del dataset ([⮨](#top))

E' possibile che un dataset abbia diverse proporzioni nei raggruppamenti dei
dati. Anche se questo fenomeno può interessare ogni insieme di dati, è
maggiormente evidente nei problemi di classificazione, nei quali abbiamo un
feedback immediato sulle differenti proporzioni grazie proprio alla presenza
delle label per le classi.

In particolare, avremo due tipi di "suddivisioni":

1. le **classi maggioritarie**, ovvero quelle con il maggior numero di
   campioni,
2. le **classi minoritarie**, ovvero quelle con a disposizione un numero
   limitato di dati.

Un dataset in cui sussiste questa ineguaglianza è detto **sbilanciato**.

E' possibile quantificare approssimativamente lo sbilanciamento del dataset. In
tal senso, possiamo rifarci alla seguente tabella:

| Grado di sblianciamento | Percentuale di campioni di classi minoritarie |
| :---------------------- | :-------------------------------------------- |
| Leggero                 | dal $20\\,\\%$ al $40\\,\\%$ del dataset      |
| Moderato                | dall'$1\\,\\%$ al $20\\,\\%$ del dataset      |
| Estremo                 | meno dell'$1\\,\\%$ del dataset               |

#### Influenza dello sbilanciamento ([⮨](#top))

Per capire qual è il problema legato allo sbilanciamento del dataset,
immaginiamo di dover creare un modello che individui una mail di spam. Per
farlo, usiamo un dataset con la seguente proporzione:

|                    | Mail spam   | Mail non spam |
| :----------------: | :---------- | :------------ |
| Numero di immagini | $5$         | $995$         |
|    Percentuale     | $0.5\\,\\%$ | $99.5\\,\\%$  |

Il problema sta nel fatto che un numero così esiguo di mail di spam farà sì che
il modello spenda la maggior parte dell'addestramento su mail normali, non
imparando quindi a riconoscere i casi di spam. Per fare un parallelismo con il
nostro cervello, se vedessimo $995$ immagini di penne e solo $5$ di matite, è
probabile che non saremmo in grado di distinguere una matita da una penna
perché, semplicemente, _non sapremmo come è fatta una matita_.

#### Sottocampionamento e upweighting ([⮨](#top))

Un modo efficace per gestire situazioni in cui il dataset è sbilanciato è
quello di utilizzare tecniche di _data balancing_. Ne esistono di diverse, più
o meno efficaci; tuttavia, la più semplice è quella di rimuovere un certo
numero di campioni di classe maggioritaria (_sottocampionamento_ o
_downsampling_), dando agli esempi sottocampionati un peso maggiore
nell'addestramento (_upweighting_).

In pratica, se scegliessimo di mantenere soltanto il $10\\,\\%$ delle mail non-
spam, avremmo circa $99$ campioni. Ciò porterà il rapporto tra le mail di spam e
quelle non di spam a circa il $5\\,\\%$, passando da una situazione di
sbilanciamento estremo ad una di sbilanciamento moderato.

A valle di questa operazione, dovremmo dare maggior peso ai campioni delle mail
non-spam, usando un fattore tendenzialmente pari a quello che abbiamo usato in
fase di downsampling. Nella pratica, ogni mail non-spam avrà un peso dieci
volte superiore a quello che avrebbe avuto se si fosse utilizzato il dataset iniziale.

### Trasformazione dei dati ([⮨](#top))

Il passo successivo nella preparazione dei dati è quello di _trasformare_
alcuni valori. In tal senso, possiamo operare per due ragioni principali.

La prima è che siano necessarie delle trasformazioni obbligatorie volte a
garantire la compatibilità dei dati, come ad esempio:

- **convertire feature non numeriche in numeriche**: non è possibile
  effettuare operazioni sensate tra interi e stringhe, per cui bisogna
  individuare un modo per favorire il confronto;
- **ridimensionare gli input ad una dimensione fissa**: alcuni modelli, come ad
  esempio le reti neurali, prevedono un numero fisso di nodi di input, per cui
  i dati in ingresso devono avere sempre la stessa dimensione.

La seconda è legata invece a delle trasformazioni opzionali, che ottimizzano
l'addestramento del modello. Ad esempio, potremmo dover effettuare la
_normalizzazione_ dei dati numerici, ovvero portarli tutti all'interno di una
stessa scala di valori, normalmente compresa tra $[0, 1] \text{ o } [-1, 1]$.
Vediamo più nel dettaglio alcune possibilità.

#### Trasformazione dei dati numerici ([⮨](#top))

Abbiamo detto in precedenza che potremmo voler applicare delle
**normalizzazioni** a dei dati numerici per migliorare le performance del
modello.

Per comprenderne il motivo, immaginiamo di avere un dataset che comprende
feature per età (che possiamo presupporre assuma valori da $0$ a $100$) e
stipendio (che possiamo presupporre assuma valori da $10,000\\,€$ a
$100,000\\,€$). Quando andiamo ad utilizzare questi valori in algoritmi che
effettuano delle operazioni tra feature, l'età diventerà presto trascurabile
rispetto allo stipendio, che è di due o tre ordini di grandezza superiore, per
cui il modello si troverà a prediligere quest'ultimo in fase di analisi. Ciò
implica quindi la necessità di arrivare ad una "base comune" a partire dalla
quale operare.

Le principali tecniche di normalizzazione disponibili sono quattro.

##### Scaling ([⮨](#top))

Lo **scaling** prevede la conversione dei valori assunti da una feature in un
range che va di solito tra $[0, 1] \text{ o } [-1, 1]$. La formula dello
scaling è la seguente:

$$y = \frac{(x - x_{\text{min}})}{(x_{\text{max}} - x_{\text{min}})}$$

##### Clipping ([⮨](#top))

Può capitare che il dataset contenga degli _outlier_, ovvero dei campioni che
divergono notevolmente dalle caratteristiche statistiche del dataset. In questo
caso, potremmo limitarci a rimuovere completamente tali valori mediante soglie
statistiche, come i range interquartili in caso di distribuzione parametrica, o
i classici $3\sigma$ in caso di distribuzione normale.

##### Trasformazione logaritmica ([⮨](#top))

Un'altra possibilità è quella di convertire i nostri valori in scala
logaritmica, comprimendo un range ampio in uno più piccolo usando la funzione
logaritmo:

$$y = \log{x}$$

##### Z-score ([⮨](#top))

Un ultimo tipo di trasformazione prevede l'uso dello _z-score_, che prevede una
riformulazione dei valori assunti dalla feature per fare in modo che questi
aderiscano ad una distribuzione a media nulla e deviazione standard unitaria.
Per calcolarlo, si usa la seguente formula:

$$y = \frac{x - \mu}{\sigma}$$

$\text{dove } \mu$ è la media della distribuzione dei nostri dati,
mentre $\sigma$ è la varianza.

#### Trasformazione dei dati categorici ([⮨](#top))

Alcune delle nostre feature possono assumere esclusivamente valori _discreti_.
Ad esempio, le nostre immagini potrebbero raffigurare diverse razze di cani,
oppure il campo "località" potrebbe riportare il codice postale. Queste feature
sono conosciute come feature _categoriche_, ed i valori ad esse associate
possono essere sia stringhe sia numeri.

> <details>
> <summary>ℹ️ <em>Le feature categoriche di tipo numerico</em></summary>
>
> Spesso, dobbiamo trattare feature categoriche che contengono valori numerici.
> Per fare un esempio, consideriamo il codice postale, che è un numero. Se lo
> si rappresentasse come una feature di tipo numerico, il nostro modello
> potrebbe interpretare la distanza tra Bari ($70126$) e Taranto ($74121$) come
> pari a $3,995$, il che non avrebbe ovviamente alcun senso.
>
> </details>

Per essere trattate, le feature categoriche hanno rappresentazioni di tipo
numerico, _mantenendo il riferimento al significato categorico e discreto_. Per
comprendere le implicazioni di questo concetto, immaginiamo i giorni della
settimana. Il modo più semplice per passare da una rappresentazione puramente
categorica ad una numerica è quella di usare un numero:

| Giorno    | Rappresentazione |
| :-------- | :--------------- |
| Lunedì    | $1$              |
| Martedì   | $2$              |
| Mercoledì | $3$              |
| Giovedì   | $4$              |
| Venerdì   | $5$              |
| Sabato    | $6$              |
| Domenica  | $7$              |

In questa maniera creeremo un "dizionario", nel quale potremo accedere ad una
chiave (la rappresentazione) che rappresenterà un determinato valore (il
giorno).

> <details open>
> <summary>⚠️ <strong>Attenzione!</strong></summary>
>
> A valle di questa trasformazione, la differenza aritmetica tra Domenica e
> Sabato continua ad avere un senso alquanto limitato, e comunque relativo a
> un generico concetto di _distanza_.
>
> </details>

Un altro modo di rappresentare le feature categoriche è mediante una
_rappresentazione sparsa_, detta anche _one-hot encoding_, nella quale ogni
valore è rappresentato da un vettore $V$ di lunghezza $m$, con $m$ numero di
categorie possibili. In questo caso, tutti i valori di $V$ saranno pari a $0$,
tranne quello rappresentativo del valore attualmente assunto dalla feature, che
sarà pari ad $1$. Ad esempio, la rappresentazione sparsa del Lunedì è data da:

```python
lunedì = np.array([1 0 0 0 0 0 0])
```

mentre quella del Giovedì:

```python
giovedì = np.array([0 0 0 1 0 0 0])
```

### Suddivisione dei dati ([⮨](#top))

L'ultimo passo nella preparazione del dataset è quello della suddivisione dei
dati. In particolare, si destinano un certo quantitativo di dati per
l'addestramento del modello, delegando la restante parte alla validazione dei
risultati ottenuti; ciò è legato alla volontà di verificare la capacità di
_generalizzazione_ del modello, ovvero a quanto è in grado di "funzionare" il
nostro algoritmo in caso di analisi di dati su cui non è stato addestrato.

Un rapporto molto usato in tal senso è quello che prevede che il $70\\,\\%$ dei
dati sia usato per l'addestramento, mentre il restante $30\\,\\%$ per la
validazione dei risultati ottenuti.

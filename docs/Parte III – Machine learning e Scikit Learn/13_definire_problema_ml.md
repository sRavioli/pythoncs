# 13 – Definire un problema di machine learning

> Corso di Python per il Calcolo Scientifico
>
> Appunti redatti da Simone Fidanza, s.fidanza1@studenti.uniba.it

Angelo Cardellicchio, angelo.cardellicchio@stiima.cnr.it

<details>
    <summary>Outline</summary>

<a name="top"></a>

<!-- TOC -->

1. [13 – Definire un problema di machine learning](#13--definire-un-problema-di-machine-learning)
   1. [Determinare l'obiettivo (⮨)](#determinare-lobiettivo-)
   2. [Comprendere i dati (⮨)](#comprendere-i-dati-)
   3. [Scegliere il modello (⮨)](#scegliere-il-modello-)

<!-- /TOC -->

</details>

Il primo passo nella risoluzione di un problema di machine learning è
_definirlo_. Dovremo analizzare il problema isolando gli elementi essenziali da
utilizzare per la sua risoluzione e determineremo la fattibilità del problema,
fornendo un insieme chiaro di _obiettivi_ e _criteri_ per la sua risoluzione.

## Determinare l'obiettivo ([⮨](#top))

Partiamo nella definizione del problema determinando il nostro obiettivo,
ovvero ciò che si vuole ottenere a valle della risoluzione del problema.

Ad esempio, potremmo voler calcolare le precipitazioni orarie in una
determinata zona, oppure vorremmo definire un modo di individuare
automaticamente lo spam in un'applicazione email, o ancora identificare delle
transazioni fraudolente in applicazioni bancarie.

Questo passo è fondamentale per un motivo. A volte il machine learning è visto
come uno strumento "universale", in grado di risolvere qualsiasi problema. Questo
non è vero e il machine learning è applicabile solo a determinati problemi, i
quali alle volte possono essere anche risolti mediante approcci meno complessi.

Una volta verificato che il problema può essere risolto mediante approcci di
machine learning, dovremo stabilire quale sia l'esatta natura del task che
vogliamo portare avanti. Mantenendoci al caso precedente:

<!-- markdownlint-disable MD013 -->

| Applicazione         | Obiettivo del problema                                     | Output del modello                     |
| -------------------- | ---------------------------------------------------------- | -------------------------------------- |
| Previsioni meteo     | Calcolare le precipitazioni orarie in una determinata zona | Predizione delle precipitazione orarie |
| Spam detector        | Individuare lo spam                                        | Alert per un possibile spam            |
| Prevenzione bancaria | Identificare transazioni fraudolente                       | Blocco transazioni sospette            |

<!-- markdownlint-enable MD013 -->

## Comprendere i dati ([⮨](#top))

La disponibilità di dati per l'analisi è alla base del machine learning. Per
effettuare delle predizioni efficaci è necessario usare dati dotati di un
certo potere predittivo. In particolare, i dati devono essere:

1. **abbondanti**: maggiori sono gli esempi rilevanti a disposizione e migliore
   risulterà essere l'algoritmo risolutivo;
2. **consistenti**: i dati devono essere raccolti usando criteri e strumenti
   ben determinati e coerenti. Ad esempio, un algoritmo meteo beneficerà di dati
   raccolti ogni mese per cento anni, piuttosto che di dati raccolti lungo lo
   stesso arco di tempo ma soltanto nel mese di luglio;
3. **affidabili**: occorre valutare la soltanto dei dati. È possibile
   comprenderla e ritenerla affidabile oppure è solo parzialmente sotto il
   nostro controllo?
4. **disponibili**: è necessario assicurarsi che i dati siano disponibili e
   completamente accessibili. Qualora siano presenti parti del dataset
   parzialmente omesse, sarebbe preferibile trascurarle completamente in fase di
   analisi;
5. **corretti**: è spesso presente una percentuale di dati con feature o label
   non corrette. Per quanto possibile, questi dati andrebbero isolati e rimossi
   in fase di preprocessing;
6. **rappresentativi**: è necessario che il dataset rappresenti in maniera
   completa il fenomeno sottostante, riflettendone accuratamente gli aspetti e
   le caratteristiche. Utilizzare un dataset non rappresentativo inficerà
   negativamente sulle performance predittive del modello.

## Scegliere il modello ([⮨](#top))

L'ultimo step è la scelta del tipo di modello da utilizzare, valutando ad
esempio tra **classificazione**, **regressione** e **clustering**.

Per la nostra applicazione meteo, ad esempio, predire il quantitativo di
pioggia che cadrà in un determinato luogo è un chiaro problema di regressione,
nel senso che date $n$ variabili indipendenti cercheremo di predire una
variabile dipendente in uscita.

> <details>
> <summary>ℹ️ <em>Regressione univariata e multivariata</em></summary>
>
> In questo caso, la regressione si dice _univariata_ a causa del fatto che si
> sta predicendo un'unica variabile dipendente. Se provassimo a predire (ad
> esempio) anche la temperatura, avremmo a che fare con una regressione
> _multivariata_.
>
> </details>

Nel caso dell'applicazione mail, dato che stiamo cercando di valutare se un
messaggio è classificabile o meno come spam, avremo a che fare con un problema
di classificazione binaria.

Una volta determinato il tipo di problema dovremo scegliere l'algoritmo da
utilizzare e, in ultimo, la metrica con cui valutare i risultati. In
particolare, quest'ultimo valore dipende molto dall'ambito applicativo: se un
errore del $10\%$ potrebbe non essere estremamente importante nell'applicazione
mail, questo diventerebbe estremamente rilevante e potenzialmente disastroso
nell'individuazione di transazioni fraudolente.

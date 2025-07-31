## Panoramica del Progetto

---

# Capturing Picasso’s Artistic Mind: Modellazione matematica dei processi cognitivi

## Descrizione

Questo progetto adotta un approccio computazionale per analizzare l'opera artistica di Pablo Picasso mediante l'estrazione di metriche visive, tecniche di clustering e analisi scientifica.

L'obiettivo è identificare pattern ricorrenti, strutture complesse e potenziali attrattori visivi nello spazio delle metriche.

## Obiettivi principali

- Pulizia e gestione dei dati
- Calcolo di metriche visive
- Clustering basato su queste metriche
- Identificazione di attrattori tramite Mean-Shift
- Visualizzazione ridotta con UMAP e t-SNE
- Esplorazione dinamica tramite biforcazioni

## Metriche calcolate

1. **Dimensione frattale**
2. **Entropia**
3. **Misura di Birkhoff**
4. **Distanza euclidea**

## Fasi dell’analisi

1. Conteggio delle opere (immagini scaricate + elementi nel JSON)  
   *(Dataset: [link Google Drive](https://drive.google.com/drive/u/1/folders/1oBE6t9TOheAfzASu3EZP70e6tsHTNr3I))*
2. Organizzazione per stile, secondo i metadati nel JSON
3. Verifica e conteggio per ciascuna categoria stilistica
4. Rimozione cartelle con meno di 10 opere (non significative)
5. Nuovo conteggio delle opere post-cleaning
6. Calcolo delle metriche visive

## Analisi effettuate

- ANOVA e MANOVA
- Clustering con Mean-Shift (bandwidth = 0.25 × mediana)
- Distribuzione Kernel (univariata e bivariata)
- Contextual Fit
- Misura di Hausdorff
- Statistiche aggregate per cluster (medie per metrica)
- Istogrammi comparativi
- Riduzione dimensionale (UMAP e t-SNE)
- Rilevamento di possibili attrattori visivi
- Interazioni social delle opere di Picasso

## Visualizzazioni incluse

- Istogrammi per cluster e metrica
- Dendrogramma
- Wordcloud
- Scatterplot 2D e 3D
- Proiezioni 2D (UMAP e t-SNE)
- Diagrammi di biforcazione (adattati al dominio visivo)


---

## Sezione 1: Conteggio delle Opere e Dati Preliminari

* **Scopo:** Stabilire il numero totale di opere d'arte e metadati disponibili per l'analisi.
* **Input:** File immagine (`.jpg`, `.jpeg`, `.png`) da diverse sottocartelle e un file JSON contenente metadati (`pablo-picasso.json`).
* **Metodi:** Scansione ricorsiva delle directory per il conteggio delle immagini; caricamento e ispezione del file JSON per il conteggio degli elementi.
* **Output:** Numero totale di immagini contate nelle cartelle e numero di elementi/record nel file JSON.
* **Insights:** Confermato un totale di 1170 opere d'arte (immagini). Il conteggio del file JSON indica la dimensione del dataset di metadati associato, essenziale per le analisi future.
* **Visualizzazioni chiave:** Nessuna visualizzazione diretta, output numerici.

---

## Sezione 2: Normalizzazione e Organizzazione dei Dati

* **Scopo:** Pulire e standardizzare i metadati delle opere, risolvendo problemi di omonimia e categorizzando le opere per periodo stilistico.
* **Input:** File JSON con i metadati delle opere (`pablo-picasso.json`) e le directory contenenti le immagini delle opere.
* **Metodi:**
    * **Rinominazione "Untitled":** Scansione del JSON per opere con titolo "Untitled" e assegnazione di un suffisso numerico progressivo (es. "Untitled_1").
    * **Organizzazione per Periodo:** Creazione di una nuova struttura di directory (`pablo-picasso-period`) dove le opere sono copiate in sottocartelle basate sul loro "periodo" stilistico. Vengono eliminate le opere senza un "period" definito.
    * **Numerazione Titoli Duplicati:** Identificazione e rinominazione delle opere con titoli identici (ignorando la case) aggiungendo un suffisso numerico (es. "Les Demoiselles d'Avignon_1"). La prima occorrenza mantiene il titolo originale.
* **Output:** File JSON aggiornato con titoli normalizzati; nuove directory di immagini organizzate per periodo stilistico.
* **Insights:** La normalizzazione dei titoli "Untitled" e l'eliminazione dei record incompleti migliorano la coerenza del dataset. L'organizzazione per periodo facilita future analisi stilistiche e la navigazione del corpus di opere. La numerazione dei titoli duplicati assicura un'identificazione univoca di ciascuna opera.
* **Visualizzazioni chiave:** Nessuna visualizzazione diretta; l'output è una struttura di file e un file JSON modificato.

---

## Sezione 3: Verifica e Consolidamento delle Opere Organizzate

* **Scopo:** Confermare l'esito delle operazioni di pulizia e organizzazione, ricontrollando il numero totale di opere dopo la categorizzazione per periodo.
* **Input:** La nuova cartella organizzata per periodo (`.\\DATA\\pablo-picasso-period`) contenente le immagini delle opere.
* **Metodi:** Scansione ricorsiva della directory `pablo-picasso-period` per contare il numero totale di file immagine presenti.
* **Output:** Il conteggio finale delle opere organizzate per periodo.
* **Insights:** Il conteggio finale di **1105 opere** conferma che le opere senza un "periodo" definito sono state rimosse e che il processo di organizzazione è stato eseguito correttamente. Questo dataset consolidato è ora pronto per le fasi successive di analisi.
* **Visualizzazioni chiave:** Nessuna visualizzazione diretta; l'output è un conteggio numerico.

---

## Sezione 4: Calcolo delle Metriche di Immagine

* **Scopo:** Quantificare le caratteristiche visive di ciascuna opera attraverso metriche matematiche, preparandole per analisi stilistiche e comparative.
* **Input:** Immagini delle opere d'arte organizzate per periodo (`.\\DATA\\pablo-picasso-period`).
* **Metodi:** Per ogni immagine, convertita in scala di grigi, vengono calcolate le seguenti metriche:
    * **Dimensione Frattale (Box-Counting):** Misura la complessità e la rugosità del contorno dell'immagine.
    * **Entropia di Shannon:** Quantifica la quantità di informazione o la casualità della distribuzione dei pixel.
    * **Misura di Birkhoff:** Calcolata come rapporto tra il perimetro totale e la radice quadrata dell'area totale di una forma binaria derivata dall'immagine. Indica la complessità del contorno rispetto alla dimensione dell'oggetto.
    * **Norma Euclidea (Frobenius):** Misura la "grandezza" o l'intensità complessiva dell'immagine, normalizzata tra 0 e 1.
* **Output:** Un file CSV (`.\\CSV\\metrics.csv`) contenente per ogni opera il periodo, il nome e i valori calcolati delle quattro metriche.
* **Insights:** Queste metriche forniscono una rappresentazione numerica degli stili pittorici di Picasso. Ad esempio, una dimensione frattale elevata potrebbe indicare opere con dettagli complessi e forme irregolari (es. cubismo), mentre l'entropia potrebbe rivelare la variabilità tonale. La misura di Birkhoff e la Norma Euclidea offrono ulteriori angolazioni sulla complessità strutturale e l'intensità visiva delle composizioni. Questi dati quantitativi sono fondamentali per un'analisi oggettiva delle evoluzioni stilistiche.
* **Visualizzazioni chiave:** Nessuna visualizzazione diretta in questa fase; l'output è un dataset tabellare.

---

## Sezione 5: Integrazione e Pulizia dei Dati Aggiuntivi

* **Scopo:** Arricchire il dataset delle metriche con informazioni temporali e stilistiche e rimuovere eventuali record incompleti.
* **Input:** File CSV con le metriche calcolate (`.\\CSV\\metrics.csv`) e il file JSON dei metadati originali (`.\\DATA\\pablo-picasso.json`).
* **Metodi:**
    * **Unione Dati:** Le colonne 'Year', 'Period' e 'Style' vengono estratte dal JSON e aggiunte al CSV, utilizzando il nome dell'opera come chiave di corrispondenza. Viene applicata una normalizzazione ai nomi delle opere per garantire un matching robusto (rimozione di caratteri speciali, conversione in minuscolo).
    * **Gestione Valori Mancanti:** Tutte le righe del dataset risultante che presentano valori nulli nelle colonne 'Year', 'Period' o 'Style' vengono eliminate per garantire l'integrità dei dati per le analisi successive.
* **Output:** Un nuovo file CSV (`.\\CSV\\metrics_with_details.csv`) contenente le metriche di immagine arricchite con l'anno di completamento, il periodo e lo stile di ciascuna opera, con tutte le righe incomplete rimosse.
* **Insights:** L'aggiunta dell'anno e del periodo è cruciale per condurre analisi temporali sull'evoluzione dello stile di Picasso. La rimozione delle righe con valori nulli assicura che le analisi successive (es. modellazione predittiva o analisi statistiche) si basino su un dataset pulito e completo, migliorando l'affidabilità dei risultati.
* **Visualizzazioni chiave:** Nessuna visualizzazione diretta; l'output è un dataset tabellare aggiornato.

---

## Sezione 6: Rimozione degli Outlier e Conteggio Post-Pulizia

* **Scopo:** Identificare ed eliminare i valori anomali (outlier) dalle metriche calcolate, garantendo che le analisi successive si basino su dati robusti e rappresentativi. Successivamente, viene effettuato un riconteggio delle opere per confermare il dataset finale.
* **Input:** Il file CSV contenente le metriche delle opere e i dettagli (`.\\CSV\\metrics_with_details.csv`).
* **Metodi:**
    * **Caricamento e Preparazione:** Il CSV viene caricato, e le colonne numeriche (`Fractal dimension`, `Entropy`, `Birkhoff measure`, `Euclidean norm`) vengono convertite nel tipo numerico, gestendo eventuali errori di conversione e rimuovendo le righe con valori NaN in queste colonne.
    * **Rimozione Outlier IQR:** Per ciascun "Periodo" stilistico e per ogni metrica numerica, vengono calcolati il Primo Quartile (Q1), il Terzo Quartile (Q3) e il Rango Interquartile (IQR). Le opere i cui valori ricadono al di fuori dell'intervallo $[Q1 - 1.5 \times IQR, Q3 + 1.5 \times IQR]$ vengono considerate outlier e rimosse. Questo processo è iterativo e può essere ripetuto fino a quando non ci sono più outlier visibili.
    * **Visualizzazione Outlier:** Vengono generati boxplot per ciascuna metrica, raggruppati per periodo, per visualizzare la distribuzione dei dati e l'efficacia della rimozione degli outlier. Questi grafici vengono salvati nella directory `.\\PLOT\\BOXPLOT\\`.
    * **Conteggio Finale:** Dopo la rimozione degli outlier, viene ricalcolato il numero totale di opere e il conteggio delle opere per ciascun periodo stilistico.
* **Output:** Un nuovo file CSV (`.\\CSV\\metrics_with_details_no_outlier.csv`) contenente il dataset ripulito dagli outlier. Vengono prodotte diverse visualizzazioni (boxplot) che mostrano la distribuzione delle metriche per periodo, senza outlier. Viene inoltre fornito un riepilogo del conteggio delle opere per ciascun periodo dopo la pulizia.
* **Insights:** La rimozione degli outlier è fondamentale per prevenire che valori estremi distorcano le analisi statistiche e i modelli predittivi. Il conteggio post-pulizia indica che il dataset finale comprende **852 opere**, con una distribuzione per periodo che ora riflette più accuratamente il corpus centrale delle opere di Picasso dopo l'eliminazione dei dati anomali. Le visualizzazioni dei boxplot confermano che i dati sono ora più concentrati e pronti per analisi comparative significative tra i diversi periodi.
* **Visualizzazioni chiave:** Boxplot di "Fractal dimension", "Entropy", "Birkhoff measure" e "Euclidean norm" raggruppati per "Periodo", mostrando le distribuzioni dopo la rimozione degli outlier.

---

## Sezione 7: Normalizzazione delle Metriche

* **Scopo:** Portare tutte le metriche calcolate su una scala comune (tra 0 e 1) per eliminare le differenze di magnitudine tra di esse, rendendole comparabili e adatte per algoritmi che sono sensibili alla scala dei dati (es. molti algoritmi di machine learning).
* **Input:** Il file CSV delle metriche pulito dagli outlier (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset viene caricato.
    * **Controllo e Pulizia:** Le colonne delle metriche ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance") vengono esplicitamente convertite in tipo numerico. Eventuali valori non numerici (risultanti da errori o conversioni precedenti) vengono trasformati in `NaN` e le righe contenenti questi `NaN` vengono rimosse dal dataset.
    * **Normalizzazione Min-Max:** Viene applicato l'algoritmo **MinMaxScaler** di Scikit-learn, che scala i valori di ogni metrica in modo che il valore minimo diventi 0 e il valore massimo diventi 1, mantenendo le relazioni relative tra i dati.
* **Output:** Un nuovo file CSV (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`) dove tutte le colonne delle metriche sono state normalizzate nell'intervallo [0, 1].
* **Insights:** La normalizzazione è un passo cruciale di pre-elaborazione che migliora le prestazioni di molti algoritmi analitici e di machine learning, garantendo che nessuna metrica domini l'analisi a causa della sua scala intrinseca. Questo rende i dati pronti per compiti come il clustering, la classificazione o l'analisi di regressione, dove la comparabilità tra feature è fondamentale.
* **Visualizzazioni chiave:** Nessuna visualizzazione diretta; l'output è un dataset tabellare con valori scalati.

---

## Sezione 8: Analisi Statistica Normalizzata per Periodo

* **Scopo:** Esaminare le differenze statistiche nelle metriche normalizzate tra i diversi periodi artistici di Picasso, identificando quali metriche mostrano variazioni significative nel tempo e tra periodi consecutivi.
* **Input:** Il file CSV con le metriche normalizzate e i dettagli delle opere (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Calcolo di Media e Deviazione Standard:** Per ciascun periodo ("Early Years", "Blue Period", "Rose Period", "African Period", "Cubist Period", "Neoclassicist & Surrealist Period", "Later Years"), vengono calcolate la media e la deviazione standard per ciascuna delle quattro metriche ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance"). I risultati vengono salvati in un CSV (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`).
    * **Analisi MANOVA (Multivariate Analysis of Variance):** Viene eseguita una MANOVA per confrontare simultaneamente le medie di tutte le metriche tra coppie di periodi consecutivi cronologicamente. La MANOVA determina se esistono differenze statisticamente significative tra i gruppi quando si considerano più variabili dipendenti insieme.
    * **Analisi ANOVA (Analysis of Variance) Univariata (post-hoc):** Se la MANOVA indica una differenza significativa (p-value < 0.05), viene condotta un'ANOVA univariata per ciascuna metrica individualmente, sempre tra le coppie di periodi consecutivi. Questo aiuta a identificare quali specifiche metriche contribuiscono alla significatività complessiva riscontrata con la MANOVA.
    * **Visualizzazione dei Trend:** Vengono creati dei grafici a linee che mostrano il trend delle medie di ciascuna metrica attraverso i periodi cronologicamente ordinati. Questi grafici vengono salvati come immagini PNG nella cartella `.\\PLOT NORMALIZED\\TREND\\`.
* **Output:**
    * Un file CSV (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`) con le medie e le deviazioni standard delle metriche per ogni periodo.
    * Un file CSV (`.\\CSV NORMALIZED\\normalized_manova_analysis_by_period.csv`) che riassume i p-value della MANOVA per ogni coppia di periodi consecutivi e indica quali metriche sono risultate significativamente diverse nell'ANOVA univariata.
    * Immagini PNG (`.\\PLOT NORMALIZED\\TREND\\normalized_metrics_trend.png`) che mostrano i trend delle metriche nel tempo.
* **Insights:** L'analisi statistica rivelerà se e come le caratteristiche visive delle opere di Picasso (misurate dalle metriche) sono cambiate significativamente da un periodo all'altro. I grafici dei trend visualizzeranno queste evoluzioni. La MANOVA e le successive ANOVA forniranno una base statistica per affermare che le transizioni tra i periodi non sono solo nominali ma si riflettono in cambiamenti quantificabili nello stile visivo delle opere. Questo passaggio è cruciale per comprendere l'evoluzione artistica di Picasso da una prospettiva basata sui dati.

---

## Sezione 8.1: Analisi Statistica con Rispetto al Periodo (Dati Normalizzati)

* **Scopo:** Approfondire l'analisi delle metriche estratte dalle opere di Picasso, concentrandosi sulle differenze statistiche tra i vari periodi artistici, utilizzando i dati **normalizzati** ottenuti nel passaggio precedente. L'obiettivo è quantificare e visualizzare l'evoluzione dello stile di Picasso attraverso le fasi della sua carriera.
* **Input:** Il file CSV contenente le metriche normalizzate e i dettagli delle opere (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Calcolo di Media e Deviazione Standard per Periodo:** Per ciascun periodo artistico definito ("Early Years", "Blue Period", "Rose Period", "African Period", "Cubist Period", "Neoclassicist & Surrealist Period", "Later Years"), vengono calcolate la media e la deviazione standard per ciascuna delle quattro metriche ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance"). Questo fornisce una prima panoramica delle tendenze centrali e della dispersione delle metriche all'interno di ogni fase. I risultati vengono salvati in un CSV (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`).
    * **Analisi MANOVA (Multivariate Analysis of Variance):** Viene condotta un'analisi MANOVA per confrontare simultaneamente le medie di tutte le metriche tra coppie di periodi consecutivi nella cronologia artistica di Picasso. La MANOVA è utilizzata per determinare se ci sono differenze statisticamente significative tra i gruppi (periodi) quando si considerano più variabili dipendenti (le metriche) contemporaneamente. Questo aiuta a capire se l'evoluzione tra un periodo e l'altro ha un impatto collettivo sulle caratteristiche delle opere.
    * **Analisi ANOVA (Analysis of Variance) Univariata (Post-Hoc):** Se la MANOVA rivela una differenza complessiva significativa tra due periodi consecutivi (p-value < 0.05), vengono eseguite delle analisi ANOVA univariate per ciascuna metrica individualmente. Questo passaggio post-hoc serve a identificare quali specifiche metriche contribuiscono maggiormente alla significatività complessiva osservata dalla MANOVA, indicando quali aspetti dello stile di Picasso sono cambiati in modo più marcato tra i periodi.
    * **Visualizzazione dei Trend delle Medie:** Vengono generati grafici a linee che illustrano l'andamento delle medie di ciascuna metrica attraverso i periodi artistici, ordinati cronologicamente. Queste visualizzazioni permettono di cogliere immediatamente le variazioni e le evoluzioni stilistiche di Picasso nel corso del tempo, rendendo evidenti i cambiamenti quantificabili nelle proprietà delle sue opere.
* **Output:**
    * Un file CSV (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`) contenente le medie e le deviazioni standard delle metriche per ogni periodo artistico.
    * Un file CSV (`.\\CSV NORMALIZED\\normalized_manova_analysis_by_period.csv`) che riassume i p-value della MANOVA per ogni coppia di periodi consecutivi e indica quali metriche sono risultate significativamente diverse nell'ANOVA univariata.
    * Immagini PNG (`.\\PLOT NORMALIZED\\TREND\\normalized_metrics_trend.png`) che mostrano i trend delle metriche nel tempo.
* **Insights:** Questa fase di analisi statistica è **fondamentale** per convalidare in modo quantitativo le transizioni tra i periodi artistici di Picasso. I risultati indicheranno non solo se ci sono state differenze significative nello stile, ma anche quali specifiche metriche (e quindi quali proprietà visive) sono state le più influenti in tali cambiamenti. I grafici dei trend forniranno una rappresentazione intuitiva di queste evoluzioni, completando l'analisi numerica con un contesto visivo.

---

## Sezione 8.2: Analisi del Cambiamento Medio nelle Metriche

* **Scopo:** Quantificare l'entità dei cambiamenti nelle metriche visive delle opere di Picasso tra periodi artistici consecutivi, fornendo una misura diretta dell'evoluzione stilistica.
* **Input:** Il file CSV contenente le medie delle metriche per ogni periodo (`.\\CSV\\metrics_by_period.csv`).
* **Metodi:**
    * **Caricamento e Preparazione Dati:** Il dataset delle medie per periodo viene caricato. Le colonne numeriche vengono convertite in formato float, gestendo la virgola come separatore decimale.
    * **Ordinamento Cronologico:** I periodi artistici di Picasso vengono ordinati cronologicamente ("Early Years", "Blue Period", "Rose Period", "African Period", "Cubist Period", "Neoclassicist & Surrealist Period", "Later Years").
    * **Calcolo delle Differenze Consecutive:** Per ogni metrica (Dimensione Frattale, Entropia, Misura di Birkhoff, Norma Euclidea), viene calcolata la differenza tra la media del periodo successivo e la media del periodo corrente. Questo evidenzia l'incremento o il decremento di ciascuna caratteristica visiva nel passaggio da una fase artistica all'altra.
* **Output:** Un nuovo file CSV (`.\\CSV\\metric_variations_by_period.csv`) che elenca ogni metrica, la transizione tra i periodi (es. "Early Years to Blue Period") e il valore della differenza media riscontrata.
* **Insights:** Questa analisi fornisce una **misura diretta e quantificabile** dell'evoluzione stilistica di Picasso. Ad esempio, una differenza positiva significativa nella "Dimensione Frattale" tra il Periodo Blu e il Cubismo indicherebbe un aumento della complessità e della frammentazione delle forme. Questi dati sono essenziali per comprendere non solo *se* lo stile è cambiato, ma anche *come* e *in che misura* ciascuna caratteristica visiva è mutata nel corso della sua carriera.
* **Visualizzazioni chiave:** Nessuna visualizzazione diretta in questa fase; l'output è un dataset tabellare che quantifica i cambiamenti.

---

## Sezione 9: Visualizzazione 3D e Clustering (Dati Normalizzati)

* **Scopo:** Visualizzare le relazioni tra i diversi periodi artistici di Picasso nello spazio delle metriche normalizzate e identificare raggruppamenti stilistici tramite l'analisi di clustering gerarchico. Questo aiuta a comprendere l'evoluzione stilistica da una prospettiva multidimensionale.
* **Input:** Il file CSV contenente le metriche normalizzate e i dettagli delle opere (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento e Preparazione Dati:** Il dataset normalizzato viene caricato. Si garantisce che la colonna 'Period' sia di tipo categorico e ordinata cronologicamente. Le colonne delle metriche sono convertite a numerico e le righe con valori mancanti nelle metriche o nel periodo vengono rimosse.
    * **Calcolo Medie per Periodo:** Vengono calcolate le medie di tutte le metriche per ciascun periodo artistico.
    * **Dendrogramma (Clustering Gerarchico):**
        * Viene applicato il **clustering gerarchico** (metodo 'ward') utilizzando le medie della 'Dimensione Frattale' e dell' 'Entropia' per ogni periodo.
        * Viene generato un **dendrogramma** che visualizza le relazioni di similarità tra i periodi. Un'eventuale linea di soglia orizzontale può aiutare a identificare potenziali cluster.
        * Il dendrogramma viene salvato come immagine PNG (`.\\PLOT NORMALIZED\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * **Grafici Scatter 3D con Traiettoria Cronologica:**
        * Per tutte le combinazioni possibili di tre metriche (es. (Dimensione Frattale, Entropia, Misura di Birkhoff), (Entropia, Misura di Birkhoff, Norma Euclidea), ecc.), vengono creati dei grafici scatter 3D.
        * Ogni punto nel grafico rappresenta un periodo artistico, posizionato in base ai valori medi delle tre metriche selezionate.
        * Vengono aggiunte **frecce cronologiche** che connettono i punti dei periodi successivi, visualizzando il "percorso evolutivo" di Picasso nello spazio delle metriche.
        * Ogni grafico 3D viene salvato come immagine PNG (`.\\PLOT NORMALIZED\\3D DISPLAY\\Evolutionary Path of Picasso's Styles (Space of ...).png`).
* **Output:**
    * Un'immagine PNG del dendrogramma (`.\\PLOT NORMALIZED\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * Diverse immagini PNG dei grafici scatter 3D, una per ogni combinazione di tre metriche, che mostrano la traiettoria cronologica dei periodi nello spazio tridimensionale delle metriche (`.\\PLOT NORMALIZED\\3D DISPLAY\\...png`).
* **Insights:** Il **dendrogramma** aiuterà a identificare quali periodi stilistici sono più simili tra loro in termini di complessità e casualità visiva, suggerendo possibili raggruppamenti naturali nello stile di Picasso. I **grafici 3D** offriranno una rappresentazione intuitiva e dinamica dell'evoluzione del suo stile, mostrando come le opere si sono spostate nello spazio delle caratteristiche visive nel corso della sua carriera. Le traiettorie cronologiche evidenzieranno le direzioni principali del cambiamento stilistico.

---

## Sezione 9.1: Visualizzazione 3D e Clustering (Dati Non Normalizzati)

* **Scopo:** Visualizzare le relazioni tra i diversi periodi artistici di Picasso nello spazio delle metriche **non normalizzate** e identificare raggruppamenti stilistici tramite l'analisi di clustering gerarchico. Questo serve come confronto con l'analisi su dati normalizzati, per osservare l'impatto della scala delle metriche sulle relazioni.
* **Input:** Il file CSV contenente le metriche originali (non normalizzate) e i dettagli delle opere (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento e Preparazione Dati:** Il dataset viene caricato. Si garantisce che la colonna 'Period' sia di tipo categorico e ordinata cronologicamente. Le colonne delle metriche sono convertite a numerico e le righe con valori mancanti nelle metriche o nel periodo vengono rimosse.
    * **Calcolo Medie per Periodo:** Vengono calcolate le medie di tutte le metriche per ciascun periodo artistico.
    * **Dendrogramma (Clustering Gerarchico):**
        * Viene applicato il **clustering gerarchico** (metodo 'ward') utilizzando le medie della 'Dimensione Frattale' e dell' 'Entropia' per ogni periodo.
        * Viene generato un **dendrogramma** che visualizza le relazioni di similarità tra i periodi. Una linea di soglia orizzontale di esempio può aiutare a identificare potenziali cluster.
        * Il dendrogramma viene salvato come immagine PNG (`.\\PLOT\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * **Grafici Scatter 3D con Traiettoria Cronologica:**
        * Per tutte le combinazioni possibili di tre metriche, vengono creati dei grafici scatter 3D.
        * Ogni punto nel grafico rappresenta un periodo artistico, posizionato in base ai valori medi delle tre metriche selezionate.
        * Vengono aggiunte **frecce cronologiche** che connettono i punti dei periodi successivi, visualizzando il "percorso evolutivo" di Picasso nello spazio delle metriche.
        * Ogni grafico 3D viene salvato come immagine PNG (`.\\PLOT\\3D DISPLAY\\Evolutionary Path of Picasso's Styles (Space of ...).png`).
* **Output:**
    * Un'immagine PNG del dendrogramma (`.\\PLOT\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * Diverse immagini PNG dei grafici scatter 3D, una per ogni combinazione di tre metriche, che mostrano la traiettoria cronologica dei periodi nello spazio tridimensionale delle metriche (`.\\PLOT\\3D DISPLAY\\...png`).
* **Insights:** Questa sezione replica l'analisi della Sezione 9 utilizzando i dati non normalizzati. Il confronto tra i risultati dei due step (9 e 9.1) è cruciale: il dendrogramma e i grafici 3D sui dati non normalizzati potrebbero mostrare **raggruppamenti e traiettorie differenti** rispetto a quelli ottenuti con i dati normalizzati. Questo metterà in evidenza come la scala delle metriche possa influenzare l'interpretazione delle relazioni stilistiche e l'importanza della normalizzazione per algoritmi sensibili alla scala.

---

## Sezione 10: Visualizzazione Box Plot (Dati Normalizzati)

* **Scopo:** Visualizzare la distribuzione delle metriche chiave (media ± deviazione standard) per periodi artistici consecutivi di Picasso, utilizzando i dati **normalizzati**. Questo aiuta a comprendere la sovrapposizione e la separazione stilistica tra le fasi della sua carriera.
* **Input:** Il file CSV contenente le medie e le deviazioni standard delle metriche normalizzate per ogni periodo (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset delle medie e deviazioni standard per periodo viene caricato.
    * **Generazione Box Plot 2D (con rettangoli di deviazione standard):** Vengono creati una serie di grafici 2D, ognuno focalizzato su due periodi consecutivi e su una specifica coppia di metriche normalizzate.
        * Per ogni periodo, viene disegnato un rettangolo centrato sulla media delle due metriche e con lati pari a due volte la deviazione standard di ciascuna metrica (media ± deviazione standard). Questo rettangolo rappresenta l'intervallo di variabilità più comune delle opere in quel periodo rispetto a quelle metriche.
        * Un punto al centro del rettangolo indica la media esatta.
        * I periodi analizzati e le metriche visualizzate sono:
            * **Early Years vs Blue Period:** Entropia (X-axis) e Distanza Euclidea (Y-axis).
            * **Blue Period vs Rose Period:** Dimensione Frattale (X-axis) e Distanza Euclidea (Y-axis).
            * **Rose Period vs African Period:** Dimensione Frattale (X-axis) e Distanza Euclidea (Y-axis).
            * **African Period vs Cubist Period:** Dimensione Frattale (X-axis) e Distanza Euclidea (Y-axis).
            * **Cubist Period vs Neoclassicist & Surrealist Period:** Misura di Birkhoff (X-axis) e Distanza Euclidea (Y-axis).
            * **Neoclassicist & Surrealist Period vs Later Years:** Entropia (X-axis) e Dimensione Frattale (Y-axis).
        * Ogni grafico viene salvato come immagine PNG nella directory `.\\PLOT NORMALIZED\\BOX DISPLAY\\`.
* **Output:** Diversi file immagine PNG (`.\\PLOT NORMALIZED\\BOX DISPLAY\\... .png`), ciascuno raffigurante i rettangoli di deviazione standard per una coppia di periodi consecutivi, visualizzando la loro sovrapposizione o separazione nello spazio delle metriche selezionate.
* **Insights:** Questa visualizzazione è fondamentale per comprendere la **distinzione e la sovrapposizione** tra i diversi periodi stilistici di Picasso in termini quantitativi. I rettangoli che si sovrappongono suggeriscono una continuità o similarità tra gli stili, mentre quelli separati indicano transizioni più nette e distintive. La dimensione del rettangolo (determinata dalla deviazione standard) fornisce anche un'indicazione della **variabilità stilistica** all'interno di ciascun periodo. L'utilizzo di dati normalizzati assicura che il confronto non sia influenzato dalle diverse scale delle metriche.

---

## Sezione 10.1: Visualizzazione Box Plot (Dati Non Normalizzati)

* **Scopo:** Visualizzare la distribuzione delle metriche chiave (media ± deviazione standard) per periodi artistici consecutivi di Picasso, utilizzando i dati **non normalizzati**. Questa sezione è cruciale per confrontare l'impatto della normalizzazione sull'interpretazione delle relazioni stilistiche.
* **Input:** Il file CSV contenente le medie e le deviazioni standard delle metriche originali (non normalizzate) per ogni periodo (`.\\CSV\\metrics_by_period.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset delle medie e deviazioni standard per periodo viene caricato.
    * **Generazione Box Plot 2D (con rettangoli di deviazione standard):** Vengono creati una serie di grafici 2D, ognuno focalizzato su due periodi consecutivi e su una specifica coppia di metriche non normalizzate.
        * Per ogni periodo, viene disegnato un rettangolo centrato sulla media delle due metriche e con lati pari a due volte la deviazione standard di ciascuna metrica (media ± deviazione standard). Questo rettangolo visualizza l'intervallo di variabilità più comune delle opere in quel periodo.
        * Un punto al centro del rettangolo indica la media esatta.
        * I periodi analizzati e le metriche visualizzate sono:
            * **Early Years vs Blue Period:** Distanza Euclidea (X-axis) ed Entropia (Y-axis).
            * **Blue Period vs Rose Period:** Dimensione Frattale (X-axis) e Distanza Euclidea (Y-axis).
            * **Rose Period vs African Period:** Dimensione Frattale (X-axis) e Distanza Euclidea (Y-axis).
            * **African Period vs Cubist Period:** Dimensione Frattale (X-axis) e Distanza Euclidea (Y-axis).
            * **Cubist Period vs Neoclassicist & Surrealist Period:** Misura di Birkhoff (X-axis) e Distanza Euclidea (Y-axis).
            * **Neoclassicist & Surrealist Period vs Later Years:** Entropia (X-axis) e Dimensione Frattale (Y-axis).
        * Ogni grafico viene salvato come immagine PNG nella directory `.\\PLOT\\BOX DISPLAY\\` con l'indicazione "(Unnormalized Data)" nel titolo e nel nome del file per chiarezza.
* **Output:** Diversi file immagine PNG (`.\\PLOT\\BOX DISPLAY\\... (Unnormalized).png`), ciascuno raffigurante i rettangoli di deviazione standard per una coppia di periodi consecutivi, basati su dati non normalizzati.
* **Insights:** Questa sezione permette un **confronto diretto** con la Sezione 10. L'analisi dei box plot con dati non normalizzati rivelerà come le differenze nelle scale delle metriche possano alterare l'aspetto delle distribuzioni e delle sovrapposizioni tra i periodi. Si prevede che le differenze di magnitudine tra le metriche possano rendere alcune separazioni più o meno evidenti, sottolineando l'importanza della normalizzazione per un'analisi comparativa equa e per la maggior parte degli algoritmi di apprendimento automatico.

---

## Sezione 11: Clustering K-Means (Dati Normalizzati)

* **Scopo:** Applicare l'algoritmo di clustering K-Means ai dati delle metriche visive normalizzate per identificare raggruppamenti naturali nelle opere di Picasso, basati sulle loro caratteristiche intrinseche piuttosto che sull'ordine cronologico. Si utilizzano 7 cluster, in linea con il numero dei periodi stilistici definiti.
* **Input:** Il file CSV contenente le metriche normalizzate per ogni opera d'arte (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset normalizzato viene caricato, assicurandosi che i decimali siano interpretati correttamente.
    * **Configurazione:** Vengono definite le quattro metriche ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') e il numero di cluster ($k=7$).
    * **Iterazione su Coppie di Metriche:** L'algoritmo K-Means viene eseguito per ogni combinazione unica di due metriche.
        * Per ogni coppia di metriche, i dati vengono selezionati e l'algoritmo K-Means viene inizializzato e addestrato. `random_state=42` assicura la riproducibilità dei risultati.
        * Le etichette dei cluster assegnate a ciascun punto dati (opera d'arte) vengono aggiunte al DataFrame.
        * Vengono calcolati e stampati i centroidi di ciascun cluster, fornendo le coordinate medie di ogni gruppo nello spazio delle metriche.
    * **Visualizzazione dei Cluster:** Per ogni coppia di metriche analizzata, viene generato un grafico a dispersione (scatter plot):
        * I punti rappresentano le singole opere d'arte, colorate in base al cluster assegnato da K-Means.
        * I centroidi di ciascun cluster sono visualizzati come marcatori 'X' rossi e neri, evidenziando il "centro" di ogni gruppo.
        * Il titolo del grafico indica le metriche utilizzate e il numero di cluster.
        * Ogni grafico viene salvato come immagine PNG nella directory `.\\PLOT NORMALIZED\\CLUSTER\\` per una successiva revisione e analisi.
* **Output:**
    * Stampa dei centroidi per ogni coppia di metriche analizzata.
    * Diversi file immagine PNG (`.\\PLOT NORMALIZED\\CLUSTER\\KMeans_Cluster_... .png`), ciascuno mostrando la distribuzione dei cluster K-Means per una specifica coppia di metriche normalizzate.
    * Il DataFrame originale arricchito con nuove colonne che indicano l'assegnazione del cluster per ogni opera d'arte in relazione a ciascuna coppia di metriche considerata.
* **Insights:** Questa sezione mira a scoprire se esistono raggruppamenti stilistici *emergenti* dalle proprietà intrinseche delle opere, indipendentemente dalla classificazione storica dei periodi. Confrontando questi cluster con i periodi predefiniti, si può valutare la coerenza delle metriche nel catturare le transizioni stilistiche. I grafici mostreranno visivamente come le opere si raggruppano in base alle loro caratteristiche quantificabili, e la posizione dei centroidi indicherà le caratteristiche medie di ciascun cluster stilistico identificato dall'algoritmo.

---

## Sezione 11.1: Clustering K-Means (Dati Non Normalizzati)

* **Scopo:** Applicare l'algoritmo di clustering K-Means ai dati delle metriche visive **non normalizzate** per identificare raggruppamenti naturali nelle opere di Picasso. Questa analisi serve come confronto diretto con la Sezione 11 per valutare l'impatto della normalizzazione sui risultati del clustering. Si mantengono $k=7$ cluster per coerenza.
* **Input:** Il file CSV contenente le metriche originali (non normalizzate) per ogni opera d'arte (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset non normalizzato viene caricato, assicurandosi che i decimali siano interpretati correttamente.
    * **Configurazione:** Vengono definite le quattro metriche ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') e il numero di cluster ($k=7$).
    * **Iterazione su Coppie di Metriche:** L'algoritmo K-Means viene eseguito per ogni combinazione unica di due metriche.
        * Per ogni coppia di metriche, i dati vengono selezionati e l'algoritmo K-Means viene inizializzato e addestrato. `random_state=42` assicura la riproducibilità.
        * Le etichette dei cluster assegnate a ciascun punto dati (opera d'arte) vengono aggiunte al DataFrame.
        * Vengono calcolati e stampati i centroidi di ciascun cluster, fornendo le coordinate medie di ogni gruppo nello spazio delle metriche.
    * **Visualizzazione dei Cluster:** Per ogni coppia di metriche analizzata, viene generato un grafico a dispersione (scatter plot):
        * I punti rappresentano le singole opere d'arte, colorate in base al cluster assegnato da K-Means.
        * I centroidi di ciascun cluster sono visualizzati come marcatori 'X' rossi e neri, evidenziando il "centro" di ogni gruppo.
        * Il titolo del grafico indica le metriche utilizzate e il numero di cluster.
        * Ogni grafico viene salvato come immagine PNG nella directory `.\\PLOT\\CLUSTER\\` (diversa da quella dei dati normalizzati) per distinguere chiaramente i risultati.
* **Output:**
    * Stampa dei centroidi per ogni coppia di metriche analizzata.
    * Diversi file immagine PNG (`.\\PLOT\\CLUSTER\\KMeans_Cluster_... .png`), ciascuno mostrando la distribuzione dei cluster K-Means per una specifica coppia di metriche non normalizzate.
    * Il DataFrame originale arricchito con nuove colonne che indicano l'assegnazione del cluster per ogni opera d'arte in relazione a ciascuna coppia di metriche considerata.
* **Insights:** Questa sezione è fondamentale per confrontare l'efficacia del clustering K-Means con e senza normalizzazione. Si prevede che i cluster formati con dati non normalizzati possano essere fortemente influenzati dalla scala delle singole metriche, portando a raggruppamenti meno significativi o dominati dalla metrica con il range di valori più ampio. Questo confronto aiuterà a evidenziare l'importanza della pre-elaborazione dei dati (normalizzazione) per ottenere risultati di clustering più robusti e interpretabili, soprattutto quando le metriche hanno scale diverse.

---

## Sezione 12: Stima della Densità del Kernel (KDE) (Dati Normalizzati)

* **Scopo:** Analizzare la distribuzione di probabilità delle metriche visive di Picasso utilizzando la Stima della Densità del Kernel (KDE) sui dati **normalizzati**. Questa sezione mira a visualizzare la densità dei punti dati nello spazio delle metriche, identificando le aree di maggiore concentrazione e le potenziali sovrapposizioni tra i periodi stilistici.
* **Input:** Il file CSV contenente le metriche normalizzate per ogni opera d'arte (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Preparazione Dati:**
        * Il dataset normalizzato viene caricato.
        * Le colonne delle metriche ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') vengono identificate e convertite in formato numerico. Le righe con valori NaN nelle metriche vengono rimosse per garantire l'integrità del calcolo.
    * **Normalizzazione (Min-Max Scaling):** Sebbene i dati siano già caricati da un file "normalizzato", viene eseguita una ri-normalizzazione Min-Max (tra 0 e 1) sulle metriche per garantire che tutti i valori rientrino in un intervallo coerente, il che è cruciale per il calcolo della Distanza di Mahalanobis e per la visualizzazione KDE.
    * **Calcolo della Distanza di Mahalanobis:**
        * Per ogni periodo artistico (definito dalla colonna 'Period'), viene calcolata la distanza di Mahalanobis di ciascuna opera d'arte dal centroide del proprio periodo.
        * La distanza di Mahalanobis tiene conto della correlazione tra le metriche e della loro varianza, fornendo una misura più robusta della "distanza" rispetto alla semplice distanza euclidea, specialmente in spazi multidimensionali.
        * Viene gestita la potenziale singolarità della matrice di covarianza aggiungendo un piccolo "jitter" per garantirne l'invertibilità.
        * Le distanze calcolate vengono aggiunte al DataFrame in una nuova colonna 'Mahalanobis_Distance_Normalized'.
    * **Calcolo del "Contesto di Adattamento" (Contextual Fit):**
        * Viene introdotta una metrica di "Contesto di Adattamento", definita come l'inverso della Distanza di Mahalanobis (1 / (Distanza di Mahalanobis + epsilon), dove epsilon è un piccolo valore per evitare divisioni per zero).
        * Un valore più alto di "Contesto di Adattamento" indica che un'opera si adatta meglio alle caratteristiche medie del suo periodo.
        * Questa metrica viene aggiunta al DataFrame in una nuova colonna 'Contextual_Fit'.
    * **Calcolo Statistiche Riassuntive:** Vengono calcolate e stampate le medie e le deviazioni standard della Distanza di Mahalanobis normalizzata e del "Contesto di Adattamento" per ciascun periodo artistico, fornendo una sintesi numerica dell'omogeneità interna di ogni periodo.
    * **Generazione di Grafici KDE:**
        * **KDE Univariata (2x2 Subplot):** Viene generato un singolo grafico contenente quattro subplot, ognuno dei quali mostra la distribuzione di densità di probabilità (KDE) per una singola metrica (Dimensione Frattale, Entropia, Misura di Birkhoff, Distanza Euclidea). Questo fornisce una visione d'insieme della distribuzione di ciascuna metrica in tutto il dataset.
        * **KDE Bivariata (per Coppie di Metriche):** Vengono generati grafici KDE separati per ogni combinazione unica di due metriche. Questi grafici mostrano la densità congiunta delle due metriche, evidenziando le aree dove le opere sono più concentrate e le relazioni tra le metriche. Vengono aggiunti anche i singoli punti dati per fornire contesto.
        * Tutti i grafici vengono salvati come immagini PNG nella directory `.\\PLOT NORMALIZED\\KDE PLOT\\`.
* **Output:**
    * Il DataFrame arricchito con le colonne 'Mahalanobis_Distance_Normalized' e 'Contextual_Fit'.
    * Stampa delle medie e deviazioni standard delle distanze di Mahalanobis e del "Contesto di Adattamento" per periodo.
    * Un file immagine PNG (`.\\PLOT NORMALIZED\\KDE PLOT\\all_univariate_kde_2x2_subplot.png`) che mostra le distribuzioni univariate di tutte le metriche.
    * Diversi file immagine PNG (`.\\PLOT NORMALIZED\\KDE PLOT\\kde_bivariate_... .png`), ciascuno raffigurante la distribuzione di densità congiunta per una coppia di metriche normalizzate.
* **Insights:** Questa sezione fornisce una comprensione approfondita della distribuzione delle metriche e della coesione interna dei periodi. I grafici KDE visualizzeranno le "impronte" stilistiche di Picasso, mostrando come le metriche si raggruppano e si sovrappongono. Le distanze di Mahalanobis e il "Contesto di Adattamento" quantificheranno quanto le singole opere si discostano o si conformano al prototipo del loro periodo, fornendo una base per valutare la fluidità o la rigidità delle transizioni stilistiche.

---

## Sezione 12.1: Stima della Densità del Kernel (KDE) (Dati Non Normalizzati)

* **Scopo:** Analizzare la distribuzione di probabilità delle metriche visive di Picasso utilizzando la Stima della Densità del Kernel (KDE) sui dati **non normalizzati**. Questa sezione è cruciale per confrontare l'impatto della normalizzazione sull'interpretazione delle relazioni stilistiche e sulla forma delle distribuzioni.
* **Input:** Il file CSV contenente le metriche originali (non normalizzate) per ogni opera d'arte (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Preparazione Dati:**
        * Il dataset originale (non normalizzato) viene caricato.
        * Le colonne delle metriche ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') vengono identificate e convertite in formato numerico. Le righe con valori NaN nelle metriche vengono rimosse.
    * **Normalizzazione (Min-Max Scaling):** A differenza della Sezione 12, qui i dati originali vengono normalizzati Min-Max (tra 0 e 1) *all'interno di questa sezione* prima di calcolare la Distanza di Mahalanobis. Questo passaggio è intenzionale per permettere il calcolo della Mahalanobis su dati scalati, anche se il focus è sui KDE dei dati non normalizzati. Per i KDE, viene utilizzato il `df_cleaned_for_kde` che contiene i valori originali (non normalizzati).
    * **Calcolo della Distanza di Mahalanobis (sui dati normalizzati):**
        * Per ogni periodo, viene calcolata la distanza di Mahalanobis di ciascuna opera d'arte dal centroide del proprio periodo, ma **su versioni normalizzate dei dati all'interno di questa fase**. Questo calcolo è funzionale a fornire la metrica 'Contextual_Fit', che è basata su dati scalati per essere interpretabile tra metriche diverse.
        * Vengono gestiti i casi di insufficienti punti dati o matrici di covarianza singolari, aggiungendo jitter per l'invertibilità.
        * Le distanze calcolate vengono aggiunte al DataFrame in una nuova colonna 'Mahalanobis_Distance_Normalized'.
    * **Calcolo del "Contesto di Adattamento" (Contextual Fit) (sui dati normalizzati):**
        * Viene calcolata la metrica di "Contesto di Adattamento" (inverso della Distanza di Mahalanobis) basata sulle distanze di Mahalanobis calcolate sui dati normalizzati.
        * Questa metrica viene aggiunta al DataFrame in una nuova colonna 'Contextual_Fit'.
    * **Calcolo Statistiche Riassuntive:** Vengono calcolate e stampate le medie e le deviazioni standard della Distanza di Mahalanobis normalizzata e del "Contesto di Adattamento" per ciascun periodo artistico.
    * **Generazione di Grafici KDE (sui dati NON normalizzati originali):**
        * **KDE Univariata (2x2 Subplot):** Viene generato un singolo grafico contenente quattro subplot, ognuno dei quali mostra la distribuzione di densità di probabilità (KDE) per una singola metrica (Dimensione Frattale, Entropia, Misura di Birkhoff, Distanza Euclidea) **utilizzando i dati originali non normalizzati** (`df_cleaned_for_kde`).
        * **KDE Bivariata (per Coppie di Metriche):** Vengono generati grafici KDE separati per ogni combinazione unica di due metriche. Questi grafici mostrano la densità congiunta delle due metriche **utilizzando i dati originali non normalizzati**, evidenziando le aree di maggiore concentrazione. Vengono aggiunti anche i singoli punti dati per fornire contesto.
        * Tutti i grafici vengono salvati come immagini PNG nella directory `.\\PLOT\\KDE PLOT\\` (diversa da quella dei dati normalizzati) per distinguere chiaramente i risultati.
* **Output:**
    * Il DataFrame arricchito con le colonne 'Mahalanobis_Distance_Normalized' e 'Contextual_Fit' (i cui valori sono derivati da metriche normalizzate internamente a questa fase).
    * Stampa delle medie e deviazioni standard delle distanze di Mahalanobis e del "Contesto di Adattamento" per periodo.
    * Un file immagine PNG (`.\\PLOT\\KDE PLOT\\all_univariate_kde_2x2_subplot.png`) che mostra le distribuzioni univariate di tutte le metriche **non normalizzate**.
    * Diversi file immagine PNG (`.\\PLOT\\KDE PLOT\\kde_bivariate_... .png`), ciascuno raffigurante la distribuzione di densità congiunta per una coppia di metriche **non normalizzate**.
* **Insights:** Il confronto tra i grafici KDE generati qui (su dati non normalizzati) e quelli della Sezione 12 (su dati normalizzati) è fondamentale. Si prevede che le distribuzioni KDE sui dati non normalizzati possano mostrare picchi e dispersioni molto diverse a causa delle scale intrinseche di ciascuna metrica, potenzialmente oscurando relazioni o creando l'illusione di separazioni dove non ve ne sono di significative dopo la normalizzazione. Questo passo rafforza l'importanza della normalizzazione per un'analisi comparativa equa e per la validità di tecniche come la Distanza di Mahalanobis, che dipendono dalle scale e correlazioni tra le variabili.

---

## Sezione 13: Misura di Hausdorff (Dati Normalizzati)

* **Scopo:** Calcolare la **Distanza di Hausdorff multidimensionale** tra periodi artistici consecutivi di Picasso, utilizzando le metriche visive **normalizzate**. Questa misura quantifica la "somiglianza" o "distanza massima" tra due insiemi di punti (periodi artistici) in uno spazio multidimensionale, offrendo un'indicazione della transizione e della distinzione tra stili.
* **Input:** Il file CSV contenente le metriche normalizzate per ogni opera d'arte (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Definizione della Funzione Hausdorff Multidimensionale:** Viene implementata una funzione `hausdorff_multidimensional(A, B)` che calcola la distanza di Hausdorff tra due insiemi di punti multidimensionali ($A$ e $B$).
        * La funzione utilizza la distanza euclidea per calcolare le distanze punto-a-punto tra gli insiemi.
        * Calcola le due distanze di Hausdorff dirette: $h(A, B)$ (la massima distanza di un punto in A al suo punto più vicino in B) e $h(B, A)$ (la massima distanza di un punto in B al suo punto più vicino in A).
        * La distanza di Hausdorff finale è il massimo di queste due distanze dirette, rappresentando la distanza "peggiore" tra i due insiemi.
        * Gestisce il caso di insiemi vuoti, ritornando $0.0$.
    * **Caricamento e Preparazione Dati:**
        * Il dataset delle metriche normalizzate viene caricato.
        * Viene definito un ordine sequenziale per i periodi artistici (`style_order`) per analizzare le transizioni consecutive.
        * Vengono selezionate le colonne delle metriche numeriche chiave: 'Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance'.
        * Viene eseguita una verifica della presenza delle colonne e una conversione esplicita a tipo numerico. Le righe con valori NaN nelle colonne delle metriche vengono rimosse per garantire la validità dei calcoli.
    * **Normalizzazione dei Dati:** I dati delle metriche vengono ulteriormente normalizzati su un intervallo $[0, 1]$ usando `MinMaxScaler`. Anche se il file di input è già "normalizzato", questa fase garantisce una coerenza e robustezza del range per i calcoli specifici della distanza di Hausdorff, specialmente se il precedente processo di normalizzazione ha utilizzato un metodo diverso o un intervallo differente.
    * **Calcolo della Distanza per Coppie Consecutive:**
        * Il codice itera attraverso le coppie di periodi consecutivi definiti in `style_order`.
        * Per ogni coppia, estrae i sottoinsiemi di dati corrispondenti alle opere di ciascun periodo.
        * Chiama la funzione `hausdorff_multidimensional` per calcolare la distanza tra i due sottoinsiemi.
        * I risultati vengono stampati e raccolti in un DataFrame.
* **Output:**
    * Una stampa tabellare che mostra la Distanza di Hausdorff multidimensionale calcolata per ogni transizione tra periodi artistici consecutivi (e.g., "Early Years" a "Blue Period", "Blue Period" a "Rose Period", ecc.).
    * Un file CSV (`.\\CSV NORMALIZED\\normalized_hausdorff4D.csv`) contenente i risultati di queste distanze.
* **Insights:** Questa sezione fornisce una quantificazione diretta della "separazione" tra i diversi periodi stilistici di Picasso nello spazio multidimensionale delle metriche visive normalizzate. Valori più alti della Distanza di Hausdorff indicheranno una maggiore discontinuità o una transizione più netta tra i due stili, suggerendo che le opere di quei periodi occupano regioni più distinte dello spazio delle caratteristiche. Al contrario, valori più bassi potrebbero suggerire una transizione più graduale o una maggiore sovrapposizione tra gli stili. Questi risultati sono cruciali per comprendere l'evoluzione stilistica di Picasso dal punto di vista computazionale.

---

## Sezione 13.1: Misura di Hausdorff (Dati Non Normalizzati)

* **Scopo:** Calcolare la **Distanza di Hausdorff multidimensionale** tra periodi artistici consecutivi di Picasso, utilizzando le metriche visive **non normalizzate**. Questa sezione funge da controparte alla Sezione 13, per investigare come la mancanza di normalizzazione influenzi la percezione delle distanze e delle transizioni tra gli stili.
* **Input:** Il file CSV contenente le metriche originali (non normalizzate) per ogni opera d'arte (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Definizione della Funzione Hausdorff Multidimensionale:** Viene utilizzata la stessa funzione `hausdorff_multidimensional(A, B)` della Sezione 13, che calcola la distanza di Hausdorff tra due insiemi di punti multidimensionali. La funzione si basa sulla distanza euclidea e considera le due distanze dirette massime tra gli insiemi.
    * **Caricamento e Preparazione Dati:**
        * Il dataset originale (non normalizzato) viene caricato.
        * Viene definito l'ordine sequenziale per i periodi artistici (`style_order`).
        * Vengono selezionate le colonne delle metriche numeriche chiave: 'Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance'.
        * Viene eseguita una verifica della presenza delle colonne e una conversione esplicita a tipo numerico. Le righe con valori NaN nelle colonne delle metriche vengono rimosse.
    * **Normalizzazione dei Dati (Importante):** Nonostante l'obiettivo sia analizzare l'impatto dei dati *non normalizzati*, la funzione `hausdorff_multidimensional` (che usa `cdist` con 'euclidean' come metrica) è sensibile alla scala. Pertanto, **all'interno di questa sezione**, i dati delle metriche vengono comunque normalizzati su un intervallo $[0, 1]$ usando `MinMaxScaler` *prima* di calcolare le distanze. Questo è un compromesso tecnico: sebbene si parta da dati "non normalizzati" nel file CSV, il calcolo della distanza di Hausdorff (che è una distanza metrica come la euclidea) spesso richiede dati scalati per evitare che una singola metrica con un range di valori molto ampio domini la distanza complessiva. Il punto cruciale è che il *file di input* non è stato pre-normalizzato come quello della Sezione 13, ma la normalizzazione viene applicata *qui* per il calcolo della Hausdorff.
    * **Calcolo della Distanza per Coppie Consecutive:**
        * Il codice itera attraverso le coppie di periodi consecutivi definiti in `style_order`.
        * Per ogni coppia, estrae i sottoinsiemi di dati corrispondenti alle opere di ciascun periodo.
        * Chiama la funzione `hausdorff_multidimensional` per calcolare la distanza tra i due sottoinsetti di dati **normalizzati in questo passaggio**.
        * I risultati vengono stampati e raccolti in un DataFrame.
* **Output:**
    * Una stampa tabellare che mostra la Distanza di Hausdorff multidimensionale calcolata per ogni transizione tra periodi artistici consecutivi.
    * Un file CSV (`.\\CSV\\hausdorff4D.csv`) contenente i risultati di queste distanze.
* **Insights:** Il confronto tra i risultati della Distanza di Hausdorff calcolati qui (con normalizzazione applicata *localmente* prima del calcolo) e quelli della Sezione 13 (dove i dati di input erano già normalizzati) è sottile ma importante. Entrambe le sezioni utilizzano dati normalizzati per il calcolo effettivo della distanza di Hausdorff. La principale differenza risiede nel punto di origine della normalizzazione e nella dicitura "non normalizzati" che si riferisce al file CSV di input. Se i risultati sono molto simili, ciò indicherebbe la robustezza del metodo di normalizzazione utilizzato. Tuttavia, la Sezione 13.1, pur normalizzando, si basa su un file CSV che non ha avuto una pre-normalizzazione "globale" come il file utilizzato in Sezione 13. Ciò potrebbe portare a leggere differenze se il processo di pulizia o normalizzazione precedente non è stato identico. L'analisi di questa sezione mira a garantire che, anche partendo da dati grezzi, l'applicazione della normalizzazione prima del calcolo della distanza di Hausdorff sia un passo necessario per ottenere misure significative della separazione stilistica.

---

## Sezione 14: Contesto di Adattamento (Dati Normalizzati)

* **Scopo:** Calcolare il "Contesto di Adattamento" (`Contextual Fit`) per ciascuna opera d'arte, basandosi sulla Distanza di Mahalanobis calcolata all'interno del proprio periodo stilistico, utilizzando i dati delle metriche visive **normalizzate**. Questa metrica fornisce un'indicazione di quanto bene un'opera si "adatta" alle caratteristiche statistiche del suo stile di appartenenza, con valori più alti che indicano un migliore adattamento. Successivamente, verranno calcolate le statistiche riassuntive (media e deviazione standard) di queste misure per ciascun periodo.
* **Input:** Il file CSV contenente le metriche normalizzate per ogni opera d'arte (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset delle metriche normalizzate viene caricato, prestando attenzione al separatore decimale (virgola).
    * **Definizione delle Caratteristiche Numeriche:** Vengono identificate le quattro metriche chiave ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance'). Viene eseguita una robusta validazione delle colonne, convertendole in numerico e rimuovendo le righe con valori NaN in queste colonne per garantire l'integrità dei calcoli.
    * **Calcolo della Distanza di Mahalanobis:**
        * Il DataFrame viene raggruppato per la colonna 'Period'.
        * Per ogni periodo:
            * Viene calcolato il centroide (media) delle metriche per quel periodo.
            * Viene calcolata la matrice di covarianza delle metriche.
            * Viene eseguito un controllo per la singolarità della matrice di covarianza (determinante zero) e, se necessario, viene aggiunto un piccolo termine diagonale (jitter) per renderla invertibile.
            * Viene calcolata la matrice di covarianza inversa.
            * Per ogni opera d'arte all'interno del periodo, viene calcolata la distanza di Mahalanobis tra l'opera stessa e il centroide del suo periodo, utilizzando la matrice di covarianza inversa specifica del periodo.
            * Le distanze calcolate vengono mappate all'indice originale del DataFrame nella nuova colonna 'Mahalanobis_Distance'.
    * **Calcolo del "Contesto di Adattamento" (`Contextual_Fit`):**
        * Viene creata una nuova colonna 'Contextual_Fit'.
        * Per ogni opera, il "Contesto di Adattamento" viene calcolato come l'inverso della sua 'Mahalanobis_Distance' (cioè, $1 / (\text{Mahalanobis_Distance})$). Un piccolo valore epsilon viene implicitamente considerato o gestito per prevenire divisioni per zero o valori infiniti per distanze vicine allo zero (anche se il codice gestisce esplicitamente il caso di distanza 0 impostando `np.inf`). Valori NaN nella distanza di Mahalanobis risultano in NaN anche per il Contesto di Adattamento.
    * **Salvataggio dei Dati Intermedi:** Il DataFrame completo con le nuove colonne 'Mahalanobis_Distance' e 'Contextual_Fit' viene salvato in un nuovo file CSV (`.\\CSV NORMALIZED\\normalized_contextual_fit.csv`).
    * **Calcolo e Salvataggio delle Statistiche Riassuntive:**
        * Vengono calcolate la media e la deviazione standard della 'Mahalanobis_Distance' e del 'Contextual_Fit' per ogni 'Period'.
        * Le colonne del DataFrame delle statistiche vengono appiattite per una migliore leggibilità.
        * Queste statistiche aggregate vengono salvate in un altro file CSV (`.\\CSV NORMALIZED\\normalized_mahalanobis_contextual_fit.csv`).
* **Output:**
    * Il DataFrame originale esteso con le colonne 'Mahalanobis_Distance' e 'Contextual_Fit', visualizzato le prime 10 righe.
    * Un file CSV (`.\\CSV NORMALIZED\\normalized_contextual_fit.csv`) contenente l'intero DataFrame aggiornato con le nuove metriche per ogni opera.
    * Una stampa tabellare delle medie e deviazioni standard di 'Mahalanobis_Distance' e 'Contextual_Fit' per ciascun periodo.
    * Un file CSV (`.\\CSV NORMALIZED\\normalized_mahalanobis_contextual_fit.csv`) contenente queste statistiche aggregate per periodo.
* **Insights:** Questa sezione fornisce una quantificazione cruciale dell'omogeneità stilistica all'interno di ciascun periodo. Un valore elevato di "Contesto di Adattamento" per un'opera indica che essa è molto rappresentativa del suo periodo. Analizzando le medie e le deviazioni standard di queste metriche per ogni periodo, è possibile identificare quali periodi sono più "compatti" (opere molto simili tra loro) e quali sono più "diversi" o "fluidi" (opere più disperse attorno al centroide), fornendo indicazioni sulla rigidità delle convenzioni stilistiche in diversi momenti della carriera di Picasso. Questi dati sono fondamentali per la discussione sull'evoluzione stilistica e la coerenza dei periodi.

---

## Sezione 14.1: Contesto di Adattamento (Dati Non Normalizzati)

* **Scopo:** Calcolare il "Contesto di Adattamento" (`Contextual Fit`) per ciascuna opera d'arte, basandosi sulla Distanza di Mahalanobis calcolata all'interno del proprio periodo stilistico, utilizzando i dati delle metriche visive **non normalizzate**. Questa sezione confronta i risultati con la Sezione 14, evidenziando l'impatto della normalizzazione sulla Distanza di Mahalanobis e, di conseguenza, sul "Contesto di Adattamento".
* **Input:** Il file CSV contenente le metriche originali (non normalizzate) per ogni opera d'arte (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset delle metriche originali viene caricato, prestando attenzione al separatore decimale.
    * **Definizione delle Caratteristiche Numeriche:** Vengono identificate le quattro metriche chiave. Viene eseguita una robusta validazione delle colonne, convertendole in numerico e rimuovendo le righe con valori NaN.
    * **Calcolo della Distanza di Mahalanobis:**
        * Il DataFrame viene raggruppato per la colonna 'Period'.
        * Per ogni periodo:
            * Viene calcolato il centroide (media) delle metriche.
            * Viene calcolata la matrice di covarianza delle metriche.
            * Viene gestita la singolarità della matrice di covarianza aggiungendo un piccolo termine diagonale (jitter) per renderla invertibile.
            * Viene calcolata la matrice di covarianza inversa.
            * Per ogni opera d'arte all'interno del periodo, viene calcolata la distanza di Mahalanobis tra l'opera stessa e il centroide del suo periodo, utilizzando la matrice di covarianza inversa specifica del periodo. **È cruciale notare che qui la Distanza di Mahalanobis viene calcolata direttamente sui valori delle metriche originali (non normalizzate) caricate dal CSV.** Questo è il punto di divergenza principale rispetto alla Sezione 14.
            * Le distanze calcolate vengono mappate all'indice originale del DataFrame nella nuova colonna 'Mahalanobis_Distance'.
    * **Calcolo del "Contesto di Adattamento" (`Contextual_Fit`):**
        * Viene creata una nuova colonna 'Contextual_Fit'.
        * Per ogni opera, il "Contesto di Adattamento" viene calcolato come l'inverso della sua 'Mahalanobis_Distance'.
    * **Salvataggio dei Dati Intermedi:** Il DataFrame completo con le nuove colonne 'Mahalanobis_Distance' e 'Contextual_Fit' viene salvato in un nuovo file CSV (`.\\CSV\\contextual_fit.csv`).
    * **Calcolo e Salvataggio delle Statistiche Riassuntive:**
        * Vengono calcolate la media e la deviazione standard della 'Mahalanobis_Distance' e del 'Contextual_Fit' per ogni 'Period'.
        * Le colonne del DataFrame delle statistiche vengono appiattite.
        * Queste statistiche aggregate vengono salvate in un altro file CSV (`.\\CSV\\mahalanobis_contextual_fit.csv`).
* **Output:**
    * Il DataFrame originale esteso con le colonne 'Mahalanobis_Distance' e 'Contextual_Fit', visualizzato le prime 10 righe.
    * Un file CSV (`.\\CSV\\contextual_fit.csv`) contenente l'intero DataFrame aggiornato con le nuove metriche per ogni opera.
    * Una stampa tabellare delle medie e deviazioni standard di 'Mahalanobis_Distance' e 'Contextual_Fit' per ciascun periodo.
    * Un file CSV (`.\\CSV\\mahalanobis_contextual_fit.csv`) contenente queste statistiche aggregate per periodo.
* **Insights:** Questa sezione è fondamentale per dimostrare l'impatto della normalizzazione sulla Distanza di Mahalanobis e sulla metrica del "Contesto di Adattamento". Senza normalizzazione, le metriche con range di valori più ampi (ad esempio, 'Euclidean distance' se i suoi valori sono molto più grandi degli altri) tenderanno a dominare il calcolo della distanza, potenzialmente mascherando i contributi delle altre metriche e portando a un "Contesto di Adattamento" meno significativo. Ci si aspetta che i valori di 'Mahalanobis_Distance' e 'Contextual_Fit' siano molto diversi rispetto alla Sezione 14, e che l'interpretazione dei periodi come più o meno "compatti" possa cambiare drasticamente. Questo sottolinea l'importanza della normalizzazione come passo di pre-elaborazione critico per l'analisi multivariata.

---

## Sezione 15: Mappe di Biforcazione (Dati Normalizzati)

* **Scopo:** Visualizzare la distribuzione della "Dimensione Frattale" delle opere di Picasso in relazione ai suoi periodi artistici e agli anni di creazione, utilizzando i dati delle metriche visive **normalizzate**. Queste "Mappe di Biforcazione" aiutano a identificare le transizioni stilistiche e la variabilità della dimensione frattale all'interno e tra i periodi.
* **Input:** Il file CSV contenente le metriche normalizzate per ogni opera d'arte (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset delle metriche normalizzate viene caricato. Viene gestito il caso di `FileNotFoundError`.
    * **Definizione e Ordine dei Periodi:** Viene definito l'ordine cronologico dei periodi artistici di Picasso (`ordered_styles`). La colonna 'Period' del DataFrame viene convertita in un tipo categorico con questo ordine per garantire che i grafici siano visualizzati correttamente in sequenza temporale.
    * **Generazione di Stripplot (Mappe di Biforcazione):** Per ciascun grafico, viene utilizzato un `sns.stripplot`. Uno stripplot è una buona scelta per visualizzare la distribuzione di punti dati unidimensionali per diverse categorie, permettendo di vedere la densità e i singoli punti. Il `jitter` aggiunge un leggero spostamento casuale ai punti per evitare sovrapposizioni e mostrare la densità dei dati.
        * **"Fractal Dimension" vs. "Artistic Period" (Orizzontale):**
            * Asse Y: 'Periodo Artistico' (con l'ordine definito).
            * Asse X: 'Dimensione Frattale'.
            * Orientamento: Orizzontale (`orient='h'`).
            * Il grafico visualizza la dispersione della dimensione frattale per ogni periodo.
            * Salvato come `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_h.png`.
        * **"Fractal Dimension" vs. "Artistic Period" (Verticale):**
            * Asse X: 'Periodo Artistico' (con l'ordine definito).
            * Asse Y: 'Dimensione Frattale'.
            * Orientamento: Verticale.
            * Etichette dell'asse X ruotate per leggibilità.
            * Salvato come `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_v.png`.
        * **"Fractal Dimension" vs. "Year" (Orizzontale):**
            * Asse Y: 'Anno di creazione'.
            * Asse X: 'Dimensione Frattale'.
            * Orientamento: Orizzontale (`orient='h'`).
            * Il grafico mostra l'evoluzione della dimensione frattale nel tempo.
            * Salvato come `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_year_horizontal.png`.
        * **"Fractal Dimension" vs. "Year" (Verticale):**
            * Asse X: 'Anno di creazione'.
            * Asse Y: 'Dimensione Frattale'.
            * Orientamento: Verticale (`orient='v'`).
            * Etichette dell'asse X ruotate per leggibilità.
            * Salvato come `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_year_vertical.png`.
    * **Salvataggio dei Grafici:** Tutti i grafici vengono salvati in un'apposita sottodirectory `.\\PLOT NORMALIZED\\BIFURCATION MAP\\`.
* **Output:** Quattro file immagine PNG, ciascuno raffigurante una mappa di biforcazione della dimensione frattale in relazione ai periodi artistici o agli anni di creazione, in orientamenti orizzontali e verticali.
* **Insights:** Le mappe di biforcazione offrono una visualizzazione dettagliata e intuitiva delle distribuzioni dei valori della dimensione frattale. Consentono di osservare:
    * **Coesione/Variabilità all'interno dei Periodi:** Se i punti per un dato periodo sono raggruppati strettamente, indica una maggiore coerenza stilistica per quella metrica. Se sono molto sparsi, suggerisce una maggiore variabilità.
    * **Transizioni tra i Periodi:** Come cambiano i range e le densità dei punti tra periodi consecutivi. Questo può evidenziare "biforcazioni" o cambiamenti improvvisi nella distribuzione della dimensione frattale, suggerendo passaggi stilistici distinti.
    * **Tendenze Temporali:** I grafici per anno possono rivelare un'evoluzione graduale o brusca della dimensione frattale nel corso della carriera di Picasso, potenzialmente correlata all'introduzione di nuovi stili o tecniche.
    * Poiché i dati sono normalizzati, la comparazione tra i periodi è più equa e non è influenzata dalle differenze di scala originali delle metriche.

---

## Sezione 15.1: Mappe di Biforcazione (Dati Non Normalizzati)

* **Scopo:** Visualizzare la distribuzione della "Dimensione Frattale" delle opere di Picasso in relazione ai suoi periodi artistici e agli anni di creazione, utilizzando i dati delle metriche visive **non normalizzate**. Questa sezione replica l'analisi della Sezione 15 ma con i dati originali, permettendo un confronto diretto sull'impatto della normalizzazione sulle visualizzazioni.
* **Input:** Il file CSV contenente le metriche originali (non normalizzate) per ogni opera d'arte (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Metodi:**
    * **Caricamento Dati:** Il dataset delle metriche originali viene caricato. Viene gestita l'assenza del file.
    * **Definizione e Ordine dei Periodi:** Viene definito l'ordine cronologico dei periodi artistici (`ordered_styles`). La colonna 'Period' del DataFrame viene convertita in un tipo categorico con questo ordine per garantire una visualizzazione cronologica.
    * **Generazione di Stripplot (Mappe di Biforcazione):** Per ciascun grafico, viene utilizzato un `sns.stripplot`, che è ideale per mostrare la distribuzione di punti dati unidimensionali per diverse categorie, rivelando la densità e la posizione dei singoli punti. Il `jitter` viene applicato per sparpagliare i punti sovrapposti.
        * **"Fractal Dimension" vs. "Artistic Period" (Orizzontale):**
            * Asse Y: 'Periodo Artistico'.
            * Asse X: 'Dimensione Frattale'.
            * Orientamento: Orizzontale (`orient='h'`).
            * Salvataggio: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_h.png`.
        * **"Fractal Dimension" vs. "Artistic Period" (Verticale):**
            * Asse X: 'Periodo Artistico'.
            * Asse Y: 'Dimensione Frattale'.
            * Orientamento: Verticale.
            * Le etichette dell'asse X sono ruotate per una migliore leggibilità.
            * Salvataggio: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_v.png`.
        * **"Fractal Dimension" vs. "Year" (Orizzontale):**
            * Asse Y: 'Anno di creazione'.
            * Asse X: 'Dimensione Frattale'.
            * Orientamento: Orizzontale (`orient='h'`).
            * Salvataggio: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_year_horizontal.png`.
        * **"Fractal Dimension" vs. "Year" (Verticale):**
            * Asse X: 'Anno di creazione'.
            * Asse Y: 'Dimensione Frattale'.
            * Orientamento: Verticale (`orient='v'`).
            * Le etichette dell'asse X sono ruotate per leggibilità.
            * Salvataggio: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_year_vertical.png`.
    * **Salvataggio dei Grafici:** Tutti i grafici vengono salvati in una sottodirectory dedicata `.\\PLOT\\BIFURCATION MAP\\`, distinta da quella usata per i dati normalizzati.
* **Output:** Quattro file immagine PNG, ciascuno rappresentante una mappa di biforcazione della dimensione frattale in rapporto ai periodi artistici o agli anni di creazione, in orientamenti orizzontali e verticali, basati su **dati non normalizzati**.
* **Insights:** Il confronto visivo tra le mappe di biforcazione di questa sezione (dati non normalizzati) e quelle della Sezione 15 (dati normalizzati) è cruciale. Si prevede che i grafici sui dati non normalizzati possano mostrare intervalli di valori molto diversi, potenzialmente influenzati dalla scala intrinseca della dimensione frattale e non dalla sua variazione relativa. Questo confronto diretto aiuterà a capire come la normalizzazione incida sulla percezione della variabilità e delle transizioni tra i periodi, e a rafforzare l'argomentazione a favore dell'uso di dati normalizzati per analisi comparative significative tra metriche.

---

## Sezione 16: Rilevamento degli Attrattori

* **Scopo:** Questa sezione mira a identificare i "cluster" o "attrattori" nello spazio multidimensionale delle metriche visive di Picasso (Dimensione Frattale, Entropia, Misura di Birkhoff, Distanza Euclidea). Questi attrattori rappresentano configurazioni morfologiche e strutturali ricorrenti o prevalenti nelle sue opere, indipendentemente dal periodo stilistico formalmente assegnato. L'approccio si basa sull'algoritmo di clustering **Mean-Shift**, seguito da visualizzazioni tramite PCA, t-SNE e UMAP per esplorare la struttura dei dati e infine un'analisi delle distribuzioni delle metriche per cluster.
* **Input:** Il file CSV `normalized_metrics.csv` contenente le metriche visive normalizzate per ciascuna opera d'arte.
* **Metodi:**
    * **Caricamento e Preparazione Dati:**
        * Il dataset viene caricato.
        * Le colonne delle metriche ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance") vengono pulite da eventuali valori non numerici e convertite in tipo `float`.
        * Viene creato un array `X` contenente solo i valori delle quattro metriche per il clustering.
    * **Clustering Mean-Shift:**
        * Viene stimata una **bandwidth adattiva** per l'algoritmo Mean-Shift. La bandwidth è un parametro cruciale che determina la scala di rilevamento dei cluster. Un valore di `0.39 * median_bw` viene utilizzato come specificato, suggerendo un approccio leggermente più permissivo nella definizione dei cluster rispetto a una bandwidth mediana pura.
        * L'algoritmo Mean-Shift viene applicato ai dati `X`. Questo algoritmo non richiede il numero predefinito di cluster e li identifica autonomamente basandosi sulla densità dei punti dati.
        * Le etichette dei cluster (`labels`) assegnate a ciascun punto dati vengono aggiunte al DataFrame originale in una nuova colonna 'Cluster'.
        * Viene stampato il numero di cluster trovati.
    * **Analisi e Visualizzazione dei Cluster:**
        * **Riduzione della Dimensionalità (PCA):** Viene applicata la Principal Component Analysis (PCA) per ridurre le quattro dimensioni delle metriche a due componenti principali (`X_pca`), facilitando la visualizzazione.
        * **Scatter Plot PCA:** Viene generato uno scatter plot dei dati proiettati su PCA 1 e PCA 2, con i punti colorati in base al cluster assegnato da Mean-Shift. Questo grafico aiuta a visualizzare la separazione dei cluster nello spazio delle componenti principali.
        * **Statistiche per Cluster:** Vengono calcolate e stampate le medie delle quattro metriche per ciascun cluster identificato. Questo fornisce una caratterizzazione numerica di ogni attrattore.
        * **Bar Plot delle Medie per Metrica e Cluster:** Vengono generati bar plot separati per ciascuna delle quattro metriche, mostrando il valore medio di quella metrica per ogni cluster. Questo aiuta a comprendere quali metriche definiscono meglio ciascun cluster.
        * **Bar Plot delle Medie Scalate per Metrica e Cluster:** Le medie delle metriche per ogni cluster vengono ulteriormente scalate (normalizzate tra 0 e 1) utilizzando `MinMaxScaler`. Questo permette un confronto diretto dell'importanza relativa di ciascuna metrica all'interno di un cluster, visualizzato in un unico bar plot raggruppato per cluster e metrica.
        * **Riduzione della Dimensionalità (t-SNE):** Viene applicata t-Distributed Stochastic Neighbor Embedding (t-SNE) per ridurre i dati a due dimensioni (`X_tsne`). t-SNE è particolarmente efficace nel preservare le relazioni di prossimità locali e nel visualizzare la struttura intrinseca dei dati in spazi ad alta dimensionalità.
        * **Scatter Plot t-SNE:** Viene creato uno scatter plot dei dati proiettati da t-SNE, colorato per cluster. Questo grafico offre una visualizzazione alternativa alla PCA, spesso rivelando raggruppamenti più distinti se i cluster hanno forme complesse.
        * **Riduzione della Dimensionalità (UMAP):** Viene applicata Uniform Manifold Approximation and Projection (UMAP) per ridurre i dati a due dimensioni (`embedding`). UMAP è simile a t-SNE ma spesso più veloce e più adatta a dataset di grandi dimensioni, cercando di preservare sia la struttura locale che globale.
        * **Scatter Plot UMAP:** Viene generato uno scatter plot dei dati proiettati da UMAP, colorato per cluster. Questo grafico fornisce un'ulteriore prospettiva sulla disposizione dei cluster nello spazio delle caratteristiche.
        * **Mappa di Biforcazione "Fractal Dimension" per Cluster:** Viene generato uno stripplot che visualizza la distribuzione dei valori della "Dimensione Frattale" per ogni cluster. Questo grafico agisce come una "mappa di biforcazione", mostrando la dispersione della metrica all'interno di ogni attrattore e le transizioni tra essi.
* **Output:**
    * Il numero di cluster trovati dall'algoritmo Mean-Shift.
    * Stampe delle statistiche medie per ogni metrica all'interno di ciascun cluster.
    * Grafici:
        * Scatter plot dei cluster proiettati su PCA.
        * Quattro bar plot separati, uno per ogni metrica, mostrando i valori medi per cluster.
        * Un bar plot unificato delle medie delle metriche per cluster (con metriche scalate).
        * Scatter plot dei cluster proiettati su t-SNE.
        * Scatter plot dei cluster proiettati su UMAP.
        * Un grafico tipo "biforcazione" della Dimensione Frattale per cluster.
* **Insights:** Questa analisi permette di identificare e caratterizzare gli "attrattori" nello stile di Picasso basati su metriche oggettive. Questi attrattori possono corrispondere o meno ai periodi stilistici tradizionali, offrendo una prospettiva data-driven sull'evoluzione del suo linguaggio visivo. I cluster rappresentano stati stabili o configurazioni morfologiche che l'artista ha esplorato. Le visualizzazioni PCA, t-SNE e UMAP aiutano a capire la separazione e la struttura di questi attrattori nello spazio delle caratteristiche, mentre l'analisi delle medie delle metriche per cluster quantifica le caratteristiche distintive di ciascun attrattore. La mappa di biforcazione della dimensione frattale (o di altre metriche) per cluster può mostrare come una metrica specifica si distribuisce all'interno di ciascun attrattore e se ci sono salti o continuità tra di essi, suggerendo punti di cambiamento o di consolidamento nello stile.

---

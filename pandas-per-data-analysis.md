
# Pandas per Data Analysis

## Guida pratica con casi reali (ferroviario e mobilità urbana)

---

## 1. Funzionalità di base essenziali

### 1.1 Introduzione a Pandas nel contesto reale

Pandas è una libreria Python progettata per lavorare con dati strutturati.
Nel contesto dell’ingegneria ferroviaria e della mobilità urbana, questi dati possono includere:

* ritardi dei treni
* log dei sensori
* traffico urbano
* flussi di passeggeri
* eventi operativi (guasti, manutenzione)

Il punto chiave:
Pandas non è solo uno strumento tecnico, ma un **mezzo per trasformare dati grezzi in informazioni utili per decisioni operative**.

---

### 1.2 Strutture fondamentali: Series e DataFrame

#### Series

Una Series è una struttura monodimensionale, simile a una colonna.

```python
import pandas as pd

# Serie di ritardi (in minuti)
ritardi = pd.Series([5, 10, 0, 3, 7])

print(ritardi)
```

Uso reale:

* analisi di una singola variabile
* output di calcoli (es. media per linea)

---

#### DataFrame

Il DataFrame è una tabella bidimensionale.
È la struttura più utilizzata nel lavoro di un Data Analyst.

```python
# Dataset ferroviario
data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE"],
    "ritardo_min": [5, 10, 0, 3, 7],
    "stato": ["OK", "Ritardo", "OK", "OK", "Ritardo"]
}

df = pd.DataFrame(data)

print(df)
```

---

### 1.3 Struttura interna del DataFrame

Ogni DataFrame è composto da:

* righe → osservazioni (es. corsa treno)
* colonne → variabili (es. ritardo, linea)
* index → identificatore univoco

```python
print(df.index)
print(df.columns)
```

Insight:
L’index è spesso sottovalutato, ma diventa critico in:

* time series
* merge
* join complessi

---

### 1.4 Ispezione iniziale dei dati

Prima di qualsiasi analisi, è obbligatorio capire il dataset.

#### head()

```python
# Prime righe del dataset
print(df.head())
```

Serve per:

* validare la struttura
* individuare errori evidenti

---

#### info()

```python
# Informazioni strutturali
print(df.info())
```

Output chiave:

* tipi di dato
* valori nulli
* dimensione dataset

---

#### describe()

```python
# Statistiche descrittive
print(df.describe())
```

Utile per:

* capire distribuzione
* identificare outlier

---

### 1.5 Caso reale: diagnosi iniziale dataset ferroviario

Simuliamo un dataset più realistico:

```python
data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE", "MI-VE"],
    "ritardo_min": [5, None, 0, 3, 7, 50],
    "stato": ["OK", "Ritardo", "OK", "OK", "Ritardo", "Ritardo"]
}

df = pd.DataFrame(data)
```

Analisi:

```python
print(df.info())
print(df.describe())
```

Interpretazione:

* presenza di valori nulli → problema di qualità dati
* valore 50 → possibile outlier
* distribuzione non uniforme

Questo è esattamente ciò che succede nei dati reali.

---

### 1.6 Caso mobilità urbana: traffico autobus

```python
data_bus = {
    "linea_bus": ["90", "91", "92", "90", "92"],
    "ritardo_min": [2, 5, 0, 3, 8],
    "ora": ["08:00", "08:05", "08:10", "08:15", "08:20"]
}

df_bus = pd.DataFrame(data_bus)

print(df_bus.info())
```

Problema reale:

* "ora" è stringa → non utilizzabile per analisi temporale

---

### 1.7 Errori tipici (molto comuni)

1. Non controllare i tipi di dato
2. Ignorare valori nulli
3. Fidarsi del dataset senza validazione
4. Non analizzare distribuzione iniziale

---

### 1.8 Best practice operative

* sempre eseguire `.info()` prima di tutto
* usare `.describe()` per validare i numeri
* controllare valori anomali
* verificare tipi di dato coerenti

---

### 1.9 Insight da Data Analyst

Questa fase non è "preparazione".
È già analisi.

Domande che devi porti subito:

* quali linee sono più critiche?
* ci sono dati mancanti sistematici?
* ci sono anomalie operative?

Un Data Analyst efficace identifica problemi **prima ancora di fare calcoli complessi**.

---

### 1.10 Sintesi

* Il DataFrame è la struttura centrale
* L’ispezione iniziale è obbligatoria
* I problemi di qualità dati emergono subito
* L’analisi inizia dal primo contatto con il dataset

---

## 2. Esplorazione dei dati

### 2.1 Ruolo dell’EDA nel processo analitico

L’Exploratory Data Analysis (EDA) è la fase in cui il Data Analyst comprende la natura del dataset prima di qualsiasi trasformazione o modellazione.

Nel contesto ferroviario e della mobilità urbana, l’EDA serve per:

* identificare problemi operativi (ritardi, anomalie)
* validare la qualità dei dati
* comprendere distribuzioni e pattern
* individuare variabili rilevanti

Errore comune: considerare l’EDA come una fase “veloce”.
In realtà, è la fase che determina la qualità dell’intera analisi.

---

### 2.2 Dataset di riferimento (caso ferroviario)

```python
import pandas as pd

# Dataset simulato: monitoraggio ritardi treni
data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE", "MI-VE", "MI-GE"],
    "ritardo_min": [5, 10, 0, 3, 7, 50, None],
    "stato": ["OK", "Ritardo", "OK", "OK", "Ritardo", "Ritardo", "OK"],
    "treni_giornalieri": [50, 50, 40, 40, 30, 30, 20]
}

df = pd.DataFrame(data)
```

---

### 2.3 Analisi strutturale

```python
# Informazioni generali
df.info()
```

Interpretazione:

* presenza di valori nulli in `ritardo_min`
* tipi di dato misti (numerico + categorico)
* dimensione dataset limitata (simulazione)

Insight operativo:
Un valore nullo in ritardo può significare:

* dato mancante
* errore di sensore
* treno non rilevato

---

### 2.4 Analisi delle prime osservazioni

```python
df.head()
```

Cosa osservare:

* coerenza tra colonne
* valori anomali evidenti
* struttura del dataset

---

### 2.5 Statistiche descrittive

```python
df.describe()
```

Interpretazione:

* media ritardo influenzata da valore 50 → possibile outlier
* distribuzione non simmetrica
* presenza di valori estremi

Insight:
Un singolo evento può distorcere la percezione operativa.

---

### 2.6 Analisi delle variabili categoriche

```python
df["stato"].value_counts()
```

Interpretazione:

* frequenza di ritardi vs operatività normale
* possibile KPI: percentuale treni in ritardo

---

### 2.7 Individuazione outlier

```python
# Identificare ritardi anomali
outliers = df[df["ritardo_min"] > 30]
print(outliers)
```

Caso reale:

* ritardi elevati possono indicare guasti o eventi critici

---

### 2.8 Analisi dei valori nulli

```python
df.isnull().sum()
```

Interpretazione:

* capire se i null sono casuali o sistematici
* valutare impatto sull’analisi

Edge case:

* molti null → dataset inutilizzabile
* null concentrati su una linea → problema operativo specifico

---

### 2.9 Caso mobilità urbana: traffico autobus

```python
data_bus = {
    "linea_bus": ["90", "91", "92", "90", "92", "91"],
    "ritardo_min": [2, 5, 0, 3, 8, None],
    "ora": ["08:00", "08:05", "08:10", "08:15", "08:20", "08:25"]
}

df_bus = pd.DataFrame(data_bus)
```

Problema:

```python
df_bus.info()
```

* `ora` è stringa → non utilizzabile per analisi temporale

Insight:
Molti dataset reali hanno problemi di tipo dati.

---

### 2.10 Distribuzione dei dati

```python
df["ritardo_min"].hist()
```

Interpretazione:

* distribuzione asimmetrica
* presenza di coda lunga (ritardi elevati)

---

### 2.11 Relazioni tra variabili

```python
df.groupby("linea")["ritardo_min"].mean()
```

Insight:

* alcune linee più critiche di altre
* possibile priorità operativa

---

### 2.12 Errori tipici nell’EDA

* limitarsi a `.head()` senza approfondire
* ignorare outlier
* non analizzare distribuzione
* non distinguere tra dato mancante e dato zero

---

### 2.13 Best practice

* combinare analisi strutturale + statistica
* verificare sempre outlier
* analizzare variabili categoriche
* contestualizzare i dati (non solo numeri)

---

### 2.14 Insight da Data Analyst

Un buon EDA risponde già a domande chiave:

* dove si concentrano i ritardi?
* quali linee sono più instabili?
* esistono pattern operativi?

L’obiettivo non è “esplorare i dati”, ma **capire il sistema reale dietro i dati**.

---

### 2.15 Sintesi

* L’EDA è una fase critica, non opzionale
* I dati reali sono imperfetti
* Outlier e nulli sono la norma
* Le prime intuizioni emergono qui

---

## 3. Indexing e selezione dei dati

### 3.1 Perché questa sezione è critica

L’indexing è il cuore operativo di Pandas.
Se non lo padroneggi, non riesci a:

* filtrare dati correttamente
* costruire KPI affidabili
* evitare errori silenziosi (i più pericolosi)

Nel contesto reale (ferroviario o mobilità urbana), errori di selezione possono portare a:

* analisi sbagliate sui ritardi
* KPI non affidabili
* decisioni operative errate

---

### 3.2 Dataset di riferimento

```python
import pandas as pd

data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE", "MI-VE"],
    "ritardo_min": [5, 10, 0, 3, 7, 50],
    "stato": ["OK", "Ritardo", "OK", "OK", "Ritardo", "Ritardo"],
    "treno_id": [101, 102, 201, 202, 301, 302]
}

df = pd.DataFrame(data)
```

---

### 3.3 Selezione base di colonne

```python
# Selezione di una singola colonna
df["ritardo_min"]

# Selezione di più colonne
df[["linea", "ritardo_min"]]
```

Uso reale:

* costruzione di dataset ridotti per analisi specifiche
* preparazione dati per dashboard

---

### 3.4 Filtri condizionali (core skill)

```python
# Treni in ritardo
df[df["ritardo_min"] > 5]
```

Caso reale:
identificare treni critici

---

#### Filtri multipli

```python
df[(df["ritardo_min"] > 5) & (df["linea"] == "MI-VE")]
```

Nota tecnica:

* usare `&` invece di `and`
* ogni condizione deve essere tra parentesi

---

### 3.5 .loc → selezione per etichetta

`.loc` è il metodo principale per selezioni esplicite.

```python
# Selezione righe + colonne
df.loc[df["ritardo_min"] > 5, ["linea", "ritardo_min"]]
```

Uso reale:

* selezioni leggibili e controllate
* standard in ambiente professionale

---

### 3.6 .iloc → selezione per posizione

```python
# Prime due righe, prime due colonne
df.iloc[0:2, 0:2]
```

Uso reale:

* operazioni tecniche
* slicing veloce

---

### 3.7 Differenza critica: .loc vs .iloc

| Metodo | Tipo selezione |
| ------ | -------------- |
| .loc   | etichette      |
| .iloc  | posizione      |

Errore comune:

```python
# Questo può generare errori logici
df.loc[0]
```

Se l’index non è numerico sequenziale → risultato inatteso

---

### 3.8 Caso reale: errore pericoloso

```python
df_filtered = df[df["ritardo_min"] > 5]

# Accesso errato
df_filtered.iloc[0]
```

Problema:

* dopo un filtro, l’index NON viene resettato

Soluzione:

```python
df_filtered = df_filtered.reset_index(drop=True)
```

Insight:
Questo è uno degli errori più comuni nei progetti reali.

---

### 3.9 Selezione con condizioni complesse

```python
df[
    (df["ritardo_min"] > 5) &
    (df["stato"] == "Ritardo")
]
```

Uso reale:

* isolare eventi critici
* costruire alert operativi

---

### 3.10 Caso mobilità urbana

```python
data_bus = {
    "linea_bus": ["90", "91", "92", "90", "92"],
    "ritardo_min": [2, 5, 0, 3, 8],
    "zona": ["Centro", "Nord", "Sud", "Centro", "Sud"]
}

df_bus = pd.DataFrame(data_bus)

# Bus in ritardo in zona Sud
df_bus[
    (df_bus["ritardo_min"] > 3) &
    (df_bus["zona"] == "Sud")
]
```

---

### 3.11 Edge cases (fondamentale)

#### 1. Condizioni su valori nulli

```python
df[df["ritardo_min"].isnull()]
```

#### 2. String matching

```python
df[df["linea"].str.contains("MI")]
```

#### 3. Valori multipli

```python
df[df["linea"].isin(["MI-BS", "MI-VE"])]
```

---

### 3.12 Errori tipici

* usare `and` invece di `&`
* dimenticare parentesi
* confondere `.loc` e `.iloc`
* ignorare l’index dopo un filtro

---

### 3.13 Best practice

* usare `.loc` per chiarezza
* resettare index dopo filtri critici
* scrivere condizioni leggibili
* testare sempre le selezioni

---

### 3.14 Insight da Data Analyst

La selezione dati non è solo tecnica.

È qui che:

* definisci il problema
* scegli cosa analizzare
* costruisci la base dei KPI

Una selezione sbagliata → analisi completamente inutile.

---

### 3.15 Sintesi

* `.loc` è lo standard operativo
* i filtri sono il cuore dell’analisi
* l’index può introdurre errori nascosti
* la chiarezza è più importante della brevità

---

## 4. Pulizia dei dati

### 4.1 Perché la pulizia dei dati è critica

Nel mondo reale, i dati sono quasi sempre:

* incompleti
* incoerenti
* sporchi
* mal formattati

Nel contesto ferroviario e della mobilità urbana, questo significa:

* sensori che non registrano dati
* ritardi mancanti o errati
* duplicazioni di eventi
* formati non standard (date, stringhe, codici)

Un’analisi senza pulizia dati porta a risultati **non affidabili**.

---

### 4.2 Dataset realistico con problemi

```python
import pandas as pd

data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE", None],
    "ritardo_min": [5, None, 0, -3, 7, 7],
    "stato": ["OK", "Ritardo", "OK", "OK", None, "Ritardo"],
    "treno_id": [101, 102, 201, 201, 301, 301]
}

df = pd.DataFrame(data)
```

Problemi presenti:

* valori nulli (`None`)
* ritardo negativo (errore)
* duplicati (`treno_id`)
* valori mancanti in colonne critiche

---

### 4.3 Identificazione dei valori nulli

```python
# Conteggio dei valori nulli per colonna
df.isnull().sum()
```

Insight:

* capire dove si concentra il problema
* valutare se i dati sono recuperabili

---

### 4.4 Rimozione dei valori nulli

```python
# Rimuovere righe con qualsiasi valore nullo
df_clean = df.dropna()
```

Problema:

* perdi dati → rischio perdita informativa

Uso reale:
solo quando i null sono pochi e non critici

---

### 4.5 Rimozione selettiva

```python
# Rimuovere solo se manca il ritardo
df_clean = df.dropna(subset=["ritardo_min"])
```

Insight:

* più controllato
* evita perdita inutile di dati

---

### 4.6 Sostituzione dei valori nulli

```python
# Sostituire null con 0
df["ritardo_min"] = df["ritardo_min"].fillna(0)
```

Attenzione:

* 0 ≠ dato mancante
* può introdurre bias

---

### 4.7 Sostituzione con logica di business

```python
# Sostituire null con media della colonna
media = df["ritardo_min"].mean()
df["ritardo_min"] = df["ritardo_min"].fillna(media)
```

Uso reale:

* imputazione controllata
* più realistico rispetto a zero

---

### 4.8 Gestione dei duplicati

```python
# Individuare duplicati
df[df.duplicated()]

# Rimuovere duplicati
df = df.drop_duplicates()
```

Caso reale:

duplicati possono derivare da:

* doppia registrazione sensore
* errori di ingestione dati

---

### 4.9 Duplicati su colonne specifiche

```python
df = df.drop_duplicates(subset=["treno_id"])
```

Insight:

* definisci cosa significa “duplicato”
* non sempre tutta la riga

---

### 4.10 Correzione dati errati

```python
# Ritardi negativi non validi
df = df[df["ritardo_min"] >= 0]
```

Caso reale:

* errore di sistema
* errore umano
* conversione sbagliata

---

### 4.11 Standardizzazione dei dati

```python
# Uniformare valori stringa
df["stato"] = df["stato"].str.lower()
```

Problema reale:

* "OK", "ok", "Ok" → incoerenza

---

### 4.12 Caso mobilità urbana

```python
data_bus = {
    "linea_bus": ["90", "91", "92", "90", "92"],
    "ritardo_min": [2, None, 0, -1, 8],
    "zona": ["Centro", "Nord", None, "Centro", "Sud"]
}

df_bus = pd.DataFrame(data_bus)
```

Pulizia:

```python
# Rimuovere ritardi negativi
df_bus = df_bus[df_bus["ritardo_min"] >= 0]

# Gestire null
df_bus["ritardo_min"] = df_bus["ritardo_min"].fillna(0)

# Rimuovere righe senza zona
df_bus = df_bus.dropna(subset=["zona"])
```

---

### 4.13 Edge cases

#### 1. Troppi valori nulli

* dataset inutilizzabile
* necessario recupero dati

#### 2. Null sistematici

* problema di sistema
* non casuale

#### 3. Duplicati non evidenti

* stesso treno, timestamp diverso

---

### 4.14 Errori tipici

* eliminare dati senza analisi
* sostituire null con 0 senza criterio
* ignorare duplicati
* non validare i risultati dopo pulizia

---

### 4.15 Best practice

* analizzare prima di modificare
* documentare le scelte
* usare logica di business
* validare il dataset finale

---

### 4.16 Insight da Data Analyst

La pulizia dati non è tecnica, è decisionale.

Stai scegliendo:

* cosa considerare valido
* cosa scartare
* come interpretare l’assenza di dati

Queste scelte influenzano direttamente i KPI.

---

### 4.17 Sintesi

* i dati reali sono sempre sporchi
* la pulizia è una fase critica
* ogni scelta introduce un bias
* la qualità dell’analisi dipende dalla qualità dei dati

---

## 5. Trasformazione dei dati

### 5.1 Ruolo della trasformazione nel processo analitico

Dopo l’esplorazione e la pulizia, i dati sono:

* coerenti
* utilizzabili
* ma ancora poco informativi

La trasformazione serve a:

* creare nuove variabili (feature engineering)
* rendere i dati analizzabili
* preparare KPI e metriche operative

Nel contesto ferroviario e della mobilità urbana, significa trasformare:

* ritardi → indicatori di performance
* timestamp → fasce orarie
* eventi → categorie analitiche

---

### 5.2 Dataset di riferimento

```python
import pandas as pd

data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE"],
    "ritardo_min": [5, 10, 0, 3, 7],
    "ora": ["08:00", "09:30", "07:45", "18:20", "22:10"]
}

df = pd.DataFrame(data)
```

---

### 5.3 Creazione di nuove colonne

```python
# Creare una colonna binaria: ritardo significativo
df["ritardo_critico"] = df["ritardo_min"] > 5
```

Uso reale:

* KPI: numero di eventi critici
* sistemi di alert

---

### 5.4 Trasformazioni vettoriali

```python
# Convertire minuti in secondi
df["ritardo_sec"] = df["ritardo_min"] * 60
```

Insight:

* operazioni veloci su intere colonne
* molto più efficienti dei loop

---

### 5.5 Uso di apply() (logica personalizzata)

```python
def classifica_ritardo(x):
    # Classificazione ritardo secondo soglia operativa
    if x == 0:
        return "puntuale"
    elif x <= 5:
        return "lieve"
    else:
        return "grave"

df["classe_ritardo"] = df["ritardo_min"].apply(classifica_ritardo)
```

Uso reale:

* categorizzazione eventi
* preparazione report

---

### 5.6 Trasformazione temporale

```python
# Convertire stringa in datetime
df["ora"] = pd.to_datetime(df["ora"])

# Estrarre ora
df["fascia_oraria"] = df["ora"].dt.hour
```

Caso reale:

* analisi picchi di traffico
* identificazione fasce critiche

---

### 5.7 Caso reale: classificazione fasce operative

```python
def fascia_operativa(hour):
    # Definizione fasce traffico urbano
    if 7 <= hour <= 9:
        return "peak_morning"
    elif 17 <= hour <= 19:
        return "peak_evening"
    else:
        return "off_peak"

df["fascia"] = df["fascia_oraria"].apply(fascia_operativa)
```

Insight:

* traduci dati grezzi in contesto operativo

---

### 5.8 Trasformazioni su stringhe

```python
# Estrarre origine linea
df["origine"] = df["linea"].str.split("-").str[0]
```

Uso reale:

* analisi per nodo ferroviario
* aggregazioni geografiche

---

### 5.9 Caso mobilità urbana

```python
data_bus = {
    "linea_bus": ["90", "91", "92"],
    "ritardo_min": [2, 5, 8]
}

df_bus = pd.DataFrame(data_bus)

# Classificare performance bus
df_bus["performance"] = df_bus["ritardo_min"].apply(
    lambda x: "buona" if x <= 3 else "critica"
)
```

---

### 5.10 Trasformazioni condizionali avanzate

```python
# Creare colonna con logica multipla
df["priorita"] = df.apply(
    lambda row: "alta" if row["ritardo_min"] > 5 and row["fascia"] == "peak_morning"
    else "normale",
    axis=1
)
```

Uso reale:

* priorità interventi
* gestione operativa

---

### 5.11 Edge cases

#### 1. apply() lento su grandi dataset

* preferire operazioni vettoriali
* usare apply solo quando necessario

#### 2. Tipi di dato errati

```python
# Se ora non è datetime → errore
df["ora"].dt.hour
```

#### 3. Valori nulli

```python
# Gestire null prima di apply
df["ritardo_min"].fillna(0)
```

---

### 5.12 Errori tipici

* usare apply quando non serve
* non validare nuove colonne
* creare logiche non coerenti con il business
* ignorare performance

---

### 5.13 Best practice

* preferire operazioni vettoriali
* mantenere logica semplice e leggibile
* validare ogni trasformazione
* documentare il significato delle nuove variabili

---

### 5.14 Insight da Data Analyst

La trasformazione è dove inizi a:

* costruire KPI
* modellare il problema
* rendere i dati utilizzabili per decisioni

Non stai più analizzando dati.
Stai costruendo un sistema informativo.

---

### 5.15 Sintesi

* trasformare significa creare valore
* nuove colonne = nuove informazioni
* apply è potente ma va usato con criterio
* il contesto operativo guida le trasformazioni

---

## 6. GroupBy e aggregazioni

### 6.1 Perché GroupBy è il cuore dell’analisi

GroupBy è lo strumento principale per:

* costruire KPI
* sintetizzare dati
* identificare pattern operativi

Nel contesto ferroviario e della mobilità urbana, serve per rispondere a domande come:

* Qual è il ritardo medio per linea?
* Quali tratte sono più critiche?
* In quali fasce orarie si concentrano i problemi?

Concetto chiave:

**split → apply → combine**

1. split → dividi i dati in gruppi
2. apply → applichi una funzione
3. combine → ricombini i risultati

---

### 6.2 Dataset di riferimento

```python
import pandas as pd

data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE", "MI-VE"],
    "ritardo_min": [5, 10, 0, 3, 7, 50],
    "fascia": ["peak_morning", "off_peak", "peak_morning", "peak_evening", "off_peak", "peak_evening"]
}

df = pd.DataFrame(data)
```

---

### 6.3 Aggregazione base

```python
# Ritardo medio per linea
df.groupby("linea")["ritardo_min"].mean()
```

Interpretazione:

* individua linee problematiche
* base per KPI operativi

---

### 6.4 Aggregazioni multiple

```python
df.groupby("linea")["ritardo_min"].agg(["mean", "max", "min"])
```

Uso reale:

* media → performance generale
* max → worst case
* min → baseline operativa

---

### 6.5 GroupBy su più colonne

```python
df.groupby(["linea", "fascia"])["ritardo_min"].mean()
```

Insight:

* analisi più granulare
* combina dimensioni operative

---

### 6.6 Reset dell’indice

```python
result = df.groupby("linea")["ritardo_min"].mean().reset_index()
```

Perché è importante:

* output più leggibile
* utile per esportazione e dashboard

---

### 6.7 Caso reale: KPI ferroviario

```python
# KPI: ritardo medio per linea
kpi_linea = df.groupby("linea")["ritardo_min"].mean().reset_index()

# Ordinare per criticità
kpi_linea = kpi_linea.sort_values(by="ritardo_min", ascending=False)

print(kpi_linea)
```

Interpretazione:

* priorità operativa sulle linee peggiori

---

### 6.8 Caso mobilità urbana

```python
data_bus = {
    "linea_bus": ["90", "90", "91", "91", "92"],
    "ritardo_min": [2, 5, 3, 7, 8],
    "zona": ["Centro", "Centro", "Nord", "Nord", "Sud"]
}

df_bus = pd.DataFrame(data_bus)

# Ritardo medio per zona
df_bus.groupby("zona")["ritardo_min"].mean()
```

Insight:

* identificazione aree urbane critiche

---

### 6.9 Conteggi (count vs size)

```python
# Numero osservazioni per linea
df.groupby("linea")["ritardo_min"].count()

# Conteggio righe (include null)
df.groupby("linea").size()
```

Differenza critica:

* `count()` esclude null
* `size()` include tutto

---

### 6.10 Aggregazioni personalizzate

```python
df.groupby("linea")["ritardo_min"].agg(
    ritardo_medio="mean",
    ritardo_massimo="max",
    numero_treni="count"
)
```

Uso reale:

* costruzione dataset KPI strutturato

---

### 6.11 Filtrare dopo GroupBy

```python
kpi = df.groupby("linea")["ritardo_min"].mean().reset_index()

# Linee con ritardo medio > 5
kpi[kpi["ritardo_min"] > 5]
```

---

### 6.12 Ordinamento dei risultati

```python
df.groupby("linea")["ritardo_min"].mean().sort_values(ascending=False)
```

Insight:

* ranking immediato delle criticità

---

### 6.13 Edge cases

#### 1. Gruppi con pochi dati

* media non significativa
* rischio interpretazione errata

#### 2. Presenza di outlier

* distorsione KPI
* usare median() come alternativa

```python
df.groupby("linea")["ritardo_min"].median()
```

---

#### 3. Valori nulli

* influenzano aggregazioni
* controllare prima del GroupBy

---

### 6.14 Errori tipici

* non resettare index
* interpretare media senza contesto
* ignorare dimensione del gruppo
* non considerare outlier

---

### 6.15 Best practice

* combinare più metriche
* ordinare sempre i risultati
* validare distribuzione dati
* usare GroupBy come base KPI

---

### 6.16 Insight da Data Analyst

GroupBy è il punto in cui:

* trasformi dati in metriche
* costruisci KPI
* supporti decisioni operative

È il passaggio da:

dato → informazione → decisione

---

### 6.17 Sintesi

* GroupBy è il cuore dell’analisi
* permette di costruire KPI reali
* richiede interpretazione, non solo calcolo
* la qualità dei dati influenza il risultato

---

## 7. Merge e Join dei dati

### 7.1 Perché Merge è fondamentale

Nei contesti reali, i dati non sono mai in un unico dataset.

Nel dominio ferroviario e della mobilità urbana, i dati arrivano da fonti diverse:

* sistemi di monitoraggio ritardi
* orari programmati
* sensori di linea
* eventi operativi (guasti, manutenzione)

Per costruire un’analisi completa, devi **integrare queste fonti**.

Errore critico:
un merge sbagliato produce dati apparentemente corretti ma in realtà falsi.

---

### 7.2 Dataset di riferimento

#### Dataset 1: ritardi treni

```python
import pandas as pd

df_ritardi = pd.DataFrame({
    "treno_id": [101, 102, 201, 202],
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO"],
    "ritardo_min": [5, 10, 0, 3]
})
```

---

#### Dataset 2: orari programmati

```python
df_orari = pd.DataFrame({
    "treno_id": [101, 102, 201, 203],
    "orario_previsto": ["08:00", "09:00", "07:30", "10:00"]
})
```

---

### 7.3 Merge base (inner join)

```python
df_merge = pd.merge(df_ritardi, df_orari, on="treno_id")
```

Risultato:

* mantiene solo i treni presenti in entrambi i dataset

Insight:

* `treno_id = 202` viene escluso
* `treno_id = 203` viene escluso

---

### 7.4 Tipi di join (fondamentale)

#### INNER JOIN

```python
pd.merge(df_ritardi, df_orari, on="treno_id", how="inner")
```

* solo intersezione

---

#### LEFT JOIN

```python
pd.merge(df_ritardi, df_orari, on="treno_id", how="left")
```

* mantiene tutto il dataset di sinistra
* aggiunge dati quando disponibili

Uso reale:
dataset principale + arricchimento

---

#### RIGHT JOIN

```python
pd.merge(df_ritardi, df_orari, on="treno_id", how="right")
```

* mantiene dataset di destra

---

#### OUTER JOIN

```python
pd.merge(df_ritardi, df_orari, on="treno_id", how="outer")
```

* unione completa
* introduce valori nulli

---

### 7.5 Caso reale: arricchimento dati

```python
df = pd.merge(df_ritardi, df_orari, on="treno_id", how="left")
```

Interpretazione:

* tutti i treni monitorati
* aggiunta informazione orario

---

### 7.6 Merge su colonne con nomi diversi

```python
df1 = pd.DataFrame({
    "train_id": [101, 102],
    "ritardo": [5, 10]
})

df2 = pd.DataFrame({
    "id_treno": [101, 102],
    "stato": ["OK", "Ritardo"]
})

pd.merge(df1, df2, left_on="train_id", right_on="id_treno")
```

---

### 7.7 Join su più colonne

```python
df_a = pd.DataFrame({
    "linea": ["MI-BS", "MI-BS"],
    "ora": ["08:00", "09:00"],
    "ritardo": [5, 10]
})

df_b = pd.DataFrame({
    "linea": ["MI-BS", "MI-BS"],
    "ora": ["08:00", "09:00"],
    "treni": [50, 60]
})

pd.merge(df_a, df_b, on=["linea", "ora"])
```

Uso reale:

* join temporali
* join per contesto operativo

---

### 7.8 Problema reale: duplicazione righe

```python
df_dup = pd.DataFrame({
    "treno_id": [101, 101],
    "evento": ["ritardo", "guasto"]
})

pd.merge(df_ritardi, df_dup, on="treno_id")
```

Problema:

* duplicazione delle righe
* effetto moltiplicatore

Insight:

Questo è uno degli errori più pericolosi.

---

### 7.9 Validazione del merge

```python
df_merge.shape
```

Controlli:

* numero righe prima/dopo
* presenza duplicati
* valori nulli inattesi

---

### 7.10 Caso mobilità urbana

```python
df_bus = pd.DataFrame({
    "linea_bus": ["90", "91", "92"],
    "ritardo_min": [2, 5, 8]
})

df_zone = pd.DataFrame({
    "linea_bus": ["90", "91", "92"],
    "zona": ["Centro", "Nord", "Sud"]
})

df_bus = pd.merge(df_bus, df_zone, on="linea_bus")
```

Insight:

* arricchimento geografico
* analisi per area urbana

---

### 7.11 Edge cases

#### 1. Chiavi non univoche

* generano duplicati
* richiedono aggregazione preventiva

---

#### 2. Valori nulli nelle chiavi

```python
# Non verranno matchati
```

---

#### 3. Tipi di dato diversi

```python
# int vs string → merge fallisce
```

---

### 7.12 Errori tipici

* non specificare il tipo di join
* ignorare duplicazioni
* non validare output
* usare chiavi non univoche

---

### 7.13 Best practice

* conoscere sempre le chiavi
* validare dimensione dataset
* usare left join come default operativo
* controllare duplicati post-merge

---

### 7.14 Insight da Data Analyst

Il merge è il punto in cui costruisci il dataset finale.

Se sbagli qui:

* KPI falsi
* analisi incoerente
* decisioni sbagliate

È una fase tecnica, ma con impatto diretto sul business.

---

### 7.15 Sintesi

* i dati reali sono distribuiti
* merge integra le informazioni
* errori di join sono critici
* validazione è obbligatoria

---

## 8. Time Series e analisi temporale

### 8.1 Perché le Time Series sono critiche

Nel contesto ferroviario e della mobilità urbana, il tempo è una variabile fondamentale.

Quasi tutti i problemi operativi sono temporali:

* ritardi nelle ore di punta
* congestione in specifiche fasce orarie
* degrado delle performance nel tempo
* eventi anomali (guasti, incidenti)

Senza una corretta gestione delle time series, l’analisi è incompleta.

---

### 8.2 Dataset di riferimento

```python
import pandas as pd

data = {
    "timestamp": [
        "2024-01-01 07:30",
        "2024-01-01 08:00",
        "2024-01-01 08:30",
        "2024-01-01 17:30",
        "2024-01-01 18:00"
    ],
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE"],
    "ritardo_min": [2, 10, 3, 15, 7]
}

df = pd.DataFrame(data)
```

---

### 8.3 Conversione a datetime

```python
# Conversione a formato datetime
df["timestamp"] = pd.to_datetime(df["timestamp"])
```

Perché è fondamentale:

* abilita tutte le operazioni temporali
* senza questa conversione → analisi impossibile

---

### 8.4 Impostare il tempo come indice

```python
df = df.set_index("timestamp")
```

Insight:

* molte operazioni time series richiedono un index temporale
* standard nei progetti reali

---

### 8.5 Estrazione di componenti temporali

```python
# Estrarre ora e giorno
df["ora"] = df.index.hour
df["giorno"] = df.index.day
```

Uso reale:

* analisi per fascia oraria
* identificazione pattern giornalieri

---

### 8.6 Caso reale: analisi per fascia oraria

```python
df.groupby("ora")["ritardo_min"].mean()
```

Interpretazione:

* individua ore critiche
* base per pianificazione operativa

---

### 8.7 Resampling (concetto chiave)

Resampling = cambiare granularità temporale

```python
# Media ritardi ogni ora
df.resample("H")["ritardo_min"].mean()
```

Uso reale:

* dati grezzi → aggregazioni temporali
* preparazione dashboard

---

### 8.8 Resampling giornaliero

```python
df.resample("D")["ritardo_min"].mean()
```

Insight:

* analisi trend giornaliero
* monitoraggio performance

---

### 8.9 Gestione dati mancanti nel tempo

```python
df.resample("H")["ritardo_min"].mean().fillna(0)
```

Problema reale:

* intervalli senza dati
* sensori offline

---

### 8.10 Caso reale: mobilità urbana

```python
data_bus = {
    "timestamp": [
        "2024-01-01 08:00",
        "2024-01-01 08:10",
        "2024-01-01 08:20",
        "2024-01-01 18:00"
    ],
    "ritardo_min": [2, 5, 3, 10]
}

df_bus = pd.DataFrame(data_bus)

df_bus["timestamp"] = pd.to_datetime(df_bus["timestamp"])
df_bus = df_bus.set_index("timestamp")

# Media ogni 30 minuti
df_bus.resample("30T")["ritardo_min"].mean()
```

---

### 8.11 Rolling window (analisi dinamica)

```python
# Media mobile su 2 osservazioni
df["rolling_mean"] = df["ritardo_min"].rolling(window=2).mean()
```

Uso reale:

* smoothing dei dati
* identificazione trend

---

### 8.12 Shift (analisi temporale avanzata)

```python
# Confronto con valore precedente
df["ritardo_precedente"] = df["ritardo_min"].shift(1)
```

Insight:

* analisi variazioni
* identificazione cambiamenti improvvisi

---

### 8.13 Edge cases

#### 1. Timestamp non ordinati

```python
df = df.sort_index()
```

---

#### 2. Timezone

* dati da fonti diverse → inconsistenza
* necessario allineamento

---

#### 3. Frequenze irregolari

* dati non uniformi nel tempo
* resample necessario

---

### 8.14 Errori tipici

* non convertire a datetime
* non impostare index temporale
* ignorare dati mancanti
* interpretare male il resample

---

### 8.15 Best practice

* convertire sempre a datetime
* usare index temporale
* validare frequenza dati
* gestire null temporali

---

### 8.16 Insight da Data Analyst

Le time series permettono di:

* capire quando avvengono i problemi
* identificare pattern operativi
* supportare decisioni temporali

Non basta sapere che esiste un problema.
Serve sapere **quando accade**.

---

### 8.17 Sintesi

* il tempo è una dimensione critica
* datetime è obbligatorio
* resample è uno strumento chiave
* le time series abilitano analisi avanzate

---

## 9. Visualizzazione dei dati

### 9.1 Perché la visualizzazione è importante

La visualizzazione non è “abbellimento”.
È uno strumento per:

* capire meglio i dati
* individuare pattern rapidamente
* comunicare risultati in modo chiaro

Nel lavoro reale:

un grafico fatto bene vale più di una tabella.

---

### 9.2 Dataset di riferimento

```python
import pandas as pd
import matplotlib.pyplot as plt

data = {
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE", "MI-VE"],
    "ritardo_min": [5, 10, 0, 3, 7, 50]
}

df = pd.DataFrame(data)
```

---

### 9.3 Istogramma (distribuzione)

```python
df["ritardo_min"].hist()
plt.show()
```

Cosa guardare:

* distribuzione dei ritardi
* presenza di outlier
* concentrazione dei valori

Insight reale:

* se hai una coda lunga → pochi eventi molto critici
* se distribuzione compatta → sistema stabile

---

### 9.4 Grafico a barre (KPI per categoria)

```python
df.groupby("linea")["ritardo_min"].mean().plot(kind="bar")
plt.show()
```

Uso reale:

* confronto tra linee
* identificazione criticità

Questo è uno dei grafici più usati in assoluto.

---

### 9.5 Line plot (time series)

```python
data_time = {
    "timestamp": [
        "2024-01-01 08:00",
        "2024-01-01 09:00",
        "2024-01-01 10:00"
    ],
    "ritardo_min": [2, 5, 8]
}

df_time = pd.DataFrame(data_time)
df_time["timestamp"] = pd.to_datetime(df_time["timestamp"])

df_time.plot(x="timestamp", y="ritardo_min")
plt.show()
```

Uso reale:

* andamento nel tempo
* identificazione trend

---

### 9.6 Caso reale: confronto linee ferroviarie

```python
kpi = df.groupby("linea")["ritardo_min"].mean()

kpi.plot(kind="bar")
plt.title("Ritardo medio per linea")
plt.xlabel("Linea")
plt.ylabel("Ritardo medio (min)")
plt.show()
```

Interpretazione:

* colonna più alta = problema operativo
* base per decisioni

---

### 9.7 Caso mobilità urbana

```python
data_bus = {
    "zona": ["Centro", "Nord", "Sud"],
    "ritardo_medio": [3, 6, 8]
}

df_bus = pd.DataFrame(data_bus)

df_bus.plot(x="zona", y="ritardo_medio", kind="bar")
plt.show()
```

Insight:

* analisi geografica
* supporto pianificazione urbana

---

### 9.8 Migliorare leggibilità (base)

```python
kpi.plot(kind="bar")
plt.title("Performance linee")
plt.xticks(rotation=0)
plt.show()
```

Regola semplice:

* meno è meglio
* grafico chiaro > grafico complesso

---

### 9.9 Edge cases

#### 1. Troppi dati

* grafico illeggibile
* aggregare prima (GroupBy)

---

#### 2. Outlier

* distorcono la scala
* valutare separazione o log scale

---

#### 3. Dati mancanti

* grafici incompleti
* verificare prima

---

### 9.10 Errori tipici

* fare grafici senza capire i dati
* usare troppi elementi visivi
* non etichettare assi
* interpretare male i risultati

---

### 9.11 Best practice

* partire sempre da un KPI
* scegliere il grafico giusto
* mantenere semplicità
* usare la visualizzazione per confermare insight

---

### 9.12 Insight da Data Analyst

La visualizzazione serve per:

* validare quello che pensi
* spiegarlo agli altri
* prendere decisioni

Non serve fare grafici belli.
Serve fare grafici utili.

---

### 9.13 Sintesi

* la visualizzazione è parte dell’analisi
* pochi grafici, ma mirati
* sempre legati a una domanda
* supporto diretto alle decisioni

---

## 10. Casi studio reali

### 10.1 Obiettivo della sezione

Questa sezione serve a mettere insieme tutto:

* pulizia dati
* trasformazione
* groupby
* time series
* visualizzazione

Non è teoria.
È simulazione di lavoro reale.

---

# Caso 1 — Analisi ritardi ferroviari

### 10.2 Contesto

Hai un dataset operativo con:

* ritardi treni
* timestamp
* linee

Obiettivo:

* identificare linee critiche
* capire quando avvengono i problemi

---

### 10.3 Dataset

```python
import pandas as pd

data = {
    "timestamp": [
        "2024-01-01 07:30",
        "2024-01-01 08:00",
        "2024-01-01 08:30",
        "2024-01-01 17:30",
        "2024-01-01 18:00",
        "2024-01-01 18:30"
    ],
    "linea": ["MI-BS", "MI-BS", "MI-TO", "MI-TO", "MI-VE", "MI-VE"],
    "ritardo_min": [2, 10, 3, 15, None, 7]
}

df = pd.DataFrame(data)
```

---

### 10.4 Pulizia dati

```python
# Convertire timestamp
df["timestamp"] = pd.to_datetime(df["timestamp"])

# Gestire valori nulli
df["ritardo_min"] = df["ritardo_min"].fillna(0)
```

Scelta:

* sostituiamo null con 0 → assumiamo “nessun ritardo registrato”

Nota:

questa è una decisione, non una verità assoluta

---

### 10.5 Trasformazione

```python
# Estrarre ora
df["ora"] = df["timestamp"].dt.hour

# Classificare fascia
def fascia(hour):
    if 7 <= hour <= 9:
        return "peak_morning"
    elif 17 <= hour <= 19:
        return "peak_evening"
    else:
        return "off_peak"

df["fascia"] = df["ora"].apply(fascia)
```

---

### 10.6 KPI principali

```python
# Ritardo medio per linea
kpi_linea = df.groupby("linea")["ritardo_min"].mean()

# Ritardo medio per fascia
kpi_fascia = df.groupby("fascia")["ritardo_min"].mean()
```

---

### 10.7 Analisi

Possibili insight:

* alcune linee mostrano ritardi sistematici
* le fasce di punta hanno valori più alti
* presenza di variabilità tra linee

---

### 10.8 Visualizzazione

```python
import matplotlib.pyplot as plt

kpi_linea.plot(kind="bar")
plt.title("Ritardo medio per linea")
plt.show()
```

---

### 10.9 Conclusioni operative

* identificare linee prioritarie
* intervenire su fasce critiche
* monitorare trend nel tempo

---

# Caso 2 — Mobilità urbana (bus)

### 10.10 Contesto

Dataset autobus:

* linea
* zona
* ritardo

Obiettivo:

* identificare aree urbane critiche

---

### 10.11 Dataset

```python
data_bus = {
    "linea_bus": ["90", "90", "91", "91", "92", "92"],
    "zona": ["Centro", "Centro", "Nord", "Nord", "Sud", "Sud"],
    "ritardo_min": [2, 4, 3, 7, 5, 9]
}

df_bus = pd.DataFrame(data_bus)
```

---

### 10.12 KPI

```python
# Ritardo medio per zona
kpi_zona = df_bus.groupby("zona")["ritardo_min"].mean()
```

---

### 10.13 Analisi

* zona Sud più critica
* differenze tra aree urbane

---

### 10.14 Visualizzazione

```python
kpi_zona.plot(kind="bar")
plt.title("Ritardo medio per zona")
plt.show()
```

---

### 10.15 Insight operativo

* allocare risorse in zone critiche
* migliorare pianificazione
* supportare decisioni strategiche

---

# Caso 3 — Integrazione dati (merge reale)

### 10.16 Contesto

Unisci:

* ritardi
* orari

---

### 10.17 Dataset

```python
df_ritardi = pd.DataFrame({
    "treno_id": [101, 102, 201],
    "ritardo_min": [5, 10, 3]
})

df_orari = pd.DataFrame({
    "treno_id": [101, 102, 201],
    "orario_previsto": ["08:00", "09:00", "07:30"]
})
```

---

### 10.18 Merge

```python
df = pd.merge(df_ritardi, df_orari, on="treno_id")
```

---

### 10.19 Trasformazione

```python
df["orario_previsto"] = pd.to_datetime(df["orario_previsto"])
df["ora"] = df["orario_previsto"].dt.hour
```

---

### 10.20 Insight

* relazione tra orario e ritardo
* analisi temporale integrata

---

# 10.21 Errori comuni nei casi reali

* non validare i dati
* KPI senza contesto
* merge errati
* interpretazioni affrettate

---

# 10.22 Best practice

* lavorare per step
* validare ogni passaggio
* collegare sempre i dati al contesto
* documentare le scelte

---

# 10.23 Insight finale

Un Data Analyst non:

* esegue codice
* crea grafici

Un Data Analyst:

* interpreta sistemi reali
* costruisce metriche
* supporta decisioni

Pandas è solo lo strumento.

---

# 10.24 Sintesi

* integra tutte le competenze
* simula lavoro reale
* prepara per portfolio e colloqui
* collega dati a decisioni

---

Autore: Miguel Saldana 24/04/2026
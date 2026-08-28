# 🌌 Classificazione di Oggetti Celesti — Sloan Digital Sky Survey (SDSS DR14)

*[Read this in English](README.en.md)*

Analisi comparativa di modelli di Machine Learning per la classificazione di oggetti celesti (Stelle, Galassie, Quasar) a partire da dati fotometrici e spettroscopici della Sloan Digital Sky Survey.

## 🎯 Obiettivo

Confrontare le performance di quattro modelli di classificazione multiclasse — **Logistic Regression**, **K-Nearest Neighbors**, **Decision Tree** e **Random Forest** — nel distinguere tra le tre categorie fondamentali di oggetti celesti: **STAR**, **GALAXY** e **QSO (Quasar)**, e comprendere quali proprietà fisiche guidano la classificazione.

## 📁 Dataset

- **Fonte:** [Sloan Digital Sky Survey, Data Release 14](https://www.sdss.org/) (2017)
- **Dimensione:** 10.000 osservazioni, 18 variabili originali
- **Target:** `class` (STAR / GALAXY / QSO) — dataset sbilanciato

## 🔬 Metodologia

**1. Data Cleaning & Feature Selection**
Rimozione degli identificatori tecnici privi di potere predittivo (`objid`, `specobjid`, `run`, `rerun`, `camcol`, `field`, `plate`, `mjd`, `fiberid`) e verifica di missing values/duplicati (nessuno riscontrato).

**2. Exploratory Data Analysis**
Analisi univariata e bivariata (istogrammi, box plot, pair plot) per individuare distribuzioni, outlier e collinearità tra le variabili.

**3. Feature Engineering — PCA**
Le cinque bande fotometriche (`u`, `g`, `r`, `i`, `z`), fortemente collineari tra loro, sono state ridotte a due componenti principali (`PCA_1`, `PCA_2`) tramite Principal Component Analysis, preservando l'informazione spettrale senza ridondanza.

**4. Modeling**
Split 70/30 stratificato, standardizzazione delle feature (fit solo su training set, per evitare data leakage), addestramento e validazione tramite Stratified 5-Fold Cross-Validation.

**5. Inferenza statistica**
Bootstrap non parametrico (B = 1.000 iterazioni) sui coefficienti della Regressione Logistica Multinomiale per stimare errori standard, z-score e p-value, e identificare le variabili statisticamente significative per ciascuna classe.

## 📈 Risultati

| Modello | Accuratezza CV (5-fold) | Accuratezza Test Set |
|---|---|---|
| Logistic Regression (OvR) | 92.11% | 91.97% |
| K-Nearest Neighbors | 94.73% | 95.40% |
| Logistic Regression (Softmax) | 96.10% | 96.00% |
| Decision Tree | 98.50% | 98.80% |
| **Random Forest** 🏆 | **98.80%** | **99.10%** |

Il **Random Forest** (100 alberi) è il modello con le performance migliori in assoluto, confermando la superiorità dell'approccio ensemble nel ridurre la varianza senza aumentare il bias.

## 💡 Key Insights

- **Il redshift è il predittore dominante**: sia i coefficienti della Regressione Logistica (validati via bootstrap) sia la feature importance del Random Forest lo identificano come la variabile più rilevante, in perfetta coerenza con la fisica del problema — il redshift separa nettamente oggetti locali (Stelle) da oggetti cosmologici distanti (Galassie e Quasar).
- **Le componenti PCA (informazione spettrale) sono decisive per distinguere Galassie da Quasar**, entrambe ad alto redshift ma con profili spettrali diversi; risultano invece non significative per la classe Stella, che si spiega quasi esclusivamente con il redshift.
- **Softmax batte One-vs-Rest** in accuratezza globale (96.00% vs 91.97%), ma l'OvR ottiene una Recall superiore sulla classe minoritaria Quasar (97%) — un classico trade-off tra performance globale e sensibilità su una classe rara.
- Data la natura sbilanciata del dataset, la valutazione non si è basata sulla sola accuratezza ma su un set completo di metriche (Precision, Recall, F1-Score, matrici di confusione).

## 🛠️ Tech Stack

`Python` · `pandas` · `NumPy` · `scikit-learn` · `SciPy` · `seaborn` · `matplotlib`

## 📂 Struttura del repository

```
├── notebook ITA/
│   └── SDSS_classification ITA.ipynb
│   └── PDF report/
│       └── SDSS_classification ITA.pdf
├── data/
│   └── (dataset SDSS DR14 — vedi sezione Dataset)
├── requirements.txt
└── README.md
```

## ▶️ Come eseguire il progetto

```bash
git clone https://github.com/elenacascone/<Celestial-Object-Classification>.git
cd <Celestial-Object-Classification>
pip install -r requirements.txt
jupyter notebook notebook ITA/SDSS_classification.ipynb
```

## 👤 Autore

**Elena Cascone**
[LinkedIn](https://www.linkedin.com/in/elena-cascone-18ec/) · [GitHub](https://github.com/elenacascone)

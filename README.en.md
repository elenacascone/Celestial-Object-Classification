# 🌌 Celestial Object Classification — Sloan Digital Sky Survey (SDSS DR14)

*[Leggi in italiano](README.md)*

A comparative Machine Learning analysis for classifying celestial objects (Stars, Galaxies, Quasars) using photometric and spectroscopic data from the Sloan Digital Sky Survey.

## 🎯 Objective

Compare the performance of four multiclass classification models — **Logistic Regression**, **K-Nearest Neighbors**, **Decision Tree**, and **Random Forest** — in distinguishing between the three fundamental categories of celestial objects: **STAR**, **GALAXY**, and **QSO (Quasar)**, and identify which physical properties drive the classification.

## 📁 Dataset

- **Source:** [Sloan Digital Sky Survey, Data Release 14](https://www.sdss.org/) (2017)
- **Size:** 10,000 observations, 18 original features
- **Target:** `class` (STAR / GALAXY / QSO) — imbalanced dataset

## 🔬 Methodology

**1. Data Cleaning & Feature Selection**
Removed technical identifiers with no predictive power (`objid`, `specobjid`, `run`, `rerun`, `camcol`, `field`, `plate`, `mjd`, `fiberid`) and checked for missing values/duplicates (none found).

**2. Exploratory Data Analysis**
Univariate and bivariate analysis (histograms, box plots, pair plots) to detect distributions, outliers, and collinearity among features.

**3. Feature Engineering — PCA**
The five photometric bands (`u`, `g`, `r`, `i`, `z`), which showed strong collinearity, were reduced to two principal components (`PCA_1`, `PCA_2`) via Principal Component Analysis, preserving spectral information without redundancy.

**4. Modeling**
Stratified 70/30 split, feature standardization (fit on the training set only, to avoid data leakage), training and validation via Stratified 5-Fold Cross-Validation.

**5. Statistical Inference**
Non-parametric bootstrap (B = 1,000 iterations) on the Multinomial Logistic Regression coefficients to estimate standard errors, z-scores, and p-values, identifying which variables are statistically significant for each class.

## 📈 Results

| Model | CV Accuracy (5-fold) | Test Set Accuracy |
|---|---|---|
| Logistic Regression (OvR) | 92.11% | 91.97% |
| K-Nearest Neighbors | 94.73% | 95.40% |
| Logistic Regression (Softmax) | 96.10% | 96.00% |
| Decision Tree | 98.50% | 98.80% |
| **Random Forest** 🏆 | **98.80%** | **99.10%** |

**Random Forest** (100 trees) is the best-performing model overall, confirming the strength of the ensemble approach in reducing variance without increasing bias.

## 💡 Key Insights

- **Redshift is the dominant predictor**: both the Logistic Regression coefficients (validated via bootstrap) and the Random Forest feature importance identify it as the most relevant variable — fully consistent with the underlying physics, as redshift cleanly separates local objects (Stars) from distant cosmological ones (Galaxies and Quasars).
- **PCA components (spectral information) are key to distinguishing Galaxies from Quasars**, both of which have high redshift but different spectral profiles; they turn out to be non-significant for the Star class, which is explained almost entirely by redshift.
- **Softmax outperforms One-vs-Rest** in overall accuracy (96.00% vs 91.97%), but OvR achieves a higher Recall on the minority Quasar class (97%) — a classic trade-off between global performance and sensitivity on a rare class.
- Given the imbalanced nature of the dataset, evaluation relied on a full set of metrics (Precision, Recall, F1-Score, confusion matrices) rather than accuracy alone.

## 🛠️ Tech Stack

`Python` · `pandas` · `NumPy` · `scikit-learn` · `SciPy` · `seaborn` · `matplotlib`

## 📂 Repository Structure

```
├── notebook/
│   └── SDSS_classification.ipynb
├── data/
│   └── (SDSS DR14 dataset — see Dataset section)
├── images/
│   └── (exported plots for the README)
├── requirements.txt
└── README.md
```

## ▶️ How to Run

```bash
git clone https://github.com/elenacascone/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
jupyter notebook notebook/SDSS_classification.ipynb
```

## 👤 Author

**Elena Cascone**
[LinkedIn](https://www.linkedin.com/in/elena-cascone-18ec/) · [GitHub](https://github.com/elenacascone)

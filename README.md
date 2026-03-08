<p align="center">
  <h1 align="center">🏗️ ML Analysis of Pile Settlement</h1>
  <p align="center">
    <em>A comprehensive machine learning framework for predicting pile settlement with advanced uncertainty quantification techniques</em>
  </p>
  <p align="center">
    <a href="#-overview"><strong>Overview</strong></a> · 
    <a href="#-repository-structure"><strong>Structure</strong></a> · 
    <a href="#-dataset"><strong>Dataset</strong></a> · 
    <a href="#-models"><strong>Models</strong></a> · 
    <a href="#-modules"><strong>Modules</strong></a> · 
    <a href="#-getting-started"><strong>Getting Started</strong></a>
  </p>
</p>

---

## 📖 Overview

This repository presents a **machine learning-driven approach** for predicting pile settlement under axial loading. It goes beyond simple point predictions by incorporating multiple **Uncertainty Quantification (UQ)** methodologies, ensuring that predictions are accompanied by calibrated confidence intervals — a critical requirement in geotechnical engineering applications.

The pipeline covers the full ML lifecycle:

1. **Hyperparameter Tuning** — Automated model optimization via Optuna with 8 pruning strategies  
2. **Conformal Prediction** — Distribution-free prediction intervals (MAPIE, PUNCC, NEXCP, Adaptive CP, mfcs)  
3. **Probabilistic Forecasting** — Full predictive distributions via NGBoost, PGBM, and related methods  
4. **Quantile Regression** — Direct estimation of conditional quantiles for prediction intervals  

---

## 📂 Repository Structure

```
ML_Analysis_Pile_Settlement/
│
├── 📁 Data/                                          # Train & test datasets
│   ├── train.csv                                     # Training set (372 samples)
│   └── test.csv                                      # Test set
│
├── 📁 HyperParameter_Tuning/                         # Optuna-based hyperparameter optimization
│   ├── Optuna_autosampler.ipynb                      # Tuning notebook with AutoSampler
│   ├── models/                                       # 80 saved best models (.pkl)
│   ├── test.xlsx                                     # Test data (Excel)
│   └── test_results.xlsx                             # Aggregated tuning results
│
├── 📁 Conformal_Predictions(MAPIE,PUNCC)/             # Conformal prediction — MAPIE & PUNCC
│   ├── Conformal Predictions(MAPIE,PUNCC).ipynb      # Analysis notebook
│   └── <Model>.xlsx                                  # Per-model result sheets (×9)
│
├── 📁 Conformal_Predictions(NEXCP,AdaptiveCP,mfcs)/   # Conformal prediction — NEXCP, Adaptive CP, mfcs
│   ├── Conformal_Predictions(NEXCP, Adaptive CP, mfcs).ipynb
│   └── <Model>.xlsx                                  # Per-model result sheets (×9)
│
├── 📁 Probabilistic_Distribution/                     # Probabilistic predictive distributions
│   ├── Probabilistic__Distribution.ipynb             # Analysis notebook
│   ├── Matrix Evaluation.xlsx                        # Evaluation metrics matrix
│   └── <Model>_Prob.xlsx / <Model>_Prob_TF.xlsx      # Per-model probabilistic results
│
├── 📁 Probabilistic_Distribution(CARD)/               # CARD-based probabilistic distribution
│   ├── Probabilistic__Distribution(CARD).ipynb       # Analysis notebook
│   ├── Matrix Evaluation.xlsx                        # Evaluation metrics matrix
│   └── <Model>.xlsx                                  # Per-model result sheets (×9)
│
├── 📁 Quantile_Regression/                            # Quantile regression analysis
│   ├── Quantile_Regression.ipynb                     # Analysis notebook
│   └── <Model>.xlsx                                  # Per-model result sheets (×9)
│
└── README.md                                         # This file
```

---

## 📊 Dataset

The dataset consists of **pile load test records** described by the following features:

| Feature      | Description                                               |
|:-------------|:----------------------------------------------------------|
| `D`          | Pile diameter (m)                                         |
| `L`          | Pile length (m)                                           |
| `N30_Ava`    | Average SPT N-value along the pile shaft                  |
| `Cu_Ava`     | Average undrained shear strength along the shaft (kPa)    |
| `N30_Base`   | SPT N-value at the pile base                              |
| `Cu_Base`    | Undrained shear strength at the pile base (kPa)           |
| `UL`         | Unloading indicator (load cycle identifier)               |
| `Q`          | Applied axial load (kN)                                   |
| **`S_exp`**  | **Measured pile settlement (mm) — Target variable**       |

- **Training samples:** 372  
- **Test samples:** ~180  

---

## 🤖 Models

The following **10 gradient boosting and ensemble models** are benchmarked across all UQ modules:

| Model                        | Key Library                     |
|:-----------------------------|:--------------------------------|
| XGBoost                      | `xgboost`                       |
| LightGBM                     | `lightgbm`                      |
| CatBoost                     | `catboost`                      |
| Gradient Boosting            | `scikit-learn`                  |
| Histogram Gradient Boosting  | `scikit-learn`                  |
| GPBoost                      | `gpboost`                       |
| NGBoost                      | `ngboost`                       |
| PGBM                         | `pgbm`                          |
| TabNet                       | `pytorch-tabnet`                |
| Random Forest                | `scikit-learn`                  |

> All models are tuned using **Optuna** with 8 pruning strategies: `MedianPruner`, `NopPruner`, `PatientPruner`, `PercentilePruner`, `SuccessiveHalvingPruner`, `HyperbandPruner`, `ThresholdPruner`, and `WilcoxonPruner`.

---

## 🔬 Modules

### 1️⃣ Hyperparameter Tuning

> 📓 **Notebook:** [`Optuna_autosampler.ipynb`](HyperParameter_Tuning/Optuna_autosampler.ipynb)

- Uses **Optuna AutoSampler** paired with 8 different pruning strategies  
- Produces **80 optimized models** (10 models × 8 pruners), saved as `.pkl` files  
- Results aggregated in `test_results.xlsx`

### 2️⃣ Conformal Predictions — MAPIE & PUNCC

> 📓 **Notebook:** [`Conformal Predictions(MAPIE,PUNCC).ipynb`](Conformal_Predictions(MAPIE,PUNCC)/Conformal%20Predictions(MAPIE,PUNCC).ipynb)

- Implements **MAPIE** (Model Agnostic Prediction Interval Estimator)  
- Implements **PUNCC** (Predictive Uncertainty for Neural Network Conformal Calibration)  
- Generates per-model Excel reports with prediction intervals at multiple significance levels

### 3️⃣ Conformal Predictions — NEXCP, Adaptive CP & mfcs

> 📓 **Notebook:** [`Conformal_Predictions(NEXCP, Adaptive CP, mfcs).ipynb`](Conformal_Predictions(NEXCP,AdaptiveCP,mfcs)/Conformal_Predictions(NEXCP,%20Adaptive%20CP,%20mfcs).ipynb)

- Implements **NEXCP** (Normalized Exchangeable Conformal Prediction)  
- Implements **Adaptive Conformal Prediction** for heteroscedastic data  
- Implements **mfcs** (Model-Free Conformal Sets)  
- Per-model Excel reports with calibrated intervals

### 4️⃣ Probabilistic Distribution

> 📓 **Notebook:** [`Probabilistic__Distribution.ipynb`](Probabilistic_Distribution/Probabilistic__Distribution.ipynb)

- Fits full predictive distributions (e.g., Gaussian, Student-t) to model outputs  
- Includes standard and **TF (TensorFlow)** variants for select models  
- Evaluation metrics consolidated in `Matrix Evaluation.xlsx`

### 5️⃣ Probabilistic Distribution — CARD

> 📓 **Notebook:** [`Probabilistic__Distribution(CARD).ipynb`](Probabilistic_Distribution(CARD)/Probabilistic__Distribution(CARD).ipynb)

- Uses the **CARD** (Classification And Regression Diffusion) framework  
- Generates conditional diffusion-based predictive distributions  
- Per-model evaluation stored in `Matrix Evaluation.xlsx`

### 6️⃣ Quantile Regression

> 📓 **Notebook:** [`Quantile_Regression.ipynb`](Quantile_Regression/Quantile_Regression.ipynb)

- Directly estimates conditional quantiles (e.g., 5th, 50th, 95th percentiles)  
- Produces prediction intervals without distributional assumptions  
- Per-model Excel reports with quantile-based intervals

---

## 🚀 Getting Started

### Prerequisites

```bash
# Core ML libraries
pip install xgboost lightgbm catboost scikit-learn
pip install ngboost pgbm pytorch-tabnet gpboost

# Hyperparameter tuning
pip install optuna

# Conformal prediction
pip install mapie puncc

# Probabilistic & utility
pip install numpy pandas matplotlib seaborn openpyxl
```

### Usage

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/ML_Analysis_Pile_Settlement.git
   cd ML_Analysis_Pile_Settlement
   ```

2. **Start with Hyperparameter Tuning** — Run [`Optuna_autosampler.ipynb`](HyperParameter_Tuning/Optuna_autosampler.ipynb) to tune all models and save the best checkpoints.

3. **Run any UQ module** — Open the corresponding notebook in `Conformal_Predictions*/`, `Probabilistic_Distribution*/`, or `Quantile_Regression/` to generate uncertainty-aware predictions.

4. **Review results** — Each module produces per-model `.xlsx` files containing predictions, intervals, and evaluation metrics.

---

## 📁 Output Files

Each UQ module produces per-model Excel files with the following general structure:

| Column          | Description                            |
|:----------------|:---------------------------------------|
| `y_true`        | Ground truth settlement values         |
| `y_pred`        | Model point predictions                |
| `lower_bound`   | Lower prediction interval bound        |
| `upper_bound`   | Upper prediction interval bound        |
| Metrics         | PICP, MPIW, RMSE, R², etc.            |

---


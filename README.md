# Predictive Analysis of Global Temperature Trends Using Machine Learning Algorithms
### The Region of South America

> Master's Degree Project in Computer and Systems Sciences — Stockholm University, Spring 2025  
> **Author:** Puja Poudel &nbsp;|&nbsp; **Supervisor:** Chantal Mutimukwe

---

## Overview

This project develops a machine learning framework for forecasting regional temperature trends across South America using integrated satellite and ground-based datasets. Three time-series models — **XGBoost**, **Prophet**, and **Exponential Smoothing (ETS)** — are trained on historical land surface temperature (LST) data spanning 1980–2024 across 12 South American countries.

The study addresses a critical gap in climate science: most existing forecasting models prioritise global or continental trends and overlook the localised variability driven by South America's diverse geography (Amazon Basin, Andean highlands, Southern Cone).

---

## Research Question

> *How effectively can time series machine learning models forecast regional temperature trends in South America when trained on integrated historical satellite and ground-based datasets?*

---

## Repository Structure

```
.
├── exponential-smoothing.ipynb   # ETS (Holt-Winters) forecasting model
├── prophet-tempdata.ipynb        # Facebook Prophet forecasting model
├── stl-linear-regression.ipynb   # STL decomposition + Linear Regression (baseline)
├── xgboost.ipynb                 # XGBoost forecasting model
├── requirements.txt              # Python dependencies
├── .gitignore
└── README.md
```

---

## Data

| Source | Format | Description |
|---|---|---|
| [NASA Earth Observing System (EOS)](https://earthdata.nasa.gov) | NetCDF / HDF | High-resolution satellite LST at 1 km² via MODIS |
| [Berkeley Earth](https://berkeleyearth.org) | TXT | Ground-station historical temperature records |

- **Countries covered:** Argentina, Bolivia, Brazil, Chile, Colombia, Ecuador, Guyana, Paraguay, Peru, Suriname, Uruguay, Venezuela
- **Temporal range:** January 1980 – December 2024 (monthly averages, °C)
- **Total records:** 6 336 rows per source

The integrated dataset (`ProductionDataAll.csv`) follows a medallion architecture (Bronze → Silver → Gold layers) for cleaning and standardisation before model training.

---

## Models

### 1. XGBoost (`xgboost.ipynb`)
Gradient-boosted decision-tree ensemble. Best suited for capturing non-linear long-term warming trajectories.

Key hyperparameters:
- `n_estimators = 200`, `learning_rate = 0.2`, `max_depth = 7`
- `subsample = 0.8`, `colsample_bytree = 0.8`

### 2. Prophet (`prophet-tempdata.ipynb`)
Facebook's decomposable time-series model. Excels at isolating seasonal components (dry/wet cycles, annual patterns).

### 3. Exponential Smoothing — ETS (`exponential-smoothing.ipynb`)
Holt-Winters triple exponential smoothing. Highest accuracy for short-term seasonal forecasting.

### 4. STL + Linear Regression (`stl-linear-regression.ipynb`)
Seasonal-Trend decomposition using LOESS as a preprocessing step, followed by linear regression on extracted trend and seasonal features. Serves as an interpretable baseline.

---

## Results

### Model Performance (test set 2020–2024)

| Model | MAE (°C) | RMSE (°C) | MAPE (%) |
|---|---|---|---|
| XGBoost | 0.623 | 1.041 | 3.187 |
| Prophet | 1.530 | 2.362 | 9.796 |
| Exponential Smoothing | **0.504** | **0.670** | **2.838** |

- **XGBoost** is preferred for long-term forecasting due to its ability to model complex non-linear trends (consistent projected increase of ~0.04 °C/year through 2029).
- **ETS** achieves the lowest error metrics overall and is ideal for short-term seasonal planning.
- **Prophet** provides the most interpretable seasonal decomposition despite higher aggregate error.

### Regional Findings (1980–2024)

| Region | Key Finding | Primary Driver |
|---|---|---|
| Amazon Basin | +2.1 °C in dry season since 2000 | Deforestation, land-use change |
| Andean Region | Slower warming (~+0.02 °C/year) | Altitude influence |
| Southern Cone | Winter warming faster than summer (+0.04 °C/year) | Precipitation cycle disruption |

Overall continental average: **+1.2 °C** since 1980.

---

## Getting Started

### Prerequisites

Python 3.9+ is recommended.

```bash
pip install -r requirements.txt
```

### Running the Notebooks

The notebooks were originally developed on [Kaggle](https://www.kaggle.com). To run them locally, update the data path in each notebook:

```python
# Replace this Kaggle path:
data = pd.read_csv('/kaggle/input/newdata/ProductionDataAll - Copy.csv')

# With your local path, e.g.:
data = pd.read_csv('data/ProductionDataAll.csv')
```

Then launch Jupyter:

```bash
jupyter notebook
```

Run notebooks in the following order for a complete reproduction:
1. `stl-linear-regression.ipynb` (baseline / exploratory)
2. `exponential-smoothing.ipynb`
3. `prophet-tempdata.ipynb`
4. `xgboost.ipynb`

---

## Tech Stack

| Tool | Role |
|---|---|
| Python / Jupyter | Modelling & analysis |
| pandas, NumPy | Data manipulation |
| scikit-learn | Metrics, scaling, linear regression |
| statsmodels | ETS (Holt-Winters), STL decomposition |
| Prophet | Seasonal forecasting |
| XGBoost | Gradient boosting regression |
| Matplotlib | Visualisation |
| Microsoft Fabric | ETL pipeline / data lake |
| Power BI | Interactive dashboards |

---

## Limitations

- Analysis is limited to land surface temperature; precipitation, humidity, and CO₂ emissions are excluded.
- Country-level aggregation may mask sub-national microclimatic variation.
- Deep learning models (e.g., LSTM) were excluded to maintain interpretability and computational feasibility.

---

## Future Work

- Integrate exogenous variables (CO₂ emissions, deforestation rates, precipitation).
- Explore hybrid and ensemble models combining XGBoost and ETS.
- Extend the framework to other geographically diverse regions.
- Incorporate real-time satellite data streams for dynamic forecasting.

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## Citation

If you use this work, please cite:

```
Poudel, P. (2025). Predictive Analysis of Global Temperature Trends Using Machine
Learning Algorithms: The Region of South America. Master's Degree Project,
Department of Computer and Systems Sciences, Stockholm University.
```

# ✈️ Flight Delay Prediction at DFW Airport
### A Machine Learning Capstone Project

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python)
![Sklearn](https://img.shields.io/badge/Scikit--Learn-1.3+-orange?style=for-the-badge&logo=scikit-learn)
![XGBoost](https://img.shields.io/badge/XGBoost-Latest-red?style=for-the-badge)
![Pandas](https://img.shields.io/badge/Pandas-Latest-150458?style=for-the-badge&logo=pandas)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter)

---

## 📌 Project Overview

This capstone project builds a **binary classification system** to predict whether a flight departing from **Dallas Fort Worth International Airport (DFW)** will arrive delayed by **10 or more minutes**.

We compare **4 machine learning models** across **2 phases**:
- 🔵 **Phase 1** — Modelling WITHOUT weather data
- 🟠 **Phase 2** — Modelling WITH weather data (temperature & wind speed)

The goal is to identify the best performing model and determine whether weather data adds predictive value beyond operational flight features.

---

## 🎯 Key Results

| Model | ROC-AUC (No Weather) | ROC-AUC (With Weather) |
|---|---|---|
| Logistic Regression | 0.951 | 0.947 |
| Decision Tree | 0.947 | 0.947 |
| Random Forest | 0.959 | 0.955 |
| **XGBoost** | **0.963** ✅ | **0.961** |

> 🏆 **XGBoost without weather data** is the best model with **ROC-AUC = 0.963** and **F1 = 0.933**

---

## 📂 Project Structure

```
flight-delay-prediction-dfw/
│
├── 📓 Capstone_apr_18.ipynb          # Main Jupyter Notebook
│
├── 📊 data/
│   ├── Capstone_Project_Data_Set_2.csv    # Flight data (182,906 rows)
│   ├── Capstone_Project_Data_Set_2.xlsx   # Airport reference data (4,565 airports)
│   └── dfw_2021_2025_all_years.csv        # NOAA weather data (DFW, 2021-2025)
│
├── 📈 outputs/
│   ├── model_comparison_no_weather.png
│   ├── model_comparison_with_weather.png
│   ├── no_weather_vs_with_weather.png
│   ├── feature_importance_no_weather.png
│   ├── feature_importance_with_weather.png
│   ├── combined_roc_curves.png
│   └── confusion_matrices_all.png
│
└── 📄 README.md
```

---

## 🗂️ Datasets

### 1. Flight Data
- **Source:** American Airlines internal hub operations data
- **Size:** 182,906 rows × 64 columns
- **Coverage:** DFW airport departures, 2021–2025
- **Key columns:** Flight number, airline, scheduled/actual times, arrival variance, departure status

### 2. Airport Data
- **Source:** Internal airport reference dataset
- **Size:** 4,565 airports worldwide
- **Key columns:** Airport code, runway length (ft), elevation (ft)

### 3. Weather Data
- **Source:** [NOAA Integrated Surface Database (ISD)](https://www.ncei.noaa.gov/products/land-based-station/integrated-surface-database)
- **Coverage:** Hourly observations at DFW, 2021–2025
- **Key columns:** Temperature (°C), Wind Speed (m/s)

---

## 🎯 Target Variable

```
Delay Threshold (Professor's Assumption): Arrival Variance >= 10 minutes

is_delayed = 1  →  Flight arrived 10+ minutes late  (DELAYED)
is_delayed = 0  →  Flight arrived less than 10 minutes late  (ON TIME)
```

---

## ⚙️ Data Preprocessing Pipeline

```
Raw Flight Data (182,906 rows)
        ↓
Filter ETD events only
        ↓
Keep last update per flight (one row per flight)
        ↓
Create unique Flight ID
        ↓
Merge departure airport features (runway, elevation)
        ↓
Merge arrival airport features (runway, elevation)
        ↓
Extract time features (hour, month, day of week)
        ↓
Label encode (airline, departure status)
        ↓
Parse NOAA weather fields (TMP, WND)
        ↓
Clean weather outliers (sensor error codes)
        ↓
Aggregate weather hourly
        ↓
merge_asof → match flights to nearest weather hour (100% match)
        ↓
Impute missing values (median strategy)
        ↓
Final Dataset: 33,598 flights ready for modelling
```

---

## 🧪 Feature Set

### Phase 1 — Without Weather (11 features)

| Feature | Description |
|---|---|
| `dep_variance` | Departure delay in minutes |
| `mins_to_schd_dep` | Minutes to scheduled departure at time of update |
| `dep_status_encoded` | Encoded departure status (ETD, CYCLE etc.) |
| `airline_encoded` | Encoded airline IATA code |
| `dep_hour` | Scheduled departure hour (0–23) |
| `dep_month` | Departure month (1–12) |
| `dep_dayofweek` | Day of week (0=Monday, 6=Sunday) |
| `dep_runway_length` | Departure airport longest runway (ft) |
| `dep_elevation` | Departure airport elevation (ft) |
| `arr_runway_length` | Arrival airport longest runway (ft) |
| `arr_elevation` | Arrival airport elevation (ft) |

### Phase 2 — With Weather (13 features)
All 11 above + 2 weather features:

| Feature | Description |
|---|---|
| `temperature` | Hourly temperature at DFW (°C) |
| `wind_speed` | Hourly wind speed at DFW (m/s) |

---

## 🤖 Models

| Model | Key Parameters |
|---|---|
| Logistic Regression | `max_iter=1000, class_weight=balanced` |
| Decision Tree | `max_depth=8, class_weight=balanced` |
| Random Forest | `n_estimators=100, max_depth=8, class_weight=balanced` |
| XGBoost | `n_estimators=100, max_depth=6, learning_rate=0.1` |

> **Class Imbalance:** Handled using `class_weight=balanced` and `scale_pos_weight` for XGBoost

---

## 📊 Model Comparison

### Without Weather vs With Weather
![Model Comparison](outputs/no_weather_vs_with_weather.png)

### ROC Curves
![ROC Curves](outputs/combined_roc_curves.png)

### Feature Importance (No Weather)
![Feature Importance No Weather](outputs/feature_importance_no_weather.png)

### Feature Importance (With Weather)
![Feature Importance With Weather](outputs/feature_importance_with_weather.png)

---

## 💡 Key Findings

1. **Departure variance is the #1 predictor** — if a flight departs late it almost always arrives late
2. **XGBoost is the best model** — ROC-AUC of 0.963 without weather data
3. **Weather did not improve performance** — adding temperature and wind speed slightly decreased scores across all models
4. **Why weather didn't help:**
   - Departure variance already captures weather-related disruptions
   - Only departure airport weather was available — no arrival airport or en-route data
   - DFW has a relatively stable climate compared to northeast US airports
5. **All models scored ROC-AUC > 0.94** — flight delay at DFW is a well-learnable problem

---

## 🔍 Why Weather Didn't Help — Explained

```
Weather causes delay
       ↓
Flight departs late
       ↓
dep_variance captures this
       ↓
Model already knows about the delay
       ↓
Adding weather = redundant information
```

---

## 🚀 How to Run

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/flight-delay-prediction-dfw.git
cd flight-delay-prediction-dfw
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost openpyxl
```

### 3. Add data files
Place these files in the project root:
- `Capstone Project Data Set 2.csv`
- `Capstone Project Data Set 2.xlsx`
- `dfw_2021_2025_all_years.csv`

### 4. Run in Google Colab (Recommended)
```
1. Upload notebook to Google Colab
2. Upload all 3 data files to Colab session
3. Run all cells top to bottom
```

### 5. Or run locally with Jupyter
```bash
jupyter notebook Capstone_apr_18.ipynb
```

---

## 📦 Dependencies

```python
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
openpyxl
jupyter
```

Install all at once:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost openpyxl jupyter
```

---

## 📈 Evaluation Metrics Used

| Metric | Why We Used It |
|---|---|
| **Accuracy** | Overall correct predictions |
| **Precision** | Of predicted delays, how many were real |
| **Recall** | Of real delays, how many we caught |
| **F1 Score** | Balance of precision and recall |
| **ROC-AUC** | Best metric for imbalanced classification |

---

## ⚠️ Limitations

- Weather data only from DFW departure airport — no arrival airport or en-route weather
- Dataset covers 2021–2025 including COVID recovery period
- No air traffic control, gate, or turnaround time data available
- Only 2 weather variables used — precipitation and visibility could add value
- Static model — no real-time prediction pipeline


---

## 👨‍💻 Author

Vaishnavi Bhamare

---

## 🏫 Academic Info

- **Course:** Capstone Project - ADTA 5980
- **University:** University of North Texas
- **Semester:** Spring 2025

---

## 📄 License

This project is for academic purposes only.

---

<p align="center">
  Made for the Capstone Project
  <br>
  ✈️ DFW Airport | 2021–2025 | Machine Learning
</p>

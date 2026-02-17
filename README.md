[hybrid_sales_forecasting_readme.md](https://github.com/user-attachments/files/25354848/hybrid_sales_forecasting_readme.md)
# Sales Forecasting of Reliance Industries Using Hybrid Deep Learning

## Project Overview
This project implements a **Hybrid Deep Learning Forecasting System** to predict Reliance Industries’ stock/sales trends using historical time‑series data. The architecture combines **LSTM (trend learner)** and **GRU (residual learner)** under a residual learning framework to improve predictive accuracy over standalone models.

---

# System Architecture

## 1. End‑to‑End Pipeline

```
Raw Time Series Data
        ↓
Data Preprocessing & Scaling
        ↓
Sliding Window Sequence Generation
        ↓
LSTM Model (Trend Learning)
        ↓
Residual Extraction (Actual − LSTM Prediction)
        ↓
GRU Model (Residual Learning)
        ↓
Trend + Residual Summation
        ↓
Final Hybrid Forecast
```

---

## 2. Data Layer

**Dataset Source:** Yahoo Finance  
**Duration:** ~10 Years  
**Granularity:** Daily  

**Features Used:**
- Open
- High
- Low
- Close (Target)
- Volume

**Preprocessing Steps:**
1. Date parsing & indexing
2. Missing value handling (forward fill)
3. Normalization (MinMax Scaling)
4. Sequential split (70–30)
5. Sliding window look‑back sequence generation

---

## 3. Phase‑1 — LSTM Trend Model

**Objective:** Capture long‑term trend & low‑frequency movement.

**Architecture:**
- LSTM Layer — 64 Units — return_sequences=True
- LSTM Layer — 64 Units — return_sequences=False
- Dense Output Layer — 1 Unit

**Configuration:**
- Optimizer: Adam
- Loss: Mean Squared Error (MSE)
- Input: Look‑back sequences

**Learning Outcome:**
- Captures macro‑trend
- Struggles with sudden spikes

---

## 4. Residual Learning Layer

Residuals are extracted as:

```
Residual_t = Actual_t − Prediction_LSTM_t
```

**Purpose:**
- Identify unexplained variance
- Capture structured error patterns
- Convert forecasting into a two‑stage correction problem

---

## 5. Phase‑2 — GRU Residual Model

**Objective:** Learn high‑frequency fluctuations left by LSTM.

**Why GRU:**
- Faster convergence
- Fewer parameters
- Efficient for residual sequences

**Input:** Residual time series  
**Output:** Predicted correction term

---

## 6. Hybrid Forecast Formation

Final prediction is computed as:

```
Final Forecast_t = LSTM_Prediction_t + GRU_Residual_Prediction_t
```

This additive correction reduces lag and improves alignment with actual prices.

---

# Model Performance

| Model | MAE | RMSE |
|------|------|------|
| Stacked LSTM | 31.55 | 39.28 |
| Hybrid LSTM‑GRU | 14.71 | 19.81 |

**Inference:** Hybrid architecture significantly reduces prediction error.

---

# Why Test Split Was Not Used

Your implementation uses:
- Training Set — 70%
- Validation Set — 30%

### Technical Reasoning

1. **Time‑Series Sequential Constraint**
   Random test splitting causes data leakage (future predicting past).

2. **Validation as Proxy Test**
   The 30% holdout segment functions as an out‑of‑sample evaluation window.

3. **Academic Prototype Scope**
   Many research prototypes validate on a single forward split rather than tri‑split.

4. **Residual Pipeline Dependency**
   Residual generation requires continuous sequences; excessive splitting complicates alignment.

---

# Limitations / Incomplete Components

## 1. No Independent Test Set
- No final blind evaluation
- Generalization not fully proven

## 2. No Walk‑Forward Validation
- Rolling forecasting origin not implemented
- Real trading simulation missing

## 3. Hyperparameter Optimization Missing
- No grid search / Bayesian tuning
- Fixed units, epochs, look‑back

## 4. Feature Engineering Limited
- Only OHLCV used
- No technical indicators (RSI, MACD, EMA)

## 5. No Exogenous Variables
- No macroeconomic data
- No sector indices

## 6. No Sentiment Integration
- News / social media signals absent

## 7. Deployment Layer Missing
- No API or dashboard
- Not productionized

## 8. Risk Metrics Not Evaluated
- No directional accuracy
- No Sharpe ratio / trading backtest

---

# Suggested Improvements

1. Add **Train–Validation–Test** (60‑20‑20 sequential)
2. Implement **Walk‑Forward Backtesting**
3. Integrate **Technical Indicators**
4. Add **News Sentiment (NLP)**
5. Try **Attention / Transformers**
6. Deploy via **Streamlit / Flask**

---

# Tech Stack

- Python
- TensorFlow / Keras
- Pandas / NumPy
- Matplotlib
- Google Colab

---

# Research Contribution

The project demonstrates that decomposing forecasting into:

- Trend Learning (LSTM)
- Error Correction (GRU)

produces superior accuracy versus monolithic deep learning models.

---

# Citation

If used academically, cite as:

**"Sales Forecasting of Reliance Industries Using Hybrid LSTM‑GRU Residual Learning."**

---

# Author

Suman Banerjee  
B.Tech CSE — University of Kalyani


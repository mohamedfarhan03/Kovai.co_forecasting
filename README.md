# 🚍 Daily Public Transport Passenger Forecasting using SARIMAX

## 📘 Overview
This project performs **time series forecasting** on public transport passenger journey data using the **SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors)** model.  
The goal is to analyze patterns, detect trends, and predict passenger counts for various **transportation services** such as Local Routes, Light Rail, Peak Services, Rapid Routes, School, and Other services.

The project focuses on producing **interpretable insights** and **realistic forecasts** that respect weekday/weekend behavior, seasonality, and transport-specific logic (e.g., schools having no passengers on weekends).

---

## ⚙️ Model Used

### **SARIMAX (Statsmodels Implementation)**
SARIMAX is a powerful extension of ARIMA that handles:
- **Seasonality** (e.g., weekly or monthly travel patterns)
- **Autoregressive components** (dependence on past values)
- **Moving average effects** (dependence on past errors)
- **Integrated differencing** to make data stationary
- Optional support for **exogenous regressors** (not used here, but extensible)

Each service (e.g., *Light Rail, Local Route, School*) is modeled independently using **fine-tuned SARIMAX parameters** (p, d, q, P, D, Q, s), optimized using AIC-based selection.

---

## 🧹 Data Preprocessing

To ensure accurate and stable forecasting, the dataset undergoes multiple cleaning and preparation steps:

1. **Missing Value Handling:**  
   Missing dates and passenger counts are filled using **time-based interpolation**.

2. **Outlier Removal:**  
   Outliers are **capped using the IQR (Interquartile Range)** method to prevent abnormal spikes from affecting forecasts.

3. **Negative Value Correction:**  
   Any negative passenger counts are replaced with 0.

4. **Weekend Adjustments:**  
   - For **School** services: values are set to **0 on weekends**.  
   - For all other services: minimum passenger floors are applied to avoid unrealistic zeros on weekends.

---

## 📊 Forecasting Steps

1. **Train-Test Split:**  
   - Last 30 days are used for model evaluation (MAE & RMSE).
   - The rest of the data is used for training SARIMAX.

2. **Model Fitting:**  
   - Each service is fitted using its optimized SARIMAX configuration.
   - Forecasts are generated for both **7-day** (realistic short-term) and **30-day** (extended trend) horizons.

3. **Post-processing:**
   - Negative forecasts are clipped to 0.
   - Floor values are applied for weekends and low-demand days.

4. **Evaluation Metrics:**  
   For each service, **MAE (Mean Absolute Error)** and **RMSE (Root Mean Squared Error)** are reported.

---

## 📈 Key Insights Generated

### For Each Service:
- **Trend:** Whether passenger count is increasing, decreasing, or stable.
- **Average & Median:** Central tendency of daily usage.
- **Peak Day:** Highest recorded passenger count with date.
- **Weekday/Weekend Ratio:** Compares average weekday traffic vs. weekends.
- **Recent 30-Day Change:** Percentage change between the last 30 days and the previous 30 days.
- **Best & Worst Travel Days:** Ranked days of the week by average ridership.

### Example Output (Summarized):
| Service | Trend | Avg | Median | Peak | Wkday/Weekend Ratio | Best Day | Worst Day |
|----------|--------|------|--------|------|----------------------|-----------|------------|
| Local Route | 📈 Increasing | 9891 | 11417 | 21070 | 4.67 | Sunday | Wednesday |
| Light Rail | 📈 Increasing | 7195 | 7507 | 15154 | 1.95 | Sunday | Wednesday |
| School | 📈 Increasing | 2353 | 568 | 7255 | N/A | Weekdays | Weekends |

---

## 🗓️ Forecasting Results

### **30-Day Forecasts**
- Generated for all services using SARIMAX.
- Saved to:  
  `/content/forecast_output/30day_sarimax_forecast_selected_services.csv`
- Combined visualization saved to:  
  `/content/forecast_output/combined_sarimax_forecast_30days.png`

### **7-Day Realistic Forecasts**
- Adjusted for real-world patterns (school closed on weekends, etc.).
- Saved to:  
  `/content/forecast_output/7day_realistic_forecast.csv`

---

## 🧠 Insights Highlighted
✅ Identifies **trends** across services (growth or decline).  
✅ Quantifies **weekday vs. weekend behavior**.  
✅ Highlights **best and worst travel days**.  
✅ Provides **error evaluation metrics** for model accuracy.  
✅ Produces **realistic, non-negative forecasts** aligned with domain logic.

---

## 📦 Repository Structure

```
📁 transport_forecasting_sarimax/
│
├── 📄 README.md                        ← Project documentation (this file)
├── 📄 forecasting_sarimax.ipynb        ← Main Colab notebook
├── 📄 Daily_Public_Transport_Passenger_Journeys_by_Service_Type_20250603.csv
│
├── 📁 forecast_output/
│   ├── 30day_sarimax_forecast_selected_services.csv
│   ├── 7day_realistic_forecast.csv
│   ├── combined_sarimax_forecast_30days.png
│   ├── sarimax_full_history_forecast_*.png
│   └── sarimax_recent_changes_forecast_*.png
```

---

## 📚 Dependencies

```bash
pip install pandas numpy matplotlib statsmodels scikit-learn
```

---

## 🧾 Summary

| Aspect | Description |
|---------|--------------|
| **Model Used** | SARIMAX (Statsmodels) |
| **Forecast Horizon** | 7 days (short-term), 30 days (extended) |
| **Data Cleaning** | Missing value interpolation, IQR outlier capping |
| **Weekend Logic** | School = 0 on weekends, others adjusted realistically |
| **Insights Provided** | Trends, weekday/weekend ratios, top/bottom days, recent changes |
| **Outputs** | CSVs + Plots for all services |

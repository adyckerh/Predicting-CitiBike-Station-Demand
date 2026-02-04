# Forecasting Citi Bike Station Demand to Improve Efficiency of Operational Rebalancing

## Overview

Every morning and evening, Citi Bike users face empty docks in residential areas or full stations at destinations. This project forecasts bike demand at 15-minute intervals to optimize rebalancing operations during rush hours in Manhattan and Brooklyn.

**Key Result:** Our Random Forest model achieves **R² = 0.96** on cluster-level predictions, capturing 96% of variance in neighborhood bike flow.

## Approach

### Data
- **4.6M trips** from August 2024 (Citi Bike)
- Hourly weather data (temperature, precipitation, wind)
- NYC borough boundaries
- Filtered to weekday rush hours: 7–10 AM and 5–8 PM

### Target Variable
**Net flow** per 15-minute interval = incoming rides − outgoing rides

### Geographic Clustering
To reduce station-level noise, we clustered 1,400+ stations into **101 neighborhood clusters** using K-Means with a 0.9 km Haversine distance threshold. This compressed the dataset by 88.7% while retaining spatial granularity.

### Models Evaluated
| Model | Validation R² | Notes |
|-------|---------------|-------|
| Linear Regression | 0.21 | Baseline |
| ARIMA | — | Poor fit due to fragmented rush-hour data |
| SARIMAX | — | Limited by univariate approach |
| XGBoost | 0.69 | Station-level |
| **Random Forest** | **0.68 → 0.96** | Station-level → Cluster-level |

### Key Features
Temporal features dominated predictive power:
- Rolling means (4-interval window)
- Cumulative lag features (1, 2, 4 intervals)
- Rush hour indicators
- Historical station activity

Weather had negligible impact—likely due to single-month (August) data lacking seasonal variation.

## Results

The cluster-level Random Forest model achieved:
- **R² = 0.96** (validation), **0.90** (test)
- **RMSE = 16.74 bikes** per cluster per 15-min interval
- **MAE = 9.69 bikes**

![Actual vs Predicted](https://github.com/michaeltwersky/predicting-citi-bike-station-demand/blob/main/images/7.jpg)

## Data Sources

Download from the [v1.0 release page](https://github.com/michaeltwersky/applied-ml-project/releases/tag/data):

| File | Description |
|------|-------------|
| `202408-citibike-tripdata_[1-5].csv` | Trip data for August 2024 |
| `weather_data_2024.csv` | NYC hourly weather |
| `Borough_boundaries_20251104.csv` | NYC borough boundaries |

**Sources:**
- [Citi Bike System Data](https://s3.amazonaws.com/tripdata/index.html)
- [Visual Crossing Weather](https://www.visualcrossing.com/weather-query-builder/)
- [NYC Open Data](https://data.cityofnewyork.us/City-Government/Borough-Boundaries/tqmj-j8zm)

## Tools

**Core:** Python, Pandas, NumPy, Scikit-learn, XGBoost

**Time Series:** ARIMA, SARIMAX (statsmodels)

**Visualization:** Plotly, Matplotlib, Seaborn

## Future Work

- Expand to multi-season data to capture weather effects
- Incorporate special events (concerts, sports games)
- Explore deep learning architectures (RNNs, transformers)

## Visualizations

<p align="center">
  <img src="https://github.com/michaeltwersky/predicting-citi-bike-station-demand/blob/main/images/5.jpg" width="45%">
  <img src="https://github.com/michaeltwersky/predicting-citi-bike-station-demand/blob/main/images/6.jpg" width="45%">
</p>
<p align="center">
  <img src="https://github.com/michaeltwersky/predicting-citi-bike-station-demand/blob/main/images/3.jpg" width="45%">
  <img src="https://github.com/michaeltwersky/predicting-citi-bike-station-demand/blob/main/images/4.jpg" width="45%">
</p>
<p align="center">
  <img src="https://github.com/michaeltwersky/predicting-citi-bike-station-demand/blob/main/images/8.jpg" width="60%">
</p>

## Authors

- **Michael Twersky** (mdt93) — Cornell Tech
- **Alexander Dyckerhoff** (ajd343) — Cornell Tech

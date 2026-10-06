M5 Retail Demand Forecasting

A machine learning project for next-day retail demand forecasting using the ⁠M5 Forecasting dataset.

The project uses historical sales, product/store information, calendar data, events, SNAP indicators, and pricing information to predict the next day’s demand for individual retail products.

Project Objective

Build a practical demand forecasting model that can predict next-day product sales while handling the large size of the M5 dataset on a memory-constrained machine.

Tech Stack

* Python
* Pandas / NumPy
* Dask
* LightGBM
* Scikit-learn
* Jupyter Notebook
* Parquet

Approach

1. Data Processing

The M5 dataset is large, so the data pipeline processes the data in chunks rather than loading the entire dataset into memory.

The processed data is stored as Parquet files for efficient storage and loading.

2. Feature Engineering

The model uses several categories of features:

* Historical demand lags: lag_1, lag_7, lag_14, lag_21, lag_28
* Rolling demand statistics
* Demand trends and momentum
* Zero-demand rates
* Days since last sale
* Calendar features
* Event indicators
* SNAP indicators
* Selling price and price-related features
* Price × event interactions

All demand-based rolling features are calculated using previous observations to avoid target leakage.

3. Model

A LightGBM regression model was trained using the engineered features.

The model was trained incrementally across data chunks to keep memory usage manageable.

Results

The model was evaluated against a simple lag-7 baseline, which predicts today’s demand using demand from the same day of the previous week.

Model

RMSE

MAE

MSE

Lag-7 Baseline

2.6601

1.2054

7.0762

LightGBM

1.9933

0.9859

3.973

The LightGBM model achieved approximately a 25% reduction in RMSE compared with the lag-7 baseline.

The validation set contained 853,720 observations.

Feature Importance

The most influential features included:

1. rolling_mean_7
2. lag_1
3. item_id
4. rolling_mean_28
5. rolling_std_7
6. trend_1_7
7. lag_7
8. event_name_1
9. lag_7_x_snap
10. lag_21

The importance of recent demand and rolling statistics indicates that short-term demand history is particularly useful for next-day forecasting.

Memory-Efficient Processing

The project was developed on a machine with 8 GB RAM.

Instead of loading the entire M5 dataset into memory, the pipeline uses:

* Chunk-wise processing
* Parquet files
* History buffers for lag features
* Explicit memory cleanup
* Reduced numerical data types where appropriate

This allowed the forecasting pipeline to operate on the large M5 dataset without requiring a high-memory machine.

Dataset

The project uses the M5 Forecasting - Accuracy dataset provided by Walmart and hosted on Kaggle.

The dataset is not included in this repository due to its size.

License

This project is intended for educational and portfolio purposes.

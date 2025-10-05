# MarketReturn_SP500ExcessPredictor
## Overview

SP500 Excess Predictor is a machine learning project designed to predict excess returns of the S&P 500 index. The project leverages historical market data and engineered features to generate predictive signals. The primary goal is to create a robust predictive model that can inform investment decisions.

This repository contains code, data processing scripts, model training, and evaluation pipelines.

## Data

The project uses two main files: `train.csv` and `test.csv`.

### train.csv
Historic market data. The coverage stretches back decades; expect to see extensive missing values early on.

**Columns:**
- `date_id` - An identifier for a single trading day.  
- `M*` - Market Dynamics / Technical features.  
- `E*` - Macro Economic features.  
- `I*` - Interest Rate features.  
- `P*` - Price / Valuation features.  
- `V*` - Volatility features.  
- `S*` - Sentiment features.  
- `MOM*` - Momentum features.  
- `D*` - Dummy / Binary features.  
- `forward_returns` - Returns from buying the S&P 500 and selling it a day later. Train set only.  
- `risk_free_rate` - Federal funds rate. Train set only.  
- `market_forward_excess_returns` - Forward returns relative to expectations, computed by subtracting the rolling five-year mean forward returns and winsorizing the result using median absolute deviation (MAD) with a criterion of 4. Train set only.  

### test.csv
A mock test set representing the structure of the unseen test set. The test set used for the public leaderboard is a copy of the last 180 date IDs in the train set. Public leaderboard scores are not meaningful. The unseen copy served by the evaluation API may be updated during model training.

**Columns:**
- `date_id` - Identifier for the trading day.  
- `[feature_name]` - Feature columns same as in `train.csv`.  
- `is_scored` - Indicates whether this row is included in evaluation metric calculation. True for the first 180 rows only. Test set only.  
- `lagged_forward_returns` - Returns from buying the S&P 500 and selling it a day later, provided with a lag of one day.  
- `lagged_risk_free_rate` - Federal funds rate, provided with a lag of one day.  
- `lagged_market_forward_excess_returns` - Forward returns relative to expectations, computed by subtracting the rolling five-year mean forward returns and winsorizing the result using MAD with a criterion of 4, provided with a lag of one day.

## Methodology

### 1. Feature Engineering
- Select relevant features from raw data.  
- Normalize features to improve model stability and interpretability.

### 2. Model Training
- Use **ElasticNetCV** to automatically tune `alpha` and `l1_ratio` via cross-validation.  
- Fit the model on scaled training data.  
- Save the trained model for inference.

### 3. Prediction & Signal Generation
- Apply the trained **ElasticNet** or **ElasticNetCV** model to test data.  
- Predict excess returns for the S&P 500.  
- Convert predicted returns into trading signals (+1 for long, -1 for short, 0 for neutral).

### 4. Evaluation
- Compare predicted signals with actual S&P 500 returns.  
- Plot cumulative returns over time.  
- Calculate performance metrics such as accuracy, Sharpe ratio, or cumulative return.

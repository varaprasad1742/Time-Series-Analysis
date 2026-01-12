# Time Series Analysis - Complete Guide

A comprehensive repository for understanding and implementing time series analysis techniques, from fundamental concepts to advanced forecasting models.

## Table of Contents
- [Overview](#overview)
- [Key Concepts & Definitions](#key-concepts--definitions)
- [Repository Contents](#repository-contents)
- [Getting Started](#getting-started)
- [Prerequisites](#prerequisites)
- [Important Terms Reference](#important-terms-reference)
- [Resources](#resources)

## Overview

Time series analysis is a statistical technique used to analyze time-ordered data points to extract meaningful statistics and identify patterns. This repository provides practical implementations and explanations of various time series concepts and modeling techniques.

## Key Concepts & Definitions

### 1. Time Series Data
**Definition:** A sequence of data points collected or recorded at successive points in time, typically at uniform intervals.

**Example:** Daily stock prices, monthly sales figures, yearly temperature records, hourly website traffic.

**Key Characteristics:**
- Temporal ordering (order matters)
- Regular or irregular intervals
- Can be univariate (single variable) or multivariate (multiple variables)

### 2. Components of Time Series

#### **Trend**
**Definition:** The long-term increase or decrease in the data over time. It represents the underlying direction in which the data is moving.

**Example:** 
- An upward trend in global temperatures over decades
- A company's sales increasing year over year
- Population growth over time

#### **Seasonality**
**Definition:** Regular, periodic fluctuations in data that occur at fixed intervals (daily, weekly, monthly, quarterly, or yearly).

**Example:**
- Ice cream sales peaking in summer months
- Retail sales spiking during holiday seasons
- Electricity usage patterns varying by time of day
- Hotel bookings increasing during vacation periods

#### **Residual (Noise)**
**Definition:** Random, irregular fluctuations in the data that cannot be attributed to trend or seasonality. Also called "random error" or "white noise."

**Example:**
- Unexpected weather events affecting sales
- Random market fluctuations
- Measurement errors

### 3. Time Series Decomposition

**Definition:** The process of separating a time series into its constituent components: trend, seasonality, and residual.

#### **Additive Model**
**Formula:** `Value = Trend + Seasonality + Residual`

**When to Use:**
- When seasonal variations are roughly constant over time
- When the data does not exhibit exponential growth
- When seasonal fluctuations have similar magnitude throughout the series

**Example:** Monthly sales where the peak season adds a constant $10,000 regardless of the base level.

#### **Multiplicative Model**
**Formula:** `Value = Trend × Seasonality × Residual`

**When to Use:**
- When seasonal variations increase or decrease proportionally with the level of the series
- When the data exhibits exponential growth or variability
- When seasonal fluctuations grow larger as the trend increases

**Example:** Retail sales where holiday peaks are 50% higher than the baseline, and this 50% represents larger absolute values as overall sales grow.

### 4. Stationarity

**Definition:** A time series is stationary when its statistical properties (mean, variance, autocorrelation) remain constant over time.

**Why It Matters:** Many time series models (like ARIMA) assume stationarity. Non-stationary data can lead to unreliable predictions.

**Types of Stationarity:**

#### **Strict Stationarity**
All statistical properties are time-invariant.

#### **Weak Stationarity (Covariance Stationarity)**
- Constant mean over time
- Constant variance over time
- Covariance depends only on time lag, not on absolute time

**Testing for Stationarity:**

##### **Augmented Dickey-Fuller (ADF) Test**
**Definition:** A statistical test to determine if a time series is stationary.

**Interpretation:**
- **Null Hypothesis (H0):** The series is non-stationary (has a unit root)
- **Alternative Hypothesis (H1):** The series is stationary
- **p-value > 0.05:** Accept H0 → Data is non-stationary
- **p-value ≤ 0.05:** Reject H0 → Data is stationary

**Example Result:**
```
ADF Statistic: -4.52
p-value: 0.0002
Critical Value (5%): -2.86
Result: p-value < 0.05, therefore the series is stationary
```

**Making Data Stationary:**
1. **Differencing:** Subtract previous observation from current observation
   - First-order: `y'(t) = y(t) - y(t-1)`
   - Second-order: `y''(t) = y'(t) - y'(t-1)`
2. **Log Transformation:** Stabilize variance
3. **Detrending:** Remove trend component
4. **Seasonal Differencing:** Remove seasonality

### 5. Autocorrelation and Partial Autocorrelation

#### **Autocorrelation Function (ACF)**
**Definition:** Measures the correlation between a time series and its lagged values. Shows how observations at different time lags are related.

**Use Case:** 
- Identify the presence of autocorrelation
- Determine the order of MA (Moving Average) component in ARIMA
- Detect seasonality patterns

**Example Interpretation:**
- High ACF at lag 1: Strong correlation with previous observation
- High ACF at lag 12: Possible yearly seasonality in monthly data
- Gradual decay: Suggests AR (AutoRegressive) process

#### **Partial Autocorrelation Function (PACF)**
**Definition:** Measures the correlation between a time series and its lagged values after removing the effects of intermediate lags.

**Use Case:**
- Determine the order of AR (AutoRegressive) component in ARIMA
- Identify direct relationships at specific lags
- Avoid multicollinearity in AR models

**Example Interpretation:**
- Significant PACF at lag 1 only: AR(1) model
- Significant PACF up to lag 3: AR(3) model
- Cut-off after lag p: Suggests AR(p) process

### 6. ARIMA Models

**Definition:** AutoRegressive Integrated Moving Average - A comprehensive forecasting model that combines autoregression, differencing, and moving averages.

**Components (p, d, q):**

#### **AR (AutoRegressive) - p**
**Definition:** The current value is regressed on its own lagged values.

**Formula:** `y(t) = c + φ₁y(t-1) + φ₂y(t-2) + ... + φₚy(t-p) + ε(t)`

**Example:** AR(2) means today's value depends on the previous two days' values.

**When to Use:** When PACF shows significant spikes at specific lags.

#### **I (Integrated) - d**
**Definition:** The number of differencing operations needed to make the series stationary.

**Values:**
- d=0: Series is already stationary
- d=1: First-order differencing (most common)
- d=2: Second-order differencing (rare)

**Example:** If you need to difference the data once to make it stationary, d=1.

#### **MA (Moving Average) - q**
**Definition:** The current value depends on past forecast errors (residuals).

**Formula:** `y(t) = μ + ε(t) + θ₁ε(t-1) + θ₂ε(t-2) + ... + θₑε(t-q)`

**Example:** MA(1) means today's value depends on yesterday's forecast error.

**When to Use:** When ACF shows significant spikes at specific lags.

#### **Common ARIMA Models:**
- **ARIMA(1,0,0)**: AR(1) - random walk with drift
- **ARIMA(0,1,0)**: Random walk
- **ARIMA(0,1,1)**: Simple exponential smoothing
- **ARIMA(0,2,2)**: Damped trend exponential smoothing

### 7. SARIMA Models

**Definition:** Seasonal ARIMA - Extends ARIMA to explicitly model seasonal patterns.

**Notation:** SARIMA(p,d,q)(P,D,Q)m

Where:
- (p,d,q): Non-seasonal parameters
- (P,D,Q): Seasonal parameters
- m: Number of periods in a season

**Example:** 
- SARIMA(1,1,1)(1,1,1,12) for monthly data with yearly seasonality
- m=12 for monthly data with yearly patterns
- m=4 for quarterly data with yearly patterns
- m=7 for daily data with weekly patterns

### 8. Model Selection Criteria

#### **AIC (Akaike Information Criterion)**
**Definition:** A metric that balances model fit with model complexity. Lower AIC values indicate better models.

**Formula:** `AIC = 2k - 2ln(L)`
- k: number of parameters
- L: maximum likelihood

**Use:** Compare models; choose the one with the lowest AIC.

#### **BIC (Bayesian Information Criterion)**
**Definition:** Similar to AIC but penalizes model complexity more heavily.

**Use:** Tends to select simpler models than AIC.

### 9. Model Evaluation Metrics

#### **Mean Absolute Error (MAE)**
**Definition:** Average absolute difference between predicted and actual values.

**Formula:** `MAE = (1/n) Σ|yᵢ - ŷᵢ|`

**Interpretation:** Lower is better; same unit as the original data.

#### **Mean Squared Error (MSE)**
**Definition:** Average squared difference between predicted and actual values.

**Formula:** `MSE = (1/n) Σ(yᵢ - ŷᵢ)²`

**Interpretation:** Penalizes large errors more heavily; sensitive to outliers.

#### **Root Mean Squared Error (RMSE)**
**Definition:** Square root of MSE; same unit as the original data.

**Formula:** `RMSE = √MSE`

**Interpretation:** More interpretable than MSE; commonly used metric.

#### **Mean Absolute Percentage Error (MAPE)**
**Definition:** Average absolute percentage difference between predicted and actual values.

**Formula:** `MAPE = (100/n) Σ|(yᵢ - ŷᵢ)/yᵢ|`

**Interpretation:** Scale-independent; useful for comparing across different datasets.

## Repository Contents

### Notebooks

#### 1. **time-series-basics.ipynb**
Introduction to time series data, basic concepts, and visualization techniques.

**Topics Covered:**
- What is time series data?
- Loading and manipulating time series data
- Basic plotting and visualization
- Identifying patterns visually

#### 2. **Decomposition-Stationarity.ipynb**
Detailed exploration of time series decomposition and stationarity concepts.

**Topics Covered:**
- Additive vs. Multiplicative decomposition
- Trend extraction techniques
- Seasonal component analysis
- Augmented Dickey-Fuller test
- Making data stationary

#### 3. **AutoRegression.ipynb**
Understanding autoregressive models and their applications.

**Topics Covered:**
- AR models theory
- ACF and PACF analysis
- Selecting AR order (p)
- Implementing AR models
- Model diagnostics

#### 4. **arima.ipynb**
Comprehensive guide to ARIMA modeling.

**Topics Covered:**
- ARIMA model components
- Parameter selection (p, d, q)
- Model fitting and validation
- Forecasting with ARIMA
- Residual analysis

#### 5. **arima-sarima.ipynb**
Advanced ARIMA and Seasonal ARIMA models.

**Topics Covered:**
- SARIMA models
- Handling seasonal patterns
- Model comparison
- Advanced forecasting techniques

#### 6. **arima-sample.ipynb**
Practical examples and case studies using ARIMA models.

### Datasets

- **MLTempDataset.csv**: Temperature data for machine learning applications
- **POP.csv**: Population data
- **Perrin Freres monthly champagne sales millions.csv**: Classic time series dataset for practice

## Getting Started

### Prerequisites

```bash
# Required Python version
Python 3.7+

# Required libraries
pip install numpy pandas matplotlib seaborn
pip install statsmodels scipy
pip install scikit-learn
pip install jupyter notebook
```

### Installation

1. Clone the repository:
```bash
git clone https://github.com/varaprasad1742/Time-Series-Analysis.git
cd Time-Series-Analysis
```

2. Install dependencies:
```bash
pip install -r requirements.txt  # If available
# Or install packages individually as listed above
```

3. Launch Jupyter Notebook:
```bash
jupyter notebook
```

4. Open any notebook and start exploring!

## Important Terms Reference

### Quick Reference Table

| Term | Definition | Example |
|------|------------|---------|
| **Trend** | Long-term increase/decrease in data | Upward trajectory in sales over years |
| **Seasonality** | Regular periodic fluctuations | Holiday sales spikes |
| **Stationary** | Constant statistical properties | Stock returns (often stationary) |
| **ACF** | Correlation with lagged values | How today relates to yesterday |
| **PACF** | Direct correlation at specific lag | Pure relationship after removing intermediate effects |
| **Differencing** | Subtracting previous value | Today's value minus yesterday's |
| **p (AR order)** | Number of lag observations | AR(2) uses previous 2 values |
| **d (Differencing)** | Number of differencing operations | d=1 for first-order differencing |
| **q (MA order)** | Number of lagged forecast errors | MA(1) uses previous error term |
| **White Noise** | Random, uncorrelated residuals | What's left after modeling |

## Best Practices

1. **Always visualize your data first** - Understand patterns before modeling
2. **Check for stationarity** - Use ADF test and make data stationary if needed
3. **Start simple** - Begin with simple models and increase complexity only if needed
4. **Validate your model** - Use train-test split and cross-validation
5. **Examine residuals** - Good models have random, normally distributed residuals
6. **Compare multiple models** - Use AIC/BIC to select the best model
7. **Consider domain knowledge** - Statistical significance doesn't always mean practical importance

## Common Pitfalls to Avoid

- Not checking for stationarity
- Overfitting with too many parameters
- Ignoring seasonality in data
- Not validating on out-of-sample data
- Extrapolating too far into the future
- Ignoring outliers and their impact
- Not considering external factors (holidays, events)

## Resources

### Books
- "Forecasting: Principles and Practice" by Rob J Hyndman and George Athanasopoulos
- "Time Series Analysis and Its Applications" by Robert H. Shumway and David S. Stoffer
- "Introduction to Time Series and Forecasting" by Peter J. Brockwell and Richard A. Davis

### Online Resources
- [Statsmodels Documentation](https://www.statsmodels.org/)
- [Time Series Forecasting Course](https://otexts.com/fpp2/)
- [Kaggle Time Series Tutorial](https://www.kaggle.com/learn/time-series)

### Research Papers
- Box, G.E.P. and Jenkins, G.M. (1970) "Time Series Analysis: Forecasting and Control"
- Dickey, D.A. and Fuller, W.A. (1979) "Distribution of the Estimators for Autoregressive Time Series with a Unit Root"

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available for educational purposes.

## Acknowledgments

This repository is designed for educational purposes to help students and practitioners understand time series analysis concepts through practical examples and clear explanations.

---

**Note:** This repository is continuously updated with new examples and techniques. Star the repository to stay updated with the latest additions!

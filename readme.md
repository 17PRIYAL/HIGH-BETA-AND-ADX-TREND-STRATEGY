# High Beta Risk-Parity Momentum Strategy with ADX Filter

A quantitative equity allocation framework built using Python that combines:

* High-beta stock selection
* ADX-based trend strength filtering
* Momentum signals
* Inverse covariance portfolio allocation
* Risk parity weighting
* Market regime detection

The strategy is designed on Indian equities using publicly available Yahoo Finance market data.

---

# Strategy Overview

This project implements a systematic trading and portfolio construction pipeline for Indian equities.

The workflow:

1. Download and clean historical market data
2. Select high-beta stocks relative to NIFTY
3. Generate ADX-based trend signals
4. Apply momentum filtering
5. Construct covariance-adjusted portfolio weights
6. Apply risk parity scaling
7. Detect market regime
8. Generate final normalized portfolio allocations

---

# Features

* Robust batch-wise market data download
* Beta estimation using OLS regression
* High-beta stock filtering
* Custom ADX implementation
* Momentum-based directional signals
* Covariance-aware allocation
* Risk parity adjustment
* Market trend filtering
* Fully implemented in Python

---

# Strategy Pipeline

## 1. Data Collection

Historical OHLC and closing price data are downloaded using `yfinance`.

The implementation includes:

* Batch downloading
* Retry-safe architecture
* Missing value handling
* Data alignment
* Forward filling for small gaps

Benchmark used:

```python
^NSEI
```

---

## 2. High Beta Stock Selection

Stock beta is calculated against NIFTY returns using Ordinary Least Squares (OLS) regression.

### Beta Formula

\beta_i = \frac{Cov(R_i, R_m)}{Var(R_m)}

Stocks are filtered using:

```python
Beta > 1
```

This isolates stocks with higher sensitivity to market movements.

---

## 3. ADX Trend Filter

The strategy computes:

* True Range (TR)
* Positive Directional Movement (+DM)
* Negative Directional Movement (-DM)
* Directional Indicators (+DI / -DI)
* Average Directional Index (ADX)

### ADX Condition

ADX > 15

If the trend strength condition is satisfied, momentum signals become active.

---

## 4. Momentum Signal Generation

Momentum is estimated using:

Momentum_t = \frac{1}{20}\sum_{i=1}^{20} r_{t-i}

Final signal logic:

```python
Signal = ADX Filter × Sign(Momentum)
```

### Signal Interpretation

| Signal | Meaning    |
| ------ | ---------- |
| +1     | Long Bias  |
| -1     | Short Bias |
| 0      | No Trade   |

---

## 5. Covariance-Based Portfolio Construction

The covariance matrix of filtered returns is computed.

Portfolio exposure is generated using inverse covariance weighting.

### Raw Weight Formula

w = \Sigma^{-1} s

Where:

* ( \Sigma^{-1} ) = inverse covariance matrix
* ( s ) = signal vector

This improves diversification across correlated assets.

---

## 6. Risk Parity Adjustment

Weights are scaled by volatility.

### Risk Adjustment

w_{rp} = \frac{w}{\sigma}

This reduces concentration in highly volatile assets.

---

## 7. Market Regime Detection

A risk parity portfolio is constructed using inverse volatility weighting.

### Risk Parity Weights

w_i = \frac{1/\sigma_i}{\sum_j 1/\sigma_j}

Market regime is estimated using rolling portfolio momentum.

### Regime Signal

| Signal | Regime  |
| ------ | ------- |
| +1     | Bullish |
| -1     | Bearish |

---

## 8. Final Portfolio Allocation

Final portfolio weights:

w_{final} = \frac{w_{rp} \times Regime}{\sum |w_{rp}|}

Weights are normalized to maintain portfolio balance.

---

# Tech Stack

| Category              | Libraries     |
| --------------------- | ------------- |
| Data Handling         | pandas, numpy |
| Market Data           | yfinance      |
| Statistical Modelling | statsmodels   |
| Portfolio Mathematics | numpy.linalg  |

---

# Installation

Clone the repository:

```bash
git clone <your-repo-link>
cd <repo-name>
```

Install dependencies:

```bash
pip install pandas numpy yfinance statsmodels
```

---

# Usage

Run the strategy:

```bash
python strategy.py
```

The script will:

* Download market data
* Compute beta-filtered universe
* Generate signals
* Construct portfolio weights
* Output final allocations

---

# Project Structure

```bash
├── strategy.py
├── README.md
└── requirements.txt
```

---

# Key Quantitative Concepts Used

* Beta Estimation
* Momentum Investing
* ADX Trend Strength
* Covariance Matrix
* Inverse Covariance Allocation
* Risk Parity
* Regime Detection
* Volatility Scaling

---

# Potential Enhancements

Possible future improvements:

* Walk-forward optimization
* Sharpe ratio optimization
* Dynamic volatility targeting
* Transaction cost modelling
* Portfolio turnover analysis
* Factor neutrality constraints
* Backtesting framework integration

---

# Example Use Cases

This project can be used for:

* Quantitative research portfolios
* Internship applications
* Algorithmic trading demonstrations
* Portfolio optimization studies
* Systematic trading research

---

# Disclaimer

This project is intended strictly for educational and research purposes only.

It should not be interpreted as investment advice, portfolio management advice, or a live trading recommendation.

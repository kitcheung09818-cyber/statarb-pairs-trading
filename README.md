# 📈 Quantitative Statistical Arbitrage Pairs Trading Engine

An end-to-end Python implementation of a **Statistical Arbitrage (StatArb) Pairs Trading Strategy** evaluating commodity-linked Exchange Traded Funds (**iShares MSCI Australia ETF `EWA`** and **iShares MSCI Canada ETF `EWC`**).

The strategy models long-term price cointegration, estimates mean-reversion speed via an Ornstein-Uhlenbeck process, and executes dynamic, state-machine trades during a 3-year out-of-sample backtest incorporating real-world market friction.

---

## 📌 Strategy Overview & Workflow

1. **Dataset Partitioning:**
   * **In-Sample (Train):** 2018-01-02 to 2022-12-30 (1,259 trading days) for parameter calibration.
   * **Out-of-Sample (Test):** 2023-01-03 to 2025-12-31 (752 trading days) for realistic backtesting without lookahead bias.

2. **Econometric Calibration (In-Sample):**
   * **OLS Regression:** Calculates optimal hedge ratio ($\beta = 0.4817$) and pricing intercept ($\alpha = 4.5478$).
   * **Engle-Granger Cointegration Test:** Confirms stationary, mean-reverting behavior of the residual spread ($p < 0.05$).
   * **Ornstein-Uhlenbeck Process:** Fits an AR(1) model to determine spread decay, yielding a calculated half-life of **33 days**.

3. **Out-of-Sample Signal Generation & Backtesting:**
   * **Dynamic Rolling Lookback:** Sets a rolling window ($1.5 \times \text{half-life} = 49\text{ days}$) to compute dynamic moving average ($\mu_t$) and standard deviation ($\sigma_t$).
   * **Z-Score Triggers:** Stateful entry at $|Z_t| \ge 2.0$ (Long/Short Spread) and exit at $|Z_t| \le 0.0$ (Mean Reversion).
   * **Institutional Friction & Capital Normalization:** Deducts **5 bps (0.05%)** transaction fee per trade shift and normalizes portfolio returns by combined capital allocation ($1 + |\beta|$).

---

## 🛠️ Tech Stack & Dependencies

* **Language:** Python 3.10+
* **Data Retrieval:** `yfinance`
* **Data Manipulation & Mathematics:** `pandas`, `numpy`
* **Econometrics & Statistics:** `statsmodels` (OLS, ADF Test, Cointegration)
* **Visualization:** `matplotlib`, `seaborn`

---

## 🚀 Quickstart & Notebook Execution

### Run Locally
```bash
# Clone the repository
git clone [https://github.com/kitcheung09818-cyber/statarb-pairs-trading.git](https://github.com/kitcheung09818-cyber/statarb-pairs-trading.git)

# Install required packages
pip install pandas numpy statsmodels yfinance matplotlib seaborn

# Launch Jupyter Notebook
jupyter notebook

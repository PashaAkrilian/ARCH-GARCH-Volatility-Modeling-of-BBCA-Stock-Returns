# ARCH GARCH Volatility Modeling of BBCA Stock Returns

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Method](https://img.shields.io/badge/Method-ARMA%20%2B%20GARCH%20%2B%20EGARCH-green.svg)

A **financial time series modeling project** focused on analyzing the volatility of **BBCA (Bank Central Asia) stock log returns** using **ARMA, ARCH effect testing, GARCH, and EGARCH models**.

This notebook evaluates return stationarity, serial dependence, conditional heteroskedasticity, and asymmetric volatility behavior to build a robust volatility forecasting framework commonly used in financial econometrics.

---

## Project Overview

The main objective of this project is to model and understand the volatility structure of BBCA daily stock returns.

The workflow includes:

* return visualization and volatility clustering analysis
* stationarity testing
* white noise diagnostics
* ARMA mean model estimation
* ARCH effect detection
* GARCH volatility modeling
* standardized residual diagnostics
* asymmetric volatility modeling using EGARCH

This approach is widely used for financial risk analysis, volatility forecasting, and portfolio decision support.

---


## Workflow

```text
Load BBCA return data
        ↓
Visualize log returns
        ↓
Stationarity test (KPSS)
        ↓
White noise test (Ljung-Box)
        ↓
ARMA(1,1) estimation
        ↓
Residual diagnostics
        ↓
ARCH effect testing
        ↓
GARCH(2,2) estimation
        ↓
Standardized residual diagnostics
        ↓
EGARCH(1,1,1) estimation
        ↓
Asymmetric volatility interpretation
```

---

## Methodology

## 1. Data Exploration

The project starts with daily **log returns of BBCA stock prices**.

Initial visualization shows clear **volatility clustering**, where periods of high volatility are followed by high volatility and low volatility follows low volatility. This indicates the presence of conditional heteroskedasticity and motivates the use of ARCH/GARCH family models.

---

## 2. Stationarity and White Noise Testing

### Tests Used

* KPSS Test
* ACF and PACF plots
* Ljung-Box Test

### Findings

The return series satisfies the stationarity assumption (**KPSS p-value > 0.05**) but fails the white noise assumption (**Ljung-Box p-value < 0.05**), indicating serial dependence that must be modeled using ARMA.

---

## 3. ARMA Mean Model Estimation

The mean equation is modeled using:

```text
ARMA(1,1)
```

This step captures linear dependence in returns before volatility modeling.

### Diagnostic Result

Residual diagnostics show:

* no significant ACF/PACF spikes
* Ljung-Box p-value > 0.05

This confirms that ARMA(1,1) successfully produces **white noise residuals** for the mean process.

---

## 4. Testing for ARCH Effects

The squared residuals from the ARMA model are analyzed using:

* ACF of squared residuals
* Ljung-Box test on squared residuals

### Result

Significant dependence in squared residuals confirms the presence of **ARCH effects**, meaning volatility clustering still exists and volatility modeling is required.

---

## 5. GARCH Volatility Model

The volatility equation is modeled using:

```text
GARCH(2,2)
```

with:

* mean = Zero
* volatility = GARCH
* residual standardization

### Key Interpretation

The GARCH model successfully captures conditional variance dynamics and reduces remaining ARCH effects.

QQ-plot diagnostics indicate generally normal behavior with visible **heavy tails**, a common characteristic of financial return data.

---

## 6. Diagnostic Checking of Standardized Residuals

Validation includes:

* ACF/PACF of standardized residuals
* Ljung-Box test
* ACF of squared standardized residuals
* Ljung-Box test on squared standardized residuals

### Result

The model diagnostics indicate that:

* no remaining autocorrelation exists
* no significant remaining ARCH effects exist

This confirms that the ARMA + GARCH specification is statistically adequate.

---

## 7. Asymmetric Volatility Modeling

To test leverage effects and asymmetric market responses, the notebook estimates:

```text
EGARCH(1,1,1)
```

This model captures whether negative shocks produce stronger volatility responses than positive shocks.

### Focus Parameter

```text
gamma[1]
```

Interpretation of this parameter determines the presence of asymmetric volatility behavior.

---

## Example Usage

```python
from arch import arch_model
from statsmodels.tsa.arima.model import ARIMA

# ARMA mean model
arma_model = ARIMA(ret, order=(1, 0, 1))
arma_result = arma_model.fit()

# GARCH volatility model
garch = arch_model(
    arma_result.resid,
    mean='Zero',
    vol='GARCH',
    p=2,
    q=2
)

garch_result = garch.fit()
print(garch_result.summary())
```

---

## Tech Stack

* Python
* pandas
* NumPy
* matplotlib
* seaborn
* statsmodels
* arch
* scipy

---

## Use Cases

This project is highly relevant for:

* financial econometrics
* stock volatility forecasting
* risk management
* Value-at-Risk baseline modeling
* portfolio optimization support
* academic time series assignments

---

## License

This project is licensed under the **MIT License**.

You are free to use, modify, distribute, and adapt this work with proper attribution.

---

## MIT License

Copyright (c) 2026 Dimas Pasha Akrilian

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

## Author

**Dimas Pasha Akrilian**

This repository is part of my **financial time series and statistical modeling portfolio**, focusing on volatility analysis, econometric forecasting, and applied ARCH/GARCH modeling.

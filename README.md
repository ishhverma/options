

# AAPL Option Pricing and Implied Volatility Analysis

A comprehensive, end-to-end pipeline for **option pricing, Greek computation, implied volatility extraction, and IV surface construction** for AAPL equity options. This repository serves as a robust template for quantitative option analysis and volatility modeling.

📄 [Interactive Notebook](https://github.com/ishhverma/options/blob/main/Option_Pricing_and_Volatility_Analysis.ipynb)

---

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [IV Surface Methodology](#iv-surface-methodology)
* [Project Structure](#project-structure)
* [Requirements](#requirements)
* [Usage](#usage)
* [References](#references)

---

## Overview

This notebook implements a complete workflow for AAPL option analysis:

1. **Data Acquisition:** Pulls spot price, available expirations, and full option chains (calls and puts) from Yahoo Finance.
2. **ATM Option Identification:** Selects the at-the-money option based on (|K - S|) minimization and computes time to maturity.
3. **Option Pricing:** Prices the selected ATM option using:

   * Black–Scholes (European)
   * Binomial Tree (American)
   * Monte Carlo Simulation (European)
4. **Implied Volatility Extraction:** Computes the market-implied volatility using a root-finding algorithm to match model prices with market quotes.
5. **Greeks Computation:** Calculates Delta, Gamma, Vega, Theta, and Rho for all options in the dataset.
6. **IV Surface Construction:** Generates dense, smoothed 3D surfaces for calls and puts using spline interpolation and Gaussian kernel smoothing.
7. **Visualization:** Produces high-quality 3D plots depicting the IV term structure and moneyness profile.

---

## Features

### Data Ingestion

* Retrieves **option chains** for all available expirations using `yfinance.Ticker('AAPL')`.
* Consolidates calls and puts into a **single DataFrame** with strike, bid/ask, last price, open interest, and raw implied volatilities.

### ATM Option Pricing

* Automated **ATM selection** for any expiry.
* Pricing methods:

  * **Black–Scholes**: Analytical European option pricing
  * **Binomial Tree**: American option pricing via recombining lattice
  * **Monte Carlo Simulation**: Path-dependent European option pricing
* Comparative visualization of **model vs market prices**.

### Implied Volatility and Greeks

* Extracts **ATM implied volatility** using Brent’s method.
* Computes **Greeks** for all strikes and expiries.
* Provides cross-sectional analysis (e.g., Delta vs Strike) for insight into option sensitivities.

### IV Smile and Surface Construction

* Constructs IV smiles for individual expirations.
* Prepares a structured ((K, T, \sigma_{\text{imp}})) dataset for surface generation.
* Applies **spline interpolation** to generate dense IV grids.
* Smooths surfaces using **Gaussian kernel filtering** to reduce noise while preserving key structural patterns.

---

## IV Surface Methodology
 <img width="1364" height="1190" alt="image" src="https://github.com/user-attachments/assets/482d31fd-f466-42e2-a4ba-4d600b4548c1" />


* **Top Row:** Call surfaces

  * Left: raw spline-interpolated IV
  * Right: Gaussian-smoothed IV

* **Bottom Row:** Put surfaces

  * Left: raw spline-interpolated IV
  * Right: Gaussian-smoothed IV

Axes definitions:

* **X-axis:** Strike
* **Y-axis:** Time to maturity (years)
* **Z-axis:** Implied volatility

This visualization highlights both the **term structure** and **moneyness dynamics** of the AAPL options market.


---

## Project Structure
<img width="2848" height="1600" alt="image" src="https://github.com/user-attachments/assets/ef7a52c9-11a2-4d41-bfce-48af4b4c6790" />

```

---

## Requirements

* Python 3.x
* Libraries: `yfinance`, `numpy`, `pandas`, `scipy`, `matplotlib`

Install dependencies via:

```bash
pip install yfinance numpy pandas scipy matplotlib
```

---

## Usage

1. Clone the repository and open the notebook in **Jupyter** or **Google Colab**.
2. Run setup cells to install dependencies and import libraries.
3. Execute **data acquisition** to fetch AAPL spot, expiration dates, and option chains.
4. Select the desired **expiry and option type** for ATM analysis.
5. Run **pricing, Greeks, and IV smile computation**.
6. Generate **3D IV surfaces** for calls and puts using spline interpolation and Gaussian smoothing.

---

## References

1. Yahoo Finance API (`yfinance`)
2. Matplotlib 3D surface plotting and Gaussian filtering techniques

<img width="1364" height="1190" alt="image" src="https://github.com/user-attachments/assets/856bb1bd-c4b5-4276-b7fb-3c2978a081cf" />
# Option Pricing and Implied Volatility Analysis for AAPL

This project is a complete, end‑to‑end pipeline to download AAPL option chains, price an at‑the‑money option with multiple models, compute Greeks, extract implied volatilities, and build smooth 3D implied volatility (IV) surfaces for calls and puts using spline interpolation and Gaussian smoothing
## Project Overview

The notebook `Option_Pricing_and_Volatility_Analysis.ipynb` automates the full workflow:
https://github.com/ishhverma/options/blob/main/Option_Pricing_and_Volatility_Analysis.ipynb

- Fetches AAPL option data from Yahoo Finance using `yfinance`.[1]
- Selects an at‑the‑money (ATM) option and computes time to maturity from today’s date.[1]
- Prices the option with:
  - Black–Scholes (European),
  - Binomial tree (American),
  - Monte Carlo simulation (European).[1]
- Retrieves the market price of the chosen option and backs out the implied volatility via a root‑finding routine.[1]
- Computes Black–Scholes Greeks (Delta, Gamma, Vega, Theta, Rho) for every option in the filtered dataset and merges them into a single DataFrame.[1]
- Extracts implied volatilities across strikes and expiries, then builds smoothed IV surfaces separately for calls and puts using spline interpolation plus Gaussian kernel smoothing.[1]
- Visualizes the final IV landscapes as 3D surfaces, as illustrated below.[2][1]

The repository is designed as a practical template for quantitative option analysis and IV surface construction on single‑name equities such as AAPL.[1]

***

## Key Features

- **Data ingestion**
  - Uses `yfinance.Ticker('AAPL')` to pull spot price, available expiration dates, and full option chains (calls and puts) for each expiry.[1]
  - Concatenates option chains into a unified DataFrame, including strikes, last prices, bids, asks, open interest, and raw implied volatilities supplied by Yahoo.[1]

- **Model pricing (ATM option)**
  - Automatically identifies an ATM call or put by minimizing \(|K - S|\) at a chosen expiry. [1]  
  - Computes:
    - Black–Scholes price via closed‑form formula,  
    - Binomial American price via a recombining tree,  
    - Monte Carlo price via discretized geometric Brownian motion paths.[1]
  - Summarizes all model prices alongside the market price in a comparison bar chart.[1]

- **Implied volatility and Greeks**
  - Uses a Brent root‑finder to match the Black–Scholes price to the market price and obtain the ATM implied volatility.[1]
  - Implements a generic `blackscholesgreeks` function and applies it row‑wise to the IV DataFrame to compute Delta, Gamma, Vega, Theta, and Rho for every option.[1]
  - Demonstrates cross‑sections such as Delta vs. strike for calls and puts.[1]

- **IV smile and surface construction**
  - Builds an implied volatility smile for a selected expiry by plotting IV against strike.[1]
  - For the full surface:
    - Splits the dataset into calls and puts,
    - Converts calendar expiries into continuous time‑to‑maturity in years,
    - Prepares \((K, T, \sigma_{\text{imp}})\) triplets for interpolation.[1]
  - Uses spline interpolation over a regular strike–maturity grid to generate dense IV surfaces.[1]
  - Applies Gaussian kernel smoothing to the interpolated grids to remove noise and local artefacts while preserving overall structure.[1]

- **3D visualization**
  - Produces a 2×2 grid of 3D surfaces with Matplotlib:
    - Spline‑interpolated call IV surface,
    - Gaussian‑smoothed call IV surface,
    - Spline‑interpolated put IV surface,
    - Gaussian‑smoothed put IV surface.[2][1]
  - Each plot uses strike on the x‑axis, time‑to‑maturity on the y‑axis, and implied volatility on the z‑axis, with separate colormaps for calls and puts.[1]

***

## Project Structure

- `Option_Pricing_and_Volatility_Analysis.ipynb`  
  The main notebook that contains all code, from data download to final visualization.[1]

- `image.jpg`  
  Example figure showing the 3D IV surfaces for AAPL calls and puts, both raw spline‑interpolated and Gaussian‑smoothed.[2][1]

***

## Getting Started

### Requirements

The notebook expects a standard scientific Python stack, including:

- Python 3  
- `yfinance`, `numpy`, `pandas`  
- `scipy` (for `stats`, optimization, and interpolation)  
- `matplotlib` and `mpl_toolkits.mplot3d`  
- `scipy.ndimage` (for Gaussian filtering)[1]

All dependencies are installed directly in the first cells of the notebook using `pip install yfinance` and standard imports.[1]

### Running the Notebook

1. **Clone or download** the repository and open `Option_Pricing_and_Volatility_Analysis.ipynb` in Jupyter or Google Colab.[1]
2. **Run the setup cells** to install `yfinance` and import all required libraries.[1]
3. **Execute the data‑download section** to fetch the AAPL spot price, expiration dates, and option chains.[1]
4. **Choose an expiration date and option type** (call or put) in the ATM selection cell; the notebook uses this to define \(S\), \(K\), \(T\), and the risk‑free rate.[1]
5. **Run the pricing, Greeks, and IV smile sections** to compute and visualize per‑option metrics.[1]
6. **Execute the IV surface section** to:
   - Build the call and put IV datasets,
   - Interpolate and smooth the IV grids,
   - Render and display the 3D surfaces.[2][1]

***

## How the IV Surfaces Are Generated

The four panels in `image.jpg` correspond exactly to the final surface‑plot block in the notebook.[2][1]

- The **top row** shows call surfaces:  
  - Left: raw spline‑interpolated IV over the \((K, T)\) grid.  
  - Right: the same surface after Gaussian smoothing with a configurable standard deviation `sigma`.[1]

- The **bottom row** repeats this procedure for **puts**, using a different colormap to visually distinguish them from calls.[2][1]

Together, these plots provide a clean, smoothed view of the implied volatility term structure and moneyness structure for both sides of the AAPL options market.[1]

# Free-Boundary Estimation for American Put Options

This repository contains research code for estimating the early-exercise free boundary of American put options using binomial tree methods under constant volatility and GARCH-based volatility.

## Overview

American put options allow early exercise before maturity. This creates a free-boundary problem: at each time step, the model determines whether it is optimal to exercise the option or continue holding it.

This project numerically estimates that early-exercise boundary and compares the results under constant volatility and GARCH-based volatility.

## Methods

- Binomial tree option pricing
- American put option valuation
- Early-exercise boundary estimation
- Constant volatility model
- GARCH-based volatility model

## Files

- `constant-volatility-free-boundary.ipynb` — estimates the free boundary under constant volatility.
- `garch-volatility-free-boundary.ipynb` — estimates the free boundary under GARCH-based volatility.
- `MSFT.csv` — Microsoft price data used for numerical testing.
- `requirements.txt` — Python package requirements.

## Research Area

Mathematical finance, option pricing, free-boundary problems, stochastic volatility, derivatives, and computational finance.

## Citation

*Free-Boundary Estimation for American Put Options*. GitHub repository.

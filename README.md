# Free-Boundary Estimation for American Put Options

**Jorge N. Hernández & Enrique Villamor** — Florida International University, Department of Mathematics and Statistics

---

## Overview

This repository contains the research code accompanying the working paper *Approximating the Free Boundary of an American Option*. The paper develops a multi-period binomial framework to estimate the early-exercise free boundary of American put options under two volatility regimes:

1. **Constant volatility** — standard recombining binomial tree with backward induction and linear interpolation to locate the free boundary at each time step.
2. **Stochastic volatility (GARCH)** — a non-recombining binomial tree where volatility is updated dynamically at each node using a locally recalibrated GARCH(1,1) model, producing path-dependent variance estimates that reflect volatility clustering and mean reversion.

The key contribution is a unified framework that simultaneously estimates the free boundary and prices the American option, bridging two problems typically treated separately in the literature.

---

## Mathematical Problem

The value $V(S,t)$ of an American put option satisfies the free-boundary problem:

$$\frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} + rS\frac{\partial V}{\partial S} - rV \leq 0, \qquad V(S,t) \geq \max(K-S,\,0)$$

with complementary slackness: at each point either the Black-Scholes PDE holds (continuation region) or the option is exercised (exercise region). The free boundary $S_f(t)$ is the critical stock price at which:

$$V(S_f(t),\,t) = K - S_f(t), \qquad \frac{\partial V}{\partial S}(S_f(t),\,t) = -1$$

No closed-form solution exists due to the coupling of the PDE with these nonlinear free-boundary and smooth-pasting conditions. This work estimates $S_f(t)$ numerically via binomial trees.

---

## Methods

### Constant Volatility
- Recombining binomial tree with up/down factors $u = e^{\sigma\sqrt{\Delta t}}$, $d = 1/u$
- Backward induction with early exercise:

$$V^*_{i,j} = \max(V_{i,j},\, K - S_{i,j})$$

- Free boundary located by bracketing the transition between continuation and exercise regions
- Linear interpolation to refine $S_f(t_j)$ between adjacent nodes
- Iterative adjustment of $S_0$ until convergence: $|V_{0,0} - \max(K-S_0,0)| < \varepsilon$

### Stochastic Volatility (GARCH)
- Non-recombining binomial tree — each path is distinct
- GARCH(1,1) recalibrated locally at each node using only the price history of the unique path leading to it:

$$\sigma_{i,j}^2 = \omega_{i,j} + \beta_{i,j}\,\sigma_{\lfloor i/2 \rfloor,\,j-1}^2 + \alpha_{i,j}\left(\frac{S_{i,j} - S_{\lfloor i/2 \rfloor,\,j-1}}{S_{\lfloor i/2 \rfloor,\,j-1}}\right)^2$$

- Stock price evolution at each node:

$$S_{i,j} = S_{\lfloor i/2 \rfloor,\,j-1}\,\exp\left(\mu\Delta t + (-1)^i\,\sigma_{\lfloor i/2 \rfloor,\,j-1}\sqrt{252}\sqrt{\Delta t}\right)$$

- After recalibration, the historical dataset is restored to prevent forward contamination across paths
- Risk-neutral probabilities are node-specific: $p_{i,j} = \frac{e^{r\Delta t} - d_{i,j}}{u_{i,j} - d_{i,j}}$

### Continuous-Time Limit
The paper also derives the continuous-time SDE limit of the GARCH(1,1) process via Itô's formula:

$$d(\sigma_t^2) = \bar\gamma\,[\Gamma_L - \sigma_t^2]\,252\,dt + \bar\alpha\sqrt{2}\sqrt{252}\,\sigma_t^2\,dW_t$$

where $\Gamma_L$ is the long-run variance and $\bar\gamma = 1 - \bar\alpha - \bar\beta$. This confirms the discrete updates converge to a mean-reverting diffusion in continuous time.

---

## Results

**Constant volatility** (`n = 100`, `K = 450`, `T = 1`, MSFT 2019–2024):
- Free boundary converges to $S_0 = 313.73$ at $t = 0$
- Boundary exhibits the expected concave-up shape approaching maturity

**Stochastic volatility** (`n = 10`, `K = 450`, MSFT 2019–2024, $S_{0,0} = 391.46$, $\varepsilon = 0.01$):
- GARCH-driven boundary lies notably higher than the constant-vol boundary, reflecting increased early-exercise incentive under volatile conditions
- Non-recombining structure produces path-dependent boundaries that constant-vol models cannot capture
- Minor irregularities at small `n` diminish as `n → ∞`

---

## Repository Structure

```
Free-Boundary/
├── constant-volatility-free-boundary.ipynb   ← Constant vol binomial tree & boundary estimation
├── garch-volatility-free-boundary.ipynb      ← GARCH non-recombining tree & boundary estimation
├── MSFT.csv                                   ← Microsoft daily price data (Aug 2019 – Aug 2024)
└── requirements.txt                           ← Python dependencies
```

---

## Replication

```bash
git clone https://github.com/jorherna1/Free-Boundary.git
cd Free-Boundary
pip install -r requirements.txt
jupyter notebook
```

Open either notebook and run all cells. The GARCH notebook is computationally intensive for large `n` due to the non-recombining structure requiring GARCH recalibration at every node (2^n nodes at depth n).

---

## Citation

Hernández, J. N., & Villamor, E. (2024). *Approximating the free boundary of an American option*. Working paper, Florida International University.

---

## Authors

- **Jorge N. Hernández** — Florida International University, Institute of Environment & Mathematics and Statistics. [jorherna@fiu.edu](mailto:jorherna@fiu.edu) · ORCID: [0009-0000-0469-4327](https://orcid.org/0009-0000-0469-4327)
- **Enrique Villamor** — Florida International University, Mathematics and Statistics. [villamor@fiu.edu](mailto:villamor@fiu.edu) · ORCID: [0000-0003-0556-6460](https://orcid.org/0000-0003-0556-6460)

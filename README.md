# Market Risk Simulator via Geometric Brownian Motion (GBM)

A Python-based quantitative finance tool that simulates asset price paths using a stochastic process and runs a risk engine to compute standard portfolio market risk metrics: **Value-at-Risk (VaR)** and **Expected Shortfall (ES)**.

## Project Overview

Predicting future asset prices and quantifying the risk of sudden market downturns are critical tasks in quantitative finance. This project implements a **Geometric Brownian Motion (GBM)** simulator to model 1,000 potential future price paths for a stock over a 1-year horizon (252 trading days). 

Once the simulation completes, the portfolio's ending distribution is analyzed by a custom risk engine to evaluate the boundaries of potential extreme financial loss.

---

## Methodology & Engine Phases

### Phase 1: Stochastic Simulation

The asset price $S_t$ is modeled using the classical Geometric Brownian Motion stochastic differential equation, solved via vectorized compounding:

$$S_t = S_{t-1} \times \exp\left( \left(\mu - \frac{1}{2}\sigma^2\right)dt + \sigma \sqrt{dt} \cdot Z_t \right)$$

Where:
* **$S_0$**: Initial stock price = $100
* **$\mu$**: Expected annual return (drift) = 5%
* **$\sigma$**: Annual volatility = 20%
* **$dt$**: Time increment ($1 / 252$ years)
* **$Z_t$**: Random shock drawn from a standard normal distribution $\mathcal{N}(0,1)$

### Phase 2: Value-at-Risk (VaR) Engine

The simulation extracts the terminal prices at Day 252 to evaluate downside risks at a **95% confidence level**:
* **95% Value-at-Risk (VaR)**: The minimum dollar amount expected to be lost over the 1-year horizon at a 5% significance level (the cut-off point of the bottom 5% of outcomes).
* **95% Expected Shortfall (ES / Conditional VaR)**: The average dollar loss given that the portfolio has already breached the 95% VaR threshold (evaluating the severity of the worst-case tail losses).

---

## Simulation Results

Based on the default parameters fixed with a random seed (`42`), the simulation yields the following portfolio metrics:

```text
--- PORTFOLIO RISK METRICS ---
Average Ending Stock Price: $105.20
Worst Simulated Outcome:    $55.67
Best Simulated Outcome:     $183.51
95% Value-at-Risk (VaR):    $25.74
Average Loss below 5% (ES): $31.92
------------------------------

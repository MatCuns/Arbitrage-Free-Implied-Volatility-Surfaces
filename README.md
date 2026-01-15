# Arbitrage-Free Implied Volatility Surfaces  
*Construction, calibration, and stochastic simulation of arbitrage-free implied volatility surfaces*

## Motivation

Implied volatility surfaces encode the market’s assessment of the full risk-neutral distribution of future asset prices. From a mathematical standpoint, an implied volatility surface arises as a nonlinear transformation of a two-parameter family of contingent claims into a scalar field whose admissible geometries are severely constrained by the absence of static and dynamic arbitrage.

Naive interpolation or simulation of implied volatilities typically violates these constraints, leading to negative risk-neutral densities, calendar-spread arbitrage, or ill-posed pricing operators. This project aims to construct, parameterize, and simulate implied volatility surfaces that are **globally consistent with arbitrage-free pricing theory**, and suitable for quantitative modeling, calibration, and numerical experimentation.

## Problem Formulation
Let $(\Omega, \mathcal{F}, (\mathcal{F}\_t)\_{t \ge 0}, \mathbb{Q})$ be a filtered probability space supporting an asset price process $S_t$ under a risk-neutral measure $\mathbb{Q}$.
For maturity $T$, the undiscounted European call price surface is

$$
C(K,T) = \mathbb{E}^{\mathbb{Q}}\left[(S_T - K)^+\right].
$$



The implied volatility surface is defined implicitly by inversion of the Black–Scholes pricing operator

$$
C(K,T) = \text{BS}\left(F_T, K, T, \sigma_{\text{imp}}(K,T)\right),
$$

where $F_T$ denotes the forward price.
The mathematical problem is to characterize and simulate families

$$
\begin{align*}
  &\{C(\cdot,T)\}_{T>0} & &\text{or equivalently} & &\sigma_{\text{imp}},
\end{align*}
$$

such that the induced pricing operator is free of static and dynamic arbitrage and admits a consistent risk-neutral representation.

## Mathematical Framework

### 1. Static Arbitrage and Measure-Theoretic Constraints

Absence of static arbitrage implies that for each maturity $T$, the mapping $K \mapsto C(K,T)$ belongs to the class of convex order preserving payoff transforms.
By the Breeden–Litzenberger representation,

$$
\frac{\partial^2 C(K,T)}{\partial K^2} = q_T(K),
$$

where $q_T$ is the Radon–Nikodym derivative of the risk-neutral measure of $S_T$ with respect to Lebesgue measure. Hence

$$
q_T(K) \ge 0, \qquad \int_0^{\infty} q_T(K)\,dK = 1,
$$

and $C(\cdot,T)$ must be convex, twice weakly differentiable, and induce a valid probability measure.
Boundary and integrability conditions further impose

$$
\lim_{K \to 0} C(K,T) = \mathbb{E}^{\mathbb{Q}}[S_T],
$$

and

$$
\lim_{K \to \infty} C(K,T) = 0.
$$

### 2. Calendar Consistency and Convex Order

For fixed strike $K$, absence of calendar-spread arbitrage requires

$$
\frac{\partial C(K,T)}{\partial T} \ge 0.
$$

At the distributional level, this implies that the family $\{S_T\}_{T>0}$ must be increasing in convex order, yielding a nested family of probability measures $\{\mu_T\}$
satisfying

$$
\int \phi(x)\,d\mu_{T_1}(x) \le \int \phi(x)\,d\mu_{T_2}(x)
$$

for all convex $\phi$ and $T_1 < T_2$.
This condition ensures the existence of a martingale coupling between maturities and underpins the absence of static arbitrage across the surface.

### 3. Total Implied Variance Geometry

Introducing log-moneyness

$$
k = \log(K/F_T)
$$

and total implied variance

$$
w(k,T) = \sigma_{\text{imp}}^2(k,T)T,
$$

translates the no-arbitrage problem into a nonlinear inequality system on $w(k,T)$.
In particular, the Dupire density operator

$$
\mathcal{L}(w)(k,T) \ge 0
$$

must remain non-negative, ensuring positivity of the induced local volatility and the existence of a valid forward Kolmogorov equation.

Asymptotic constraints such as Lee’s moment formulas further restrict the admissible growth of $w(k,T)$ as $|k| \to \infty$, linking tail behavior of the risk-neutral distribution to the geometry of the surface.

### 4. Arbitrage-Free Parameterizations

The project focuses on parametric representations of total variance

$$
w(k,T) = w(k; \theta_T),
$$

where $\theta_T \in \Theta \subset \mathbb{R}^n$ evolves on a constrained parameter manifold.
In particular, SVI-type parameterizations

$$
w(k) = a + b\left(\rho(k - m) + \sqrt{(k - m)^2 + \sigma^2}\right)
$$



and their extensions (eSSVI) are studied as nonlinear embeddings of low-dimensional manifolds into the infinite-dimensional convex cone of arbitrage-free volatility smiles.

### 5. Stochastic Surface Dynamics

Beyond static consistency, the project investigates stochastic dynamics of the form

$$
d\theta_T(t) = \mu_T(\theta(t))\,dt + \Sigma_T(\theta(t))\,dW_t,
$$

inducing a random field

$$
w_t(k,T) = w(k; \theta_T(t)).
$$

The objective is to design dynamics such that:

- static arbitrage constraints hold almost surely  
- call prices remain martingales under the pricing measure  
- the surface evolution is dynamically consistent across maturities  

This leads to drift-restriction problems analogous to Heath–Jarrow–Morton theory, but posed on nonlinear manifolds of admissible volatility surfaces.

## Project Goals

- Formalize the mathematical structure of arbitrage-free call price surfaces  
- Implement arbitrage-free parameterizations (SVI / eSSVI)  
- Calibrate them to real option market data  
- Numerically verify convexity, density positivity, and calendar consistency  
- Simulate surface dynamics via Monte Carlo methods 
- Study geometric and probabilistic properties of implied volatility evolution
- Backtesting using past data

## Language and Tools

The project is implemented primarily in **Python**, combining scientific computing, constrained numerical optimization, stochastic simulation, data-driven calibration, and quantitative visualization.

## References and Literature

- Gatheral, J. — *The Volatility Surface*
- Gatheral & Jacquier — *Arbitrage-free SVI volatility surfaces*
- Carr & Madan — *Sufficient conditions for no arbitrage*
- Fengler — *Semiparametric modeling of implied volatility*
- Lee — *Moment formulas and implied volatility at extreme strikes*

## Disclaimer

This project is for educational and research purposes only and does not constitute financial advice.

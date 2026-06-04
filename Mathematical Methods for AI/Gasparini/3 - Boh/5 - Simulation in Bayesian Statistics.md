> [!note] Recall Previous class on **univariate** and **multivariate** simulations.
---

## Simple plots

### 1. $X_1, \dots, X_n \overset{iid}{\sim} N(1, 3^2)$

![[5 - Simulation in Bayesian Statistics-1780584600919.webp]]

### 2. $X_1, \dots, X_n \overset{iid}{\sim} N(-1, 1)$
![[5 - Simulation in Bayesian Statistics-1780584624423.webp]]

### 3. $X_1, \dots, X_n \overset{iid}{\sim} N(i, 1)$
![[5 - Simulation in Bayesian Statistics-1780584717485.webp]]

> [!tip] Mean drift phenomenon Here the mean grows with the index $i$. With $n = 1000$, the cloud of points drifts diagonally upward along the line $x_i \approx i$.

---

## Mean shift (change point)
![[5 - Simulation in Bayesian Statistics-1780584668174.webp]]
$$ X_1, \dots, X_n \overset{iid}{\sim} N\big(,0 \cdot (i < 500) + 1 \cdot (i \geq 500),\ 1,\big) $$

where the indicator is $0$ if false, $1$ if true.

- **Mean shift at $i = 500$:** the band of points jumps from being centered at $0$ to being centered at $1$.

> [!important] Change point In **quality control**, **econometrics**, and other fields, $i = 500$ is called a **change point**. It is very interesting to estimate if you do not know it.
> 
> _e.g._ $x_i =$ difference between a certain manufactured item's characteristic and its design.

---

## Variance drift
![[5 - Simulation in Bayesian Statistics-1780584768007.webp]]
$$ X_1, \dots, X_n \overset{iid}{\sim} N(0, i^2) $$

- **Variance drift:** the points stay centered at $0$, but the spread widens as $i$ increases (fan shape), up to $\sigma = 1000$.

## Variance shift
![[5 - Simulation in Bayesian Statistics-1780584805008.webp]]
$$ X_1, \dots, X_n \overset{iid}{\sim} N\big(0,\ 1^2 \cdot (i < 500) + 4^2 \cdot (i \geq 500),\big) $$

- **Variance shift:** centered at $0$ throughout, but the spread suddenly increases at $i = 500$ (from $\sigma = 1$ to $\sigma = 4$).

---

## Random walk (dependent series)

$$ X_i = X_{i-1} + \varepsilon_i, \qquad \varepsilon_i \overset{iid}{\sim} N(0, 1) $$

> [!warning] The $\varepsilon_i$ are iid, but the $X_i$ are **not**.

![[5 - Simulation in Bayesian Statistics-1780584969623.webp]]
With $X_0 = 0$:

$$ \begin{aligned} X_1 &= \varepsilon_1 \ X_2 &= X_1 + \varepsilon_2 = \varepsilon_1 + \varepsilon_2 \ &\ \ \vdots \end{aligned} $$

- With $n = 1000$, the trajectory wanders (Brownian-motion-like behavior).
- In the lag plot of $x_{i+1}$ vs $x_i$, **pairs tend to appear** along the diagonal (positive correlation between consecutive values).

## Anti-correlated series

$$ X_i = -X_{i-1} + \varepsilon_i, \qquad \varepsilon_i \overset{iid}{\sim} N(0, 1) $$
![[5 - Simulation in Bayesian Statistics-1780585053363.webp]]
With $X_0 = 0$:

$$ \begin{aligned} X_1 &= \varepsilon_1 \ X_2 &= -\varepsilon_1 + \varepsilon_2 \quad \big(\overset{d}{=} \varepsilon_1 + \varepsilon_2\big) \end{aligned} $$

- **Alternate behavior:** values flip sign from step to step (zig-zag).
- In the lag plot of $x_{i+1}$ vs $x_i$, points fall along the **anti-diagonal** (negative correlation).

---

## Running mean and the Law of Large Numbers

$$ X_1, \dots, X_n \overset{iid}{\sim} N(0, 1), \qquad \bar{X}_i = \frac{1}{i} \sum_{k=1}^{i} X_k \ \approx\ N\left(0, \tfrac{1}{i}\right) $$
![[5 - Simulation in Bayesian Statistics-1780585101790.webp]]
- The running mean $\bar{X}_i$ funnels in toward $0$ as $i$ grows; its spread shrinks like $\tfrac{1}{i}$.
- The (red) density of $\bar{X}_i$ concentrates around $\mu = 0$.

> [!quote] Law of Large Numbers $$\bar{X}_i \xrightarrow[i \to \infty]{} 0 = \mu$$

---

## Estimating a probability by simulation
![[5 - Simulation in Bayesian Statistics-1780585161739.webp]]
$$ X_1, \dots, X_n \overset{iid}{\sim} N(0, 1) $$

Count the fraction of points below a threshold (red line at $1.5$):

$$ \frac{\#{\text{points below } 1.5}}{\cancel{n}1000} = \text{fraction of points below red line} $$

$$ \approx \texttt{pnorm}(1.5) = \int_{-\infty}^{1.5} \frac{1}{\sqrt{2\pi}} e^{-\frac{1}{x^2}} dx $$

---

## Reconstructing a distribution from samples

> [!summary] The last two examples show that **if you are able to simulate from a distribution, you can reconstruct it** even if you do not know its analytical form precisely.

For $n = 10^6$ samples (red line at $1.5$, green line at $-3$):
![[5 - Simulation in Bayesian Statistics-1780585233387.webp]]
$$ \mu \approx \bar{X} $$ $$ P(X_i \leq 1.5) \approx \text{fraction of points below the red line} $$ $$ P(X_i \leq -3) \approx \text{fraction of points below the green line} $$

> [!important] This is the basis of modern **generative models**.
> 
> Sometimes, **if** we are able to simulate from $F$, we can estimate its properties by **brute-force simulation**.

---

## First application: approximating a posterior distribution

> [!example] Setup First application: using simulation to approximate a **posterior distribution** in Bayesian statistics.

**Model:**
![[5 - Simulation in Bayesian Statistics-1780585314284.webp]]
$$ X_1, \dots, X_n  \overset{c.i.i.d.}{\sim} N(\mu, \sigma^2) = N\left(\mu, \tfrac{1}{\tau}\right) $$
**Priors (independent):**

$$ \mu \sim N\left(\mu_0, \tfrac{1}{\tau_0}\right), \qquad \tau \sim \text{Gamma}(a, \lambda) $$

where $\tau = \frac{1}{\sigma^2}$ is the **precision**.

### Posterior

$$ \pi(\mu, \tau \mid x_1, \dots, x_n) \propto \text{prior} \times \text{likelihood} $$

$$ \propto \underbrace{\exp \left\{-\tfrac{\tau_0}{2}(\mu - \mu_0)^2\right\}}_{\text{prior on } \mu} \times \underbrace{\tau^{a-1} e^{-\lambda \tau}}_{\text{prior on } \tau} \times \underbrace{\tau^{n/2} \exp \left\{-\tfrac{\tau}{2} \sum (x_i - \mu)^2\right\}}_{\text{likelihood}} $$

(constants eliminated)

$$ = \exp\left\{-\tfrac{\tau_0}{2}(\mu - \mu_0)^2 - \tfrac{\tau}{2}\sum (x_i - \mu)^2\right\}\ \tau^{a + \frac{n}{2} - 1}\ e^{-\lambda \tau} $$

> [!fail] No conjugacy It is **not possible to factorize** this; therefore $\mu$ and $\tau$ are **not independent a posteriori**.
> 
> This posterior on $(\mu, \tau)$ is difficult to deal with.

### **BUT** — the full conditionals

**(1)** Full conditional of $\mu$ (with $\tau$ known):

$$ \pi(\mu \mid x_1, \dots, x_n, \boxed{\tau}) = N\left( \frac{\tau_0 \mu_0 + n \tau \bar{x}}{\tau_0 + n\tau},\ \frac{1}{\tau_0 + n\tau} \right) $$

**(2)** Full conditional of $\tau$ (with $\mu$ known):

$$ \pi(\tau \mid x_1, \dots, x_n, \boxed{\mu}) \propto \tau^{\overbrace{a + \frac{n}{2}}^{a^\ast} - 1} \exp \left\{ -\overbrace{\Big(\lambda + \tfrac{1}{2}\sum (x_i - \mu)^2\Big)}^{b^\ast} \tau \right\} $$

$$ \sim \text{Gamma}(a^\ast, b^\ast) $$

> [!success] This gives us the possibility to **sample** $\pi(\mu, \tau \mid x_1, \dots, x_n)$ **without using its analytical expression**, but using the **full conditionals (1) and (2)**.
> 
> This is exactly an application of **Gibbs sampling**.
> 
> Beautiful result, since we can explore $\pi(\mu, \tau \mid x_1, \dots, x_n)$ without using its complicated bivariate expression.

---

## Software (historical lineage)

```
≈ '90   BUGS      (Bayesian Using the Gibbs Sampler)
            ↓
≈ '00   WinBUGS
            ↓
        OpenBUGS
            ↓
≈ '10     JAGS      (Just Another Gibbs Sampler)
            ↓
≈ '20      STAN  ───────────►  In Python: pgmpy
            ↓                  (Probabilistic Graphical
          Julia                 Models in Python)
```

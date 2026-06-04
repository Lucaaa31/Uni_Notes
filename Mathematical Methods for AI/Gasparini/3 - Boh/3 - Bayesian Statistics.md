## Basic Idea

1. If you **do know something**, you give it a probability.
2. After seeing related data, you **UPDATE** your probability using the conditional distribution given the data you observed.
_"Something" can be:_
- Parameters $\theta$
- Future observations $Y_{n+1}$
- Missing data

---
## The Two Steps

### 1. Prior Distribution

_(assigned "ahead of time, before anything else")_

Assigning **prior distributions** to unknown things, including parameters $\theta$ — differently from frequentist statistics.

In particular, the prior distribution on $\theta$ will be indicated by $\pi(\theta)$.

### 2. Bayesian Updating

Calculating the **conditional distributions** of $\theta$, of $Y_{n+1}$, of missing data… given the data. These are called **posterior distributions**.

---

## General Framework

$$\theta \sim \pi(\theta) \xrightarrow{ X_1, X_2, \ldots, Y_1, \ldots} \theta \mid \text{data} \sim \pi(\theta \mid \text{data})$$

|Step|Name|
|---|---|
|$\theta \sim \pi(\theta)$|Prior assignment|
|Observe $X_1, \ldots, X_n$|Collect data|
|$\theta \mid \text{data} \sim \pi(\theta \mid \text{data})$|Compute posterior|

The data $X_1, X_2, \ldots, Y_1, Y_2, \ldots, Z_1, \ldots$ (capital letters = observable random variables) will have a distribution called the **likelihood** (as before), which depends somehow on $\theta$.

---

## Example (Prototype): Normal Sample with Unknown Mean

**Setup:**

- $X_1, \ldots, X_n \mid \mu \overset{\text{c.i.i.d.}}{\sim} \mathcal{N}(\mu, \sigma^2)$ — this is the **likelihood** (conditionally on $\mu$)
- $\theta = \mu$ only (since $\sigma^2$ is known)

**Prior:** $\mu \sim \pi(\mu)$

> This is new! In the past, $\mu$ was just a fixed unknown — not a random variable.

Having seen data $X_1 = x_1, X_2 = x_2, \ldots, X_n = x_n$ (actual numbers), we want to compute the **posterior density**:

$$\pi_{M \mid x_1,\ldots,x_n}(\mu \mid x_1, \ldots, x_n) \quad \text{simplified to} \quad \pi(\mu \mid x_1, \ldots, x_n)$$

---

## Computing the Posterior: Bayes' Theorem

$$\pi(\theta \mid x_1, \ldots, x_n) = \frac{\overbrace{\pi(\theta, x_1, \ldots, x_n)}^{\text{joint of } \theta \text{ and data}}}{\underbrace{f_{X_1,\ldots,X_n}(x_1,\ldots,x_n)}_{\text{marginal of data}}}$$

$$= \frac{\pi(\theta) f_{X_1,\ldots,X_n \mid \theta}(x_1,\ldots,x_n \mid \theta)}{f_{X_1,\ldots,X_n}(x_1,\ldots,x_n)}$$

Or simply:

$$\pi(\theta \mid x_1,\ldots,x_n) = \frac{\pi(\theta) f(x_1,\ldots,x_n \mid \theta)}{f_{X_1,\ldots,X_n}(x_1,\ldots,x_n)}$$

Or even more simply (dropping the denominator since it doesn't depend on $\theta$):

> [!important] Key Result $$\pi(\theta \mid x_1,\ldots,x_n) \propto \pi(\theta) \cdot f(x_1,\ldots,x_n \mid \theta)$$ $$\text{posterior density} \propto \text{prior density} \times \text{likelihood}$$

---

## Back to the Normal Example

$$\theta = \mu, \qquad X_1,\ldots,X_n \mid \mu \overset{\text{c.i.i.d.}}{\sim} \mathcal{N}(\mu, \sigma^2)$$

**Which prior?** If we use another normal, calculations are easier!
Say
$$\pi(\mu) = \mathcal{N}(\mu_0, \sigma_0^2)$$

**Bayesian representation:** $\tau_0 = \frac{1}{\sigma_0^2}$ (called _prior precision_), $\tau = \frac{1}{\sigma^2}$ (_sample precision_).

So: $$X_i \sim \mathcal{N}\left(\mu, \frac{1}{\tau}\right) \quad \forall i$$

---

## Computation of the Posterior

$$\pi(\mu \mid x_1,\ldots,x_n) \propto e^{-\frac{1}{2\sigma_0^2}(\mu-\mu_0)^2} \cdot e^{-\frac{1}{2\sigma^2}\sum_{i=1}^n (x_i - \mu)^2}$$

Since the denominator does not depend on $\mu$, substitute $\tau, \tau_0$ for $\frac{1}{\sigma^2}, \frac{1}{\sigma_0^2}$ and do some algebra (completion of squares):

$$\propto \exp\left\{-\frac{1}{2}(\tau_0 + n\tau)\left(\mu - \frac{\tau_0\mu_0 + n\tau\bar{x}}{\tau_0 + n\tau}\right)^2\right\}$$

Here you should be able to **recognize** that:

$$\mu \mid x_1,\ldots,x_n \sim \mathcal{N}\left(\frac{\tau_0\mu_0 + n\tau\bar{x}}{\tau_0 + n\tau}, \frac{1}{\tau_0 + n\tau}\right)$$

---

## Alternative Method to Compute the Posterior

### Step 1 — Sufficient statistic

Recognize that $\pi(\mu \mid x_1,\ldots,x_n)$ is the same as $\pi(\mu \mid \bar{x})$, where

$$\bar{x} = \frac{\sum x_i}{n}$$

is a realization of the sample mean $\bar{X} = \frac{\sum X_i}{n}$.

_Why?_ Because:

$$e^{-\frac{1}{2\sigma^2}\sum(x_i-\mu)^2} \propto e^{-\frac{1}{2\sigma^2}(-2n\mu\bar{x} + n\mu^2)}$$

which only depends on $\bar{x}$.

### Step 2 — Joint density of $(\mu, \bar{X})$

$$ \binom{\mu}{\bar{X}} \sim \mathcal{N}_2\left(\binom{\mu_0}{\mu_0} , \begin{pmatrix}\frac{1}{\tau_0} & \frac{1}{\tau_0} \\ \frac{1}{\tau_0} & \frac{1}{n\tau}+\frac{1}{\tau_0}\end{pmatrix}\right)$$

Since:

- $\mu \sim \mathcal{N}\left(\mu_0, \frac{1}{\tau_0}\right)$
- $\bar{X} \mid \mu \sim \mathcal{N}\left(\mu, \frac{1}{n\tau}\right)$

So:

- $E(\mu) = \mu_0$
- $E(\bar{X}) = E(E(\bar{X}\mid\mu)) = E(\mu) = \mu_0$
- $\text{Var}(\mu) = \frac{1}{\tau_0}$
- $\text{Var}(\bar{X}) = E(\text{Var}(\bar{X}\mid\mu)) + \text{Var}(E(\bar{X}\mid\mu)) = \frac{1}{n\tau} + \frac{1}{\tau_0}$

For the covariance: $$\text{Cov}(\mu, \bar{X}) = E(\mu\bar{X}) - E(\mu)E(\bar{X}) = E(E(\mu \bar{x} | \mu)) - \mu_0 \mu_0 = E(\mu \cdot \mu) - \mu_0^2 $$
$$= \text{Var}(\mu) + E(\mu)^2 - \mu_0^2$$
$$= \text{Var}(\mu) = \frac{1}{\tau_0} + \cancel{\mu_0^2} - \cancel{\mu_0^2}$$

Correlation: $$\rho = \frac{\frac{1}{\tau_0}}{\sqrt{\frac{1}{\tau_0}\cdot\left(\frac{1}{n\tau}+\frac{1}{\tau_0}\right)}}$$

Now we can compute the conditional density of $\mu$ given $\bar{X} = \bar{x}$ using the **bivariate normal conditional formula**:

$$E(\theta \mid \bar{x}) = \mu_0 + \frac{n\tau}{\tau_0 + n\tau}(\bar{x} - \mu_0) = \left(1 - \frac{n\tau}{\tau_0+n\tau}\right)\mu_0 + \frac{n\tau}{\tau_0+n\tau}\bar{x}$$

$$\text{Var}(\theta \mid \bar{x}) = \cdots = \frac{1}{\tau_0 + n\tau}$$

---

## Recap

$$\theta \mid \bar{x} = \theta \mid x_1,\ldots,x_n \sim \mathcal{N}\left(\frac{\tau_0\mu_0 + n\tau\bar{x}}{\tau_0 + n\tau}, \frac{1}{\tau_0 + n\tau}\right)$$

_(same as Method 1 ✓)_

---

## Key Observations

### 1. Posterior Precision Adds Up

$$\frac{1}{\text{Var}(\mu \mid x_1,\ldots,x_n)} = \tau_0 + n\tau$$

$$\boxed{\text{posterior precision} = \text{prior precision} + n \times \text{sample precision}} $$

### 2. Posterior Mean is a Weighted Average

$$E(\mu \mid x_1,\ldots,x_n) = \underbrace{\frac{\tau_0}{\tau_0+n\tau}}_{\text{1 – weight}}\mu_0 + \underbrace{\frac{n\tau}{\tau_0+n\tau}}_{\text{weight}}\bar{x}$$

**Posterior mean = weighted average of prior mean and sample mean**

### 3. Large-Sample Behavior

 $$\frac{n\tau}{\tau_0 + n\tau} \underset{n \to \infty}{\to} 1$$

With a lot of data, **only the data speak** — the prior is forgotten.

### 4. Noninformative Prior (mirror image of 3)
 $$\frac{n\tau}{\tau_0 + n\tau} \underset{\tau_0 \to 0}{\to} 1$$
(i.e. $\frac{1}{\sigma_0^2} \to \infty$)
This corresponds to $\mu \sim \mathcal{N}(\mu_0, \infty)$ — not literally admissible, but it is the limiting scenario of a completely **noninformative prior**.

---

## Graphical Representation

The example can be represented as a **directed acyclic graph (DAG)**:

```
       (μ)           ← Bayes theorem applies here
      / | \
    (X₁)(X₂)…(Xₙ)
```

Or using **plate notation** (for loops):

```
    (μ)
     |
  ┌──────┐
  │ (Xᵢ) │  i = 1,…,n
  └──────┘
```
# Bayesian Statistics

> [!abstract] Core idea
> Bayesian statistics makes **complete use of probability**.

**Historical note:** Bayes / Pierce ($\approx 1770$), later rediscovered by Savage ($\approx 1950$) and De Finetti ($\approx 1925$).

## Basic idea

1. If you **do know something**, you give it a probability.
2. After seeing related data, you **UPDATE** your probability using the conditional distribution given the data you observed.

The "something" can be:
- **parameters** $\theta$
- **future observations** $Y_{n+1}$
- **missing data**

### Two steps

1. **Assigning prior distributions** to unknown things (including parameters $\theta$) *ahead of time, before anything else*. This differs from the frequentist statistics done so far. The prior distribution on $\theta$ is indicated by $\pi(\theta)$.
2. **Bayesian updating** — simply the calculation of conditional distributions (of $\theta$, of $Y_{n+1}$, of missing data, …) given the data. These are called **posterior distributions**.

---

## Example: inference about an unknown parameter $\theta$

> *(only God knows $\theta$)*

$$
\underbrace{\theta \sim \pi(\theta)}_{\text{prior assignment}}
\quad\xrightarrow{\;\;\text{collect data } X_1, X_2, \dots, Y_1, \dots\;\;}\quad
\underbrace{\theta \mid \text{data} \sim \pi(\theta \mid \text{data})}_{\text{compute posteriors}}
$$

The data $X_1, X_2, \dots, Y_1, Y_2, \dots, Z_1, \dots$ (we use **capital letters** to denote observable random variables, i.e. data) will have a distribution — called the **likelihood**, as before — which is dependent somehow on $\theta$.

---

## Prototype example: normal sample with unknown mean, known $\sigma^2$

$$
X_1, \dots, X_n \overset{\text{c.i.i.d.}}{\sim} N(\mu, \sigma^2)
\qquad \theta = \mu \text{ only, since } \sigma^2 \text{ is known}
$$

- The "c.i.i.d." means **conditionally i.i.d. on $\mu$**; this is the **likelihood**.

$$
\mu \sim \pi(\mu) \qquad \text{(the \textbf{prior})}
$$

> [!note] This is new!
> In the past, $\mu$ was just a *fixed unknown* — not a random variable.

Now, having seen data $X_1 = x_1, X_2 = x_2, \dots, X_n = x_n$ (numbers — actual data!), we want to compute the **posterior density**:

$$
\pi_{M \mid X_1 \dots X_n}(\mu \mid x_1, \dots, x_n)
\quad\text{simplified to}\quad
\pi(\mu \mid x_1, \dots, x_n)
$$

---

## Bayes' theorem: computing the posterior

In general, the posterior is computed using **Bayes' theorem**:

$$
\pi(\theta \mid x_1, \dots, x_n)
= \frac{\overbrace{\pi(\theta,\, x_1, \dots, x_n)}^{\text{joint of } \theta \text{ and data}}}
       {\underbrace{f_{X_1 \dots X_n}(x_1, \dots, x_n)}_{\text{marginal of data}}}
$$

$$
= \frac{\pi(\theta)\, f_{X_1 \dots X_n \mid \theta}(x_1, \dots, x_n \mid \theta)}
       {f_{X_1 \dots X_n}(x_1, \dots, x_n)}
$$

or simply (writing the likelihood as $f(x_1, \dots, x_n \mid \theta)$):

$$
\pi(\theta \mid x_1, \dots, x_n)
= \frac{\pi(\theta)\, f(x_1, \dots, x_n \mid \theta)}
       {f_{X_1 \dots X_n}(x_1, \dots, x_n)}
$$

or, even more simply:

> [!important] Posterior $\propto$ prior $\times$ likelihood
> $$
> \pi(\theta \mid x_1, \dots, x_n) \;\propto\; \pi(\theta)\, f(x_1, \dots, x_n \mid \theta)
> $$
> $$
> \underbrace{\text{posterior density}}_{} \;\propto\; \underbrace{\text{prior density}}_{} \times \underbrace{\text{likelihood}}_{}
> $$

---

## Back to the normal example

$$
\theta = \mu, \qquad X_1, \dots, X_n \mid \mu \overset{\text{c.i.i.d.}}{\sim} N(\mu, \sigma^2)
$$

$$
\mu \sim \pi(\mu) \quad \text{— which prior?}
$$

> If we use **another normal**, calculations are easier!

Say:

$$
\pi(\mu) = N(\mu_0, \sigma_0^2)
$$

**Bayesian-like re-parameterization** (precision):

$$
\tau_0 = \frac{1}{\sigma_0^2} \qquad (\tau_0 \text{ is the \textbf{prior precision}})
$$

Similarly, for the sample:

$$
X_i \sim N(\mu, \sigma^2) = N\!\left(\mu, \tfrac{1}{\tau}\right) \quad \forall i
\qquad (\tau \text{ is the \textbf{sample precision}})
$$

### Computation of the posterior

$$
\pi(\mu \mid x_1, \dots, x_n)
= \frac{\dfrac{1}{\sqrt{2\pi\sigma_0^2}}\, e^{-\frac{1}{2\sigma_0^2}(\mu - \mu_0)^2}
        \displaystyle\prod_{i=1}^{n} \dfrac{1}{\sqrt{2\pi\sigma^2}}\, e^{-\frac{1}{2\sigma^2}(x_i - \mu)^2}}
       {\displaystyle\int \dfrac{1}{\sqrt{2\pi\sigma_0^2}}\, e^{-\frac{1}{2\sigma_0^2}(m - \mu_0)^2}
        \prod_{i=1}^{n} \dfrac{1}{\sqrt{2\pi\sigma^2}}\, e^{-\frac{1}{2\sigma^2}(x_i - m)^2}\, dm}
$$

Since the **denominator does not depend on $\mu$**:

$$
\propto e^{-\frac{1}{2\sigma_0^2}(\mu - \mu_0)^2}\; e^{-\frac{1}{2\sigma^2}\sum_{i=1}^{n}(x_i - \mu)^2}
$$

Substitute $\tau, \tau_0$ for $\tfrac{1}{\sigma^2}, \tfrac{1}{\sigma_0^2}$ and do some algebra (**completion of squares**):

$$
\propto \exp\!\left\{ -\frac{1}{2}\underbrace{(\tau_0 + n\tau)}_{\text{variance}^{-1}}
\left( \mu - \underbrace{\frac{\tau_0 \mu_0 + n\tau \bar{x}}{\tau_0 + n\tau}}_{\text{mean}} \right)^{\!2} \right\}
$$

You should be able to **recognize** that:

$$
\boxed{\;\mu \mid x_1, \dots, x_n \sim N\!\left( \frac{\tau_0 \mu_0 + n\tau \bar{x}}{\tau_0 + n\tau},\; \frac{1}{\tau_0 + n\tau} \right)\;}
$$

since we know how to recognize a normal density.

---

## Alternative method

To compute the posterior of $\mu$ given $x_1, \dots, x_n$, do the following:

### 1. Reduce to the sample mean

Recognize that $\pi(\mu \mid x_1, \dots, x_n)$ is the same as $\pi(\mu \mid \bar{x})$, where

$$
\bar{x} = \frac{\sum x_i}{n} \quad \text{is a realization of} \quad \bar{X} = \frac{\sum X_i}{n}, \text{ the sample mean.}
$$

This is because:

$$
e^{-\frac{1}{2\sigma^2}\sum(x_i - \mu)^2}
= e^{-\frac{1}{2\sigma^2}\left(\sum x_i^2 - 2\mu \overbrace{\sum x_i}^{n\bar{x}} + n\mu^2\right)}
\propto e^{-\frac{1}{2\sigma^2}(-2n\mu\bar{x} + n\mu^2)}
$$

which **only depends on $\bar{x}$**.

### 2. Use the joint density of $(\mu, \bar{X})'$

$$
\begin{pmatrix} \mu \\ \bar{X} \end{pmatrix}
\sim N_2\!\left(
\begin{pmatrix} \mu_0 \\ \mu_0 \end{pmatrix},\;
\begin{pmatrix} \tfrac{1}{\tau_0} & \tfrac{1}{\tau_0} \\[4pt] \tfrac{1}{\tau_0} & \tfrac{1}{n\tau} + \tfrac{1}{\tau_0} \end{pmatrix}
\right)
$$

since

$$
\mu \sim N\!\left(\mu_0, \tfrac{1}{\tau_0}\right), \qquad
\bar{X} \mid \mu \sim N\!\left(\mu, \tfrac{1}{n\tau}\right)
$$

**Computing the moments:**

$$
E(\mu) = \mu_0
$$

$$
E(\bar{X}) = E\big(E(\bar{X} \mid \mu)\big) = E(\mu) = \mu_0 \qquad \text{(by tower property)}
$$

$$
\mathrm{Var}(\mu) = \frac{1}{\tau_0}
$$

$$
\mathrm{Var}(\bar{X}) = E\big(\mathrm{Var}(\bar{X} \mid \mu)\big) + \mathrm{Var}\big(E(\bar{X} \mid \mu)\big)
= \frac{1}{n\tau} + \frac{1}{\tau_0}
$$

$$
\begin{aligned}
\mathrm{Cov}(\mu, \bar{X})
&= E(\mu \bar{X}) - E(\mu)E(\bar{X}) \\
&= E\big(E(\mu \bar{X} \mid \mu)\big) - \mu_0 \cdot \mu_0 \\
&= E(\mu \cdot \mu) - \mu_0^2 \\
&= \mathrm{Var}(\mu) + E(\mu)^2 - \mu_0^2 \\
&= \frac{1}{\tau_0} + \cancel{\mu_0^2} - \cancel{\mu_0^2}
= \frac{1}{\tau_0}
\end{aligned}
$$

**Correlation:**

$$
\rho = \frac{\tfrac{1}{\tau_0}}{\sqrt{\tfrac{1}{\tau_0}\left(\tfrac{1}{n\tau} + \tfrac{1}{\tau_0}\right)}}
$$

### Conditional density for bivariate normal

Now we know how to compute the conditional density of $\mu$ given $\bar{X}$ using the formula for the bivariate normal:

$$
E(\theta \mid \bar{x}) = \mu_0 + \frac{n\tau}{\tau_0 + n\tau}(\bar{x} - \mu_0)
= \underbrace{\left(1 - \frac{n\tau}{\tau_0 + n\tau}\right)}_{=\;\tau_0 / (\tau_0 + n\tau)} \mu_0 + \frac{n\tau}{\tau_0 + n\tau}\bar{x}
$$

$$
\mathrm{Var}(\theta \mid \bar{x}) = \dots = \frac{1}{\tau_0 + n\tau}
$$

> [!info] General bivariate-normal conditional formula
> $$
> N\!\left( \mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(x - \mu_X),\; (1 - \rho^2)\sigma_Y^2 \right)
> $$
> with $Y = \theta$, $X = \bar{X}$, and
> $$
> \rho\frac{\sigma_Y}{\sigma_X}
> = \frac{\tfrac{1}{\tau_0}}{\sqrt{\tfrac{1}{\tau_0}\left(\tfrac{1}{\tau_0} + \tfrac{1}{n\tau}\right)}}
>   \cdot \frac{\sqrt{\tfrac{1}{\tau_0}}}{\sqrt{\tfrac{1}{\tau_0} + \tfrac{1}{n\tau}}}
> = \frac{\tfrac{1}{\tau_0}}{\tfrac{1}{\tau_0} + \tfrac{1}{n\tau}}
> = \frac{n\tau}{\tau_0 + n\tau}
> $$

---

## Recap

$$
\boxed{\;\theta \mid \bar{X} = \theta \mid x_1, \dots, x_n \sim N\!\left( \frac{\tau_0 \mu_0 + n\tau \bar{x}}{\tau_0 + n\tau},\; \frac{1}{\tau_0 + n\tau} \right)\;}
$$

as in method 1.

### Some very important things to notice

> [!tip] 1. Posterior precision
> $$
> \frac{1}{\mathrm{Var}(\mu \mid x_1, \dots, x_n)} = \frac{1}{\frac{1}{\tau_0 + n\tau}} = \tau_0 + n\tau
> $$
> $$
> \underbrace{\text{posterior precision}}_{} = \underbrace{\text{prior precision}}_{\tau_0} + n\,\underbrace{\text{sample precision}}_{\tau} \quad \text{:)}
> $$

> [!tip] 2. Posterior mean (a sort of Bayesian estimator of $\theta$)
> $$
> E(\mu \mid x_1, \dots, x_n) = \underbrace{\frac{\tau_0}{\tau_0 + n\tau}}_{1-\text{weight}}\,\underbrace{\mu_0}_{\text{prior mean}} + \underbrace{\frac{n\tau}{\tau_0 + n\tau}}_{\text{weight}}\,\underbrace{\bar{x}}_{\text{sample mean}}
> $$
> The **posterior mean is a weighted average of the prior mean and the sample mean!**

The weight of the sample mean:

$$
\frac{n\tau}{\tau_0 + n\tau} \xrightarrow[n \to \infty]{} 1
$$

> *With a lot of data, only the data speak and the prior is forgotten.*

> [!tip] 3. The mirror image of (2)
> This is the situation $\tau_0 \to 0$:
> $$
> \frac{n\tau}{\tau_0 + n\tau} \xrightarrow[\tau_0 \to 0]{} 1
> $$
> That is the situation $\frac{1}{\sigma_0^2} \to \infty$, i.e. **infinite prior variance**:
> $$
> \mu \sim N(\mu_0, \infty)
> $$
> This is **not literally admissible**, but it is a limiting scenario of a completely **NONINFORMATIVE PRIOR**.

---

## Graphical representation

Finally, the example can be represented as a **directed graphical model**:

```
         (μ)                              (μ)
        / | \                              |
       /  |  \              or            ▼
      ▼   ▼   ▼                      ┌──────────┐
    (X₁)(X₂)…(Xₙ)                    │   (Xᵢ)   │
                                     └──────────┘
                                        i = 1…n
```

- The arrows from $\mu$ to the $X_i$ encode **Bayes' theorem**.
- The plate (box) on the right is **"sheet" notation for loops** ($i = 1, \dots, n$).

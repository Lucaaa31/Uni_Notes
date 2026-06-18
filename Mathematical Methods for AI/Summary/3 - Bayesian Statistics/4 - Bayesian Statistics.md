   # Bayesian Statistics — Summary

- **Prior:** belief assigned to unknown quantities *before* observing data, denoted $\pi(\theta)$
- **Likelihood:** distribution of the observable data, dependent on $\theta$
- **Posterior:** updated belief about the unknown *after* observing data, $\pi(\theta \mid \text{data})$
- **Bayesian updating:** computing conditional distributions given the data

$$\text{Posterior} \propto \text{Prior} \times \text{Likelihood}$$

---

### The Bayesian Framework

> **Context:** Bayesian statistics makes complete use of probability. Anything unknown — parameters $\theta$, future observations $Y_{n+1}$, or missing data — is assigned a prior probability ahead of time. After seeing related data, beliefs are updated through the conditional distribution given the observations.

> [!definition] Prior and Posterior
> 
> The unknown $\theta$ is treated as a random variable with a **prior** distribution $\pi(\theta)$. After collecting data, the **posterior** is the conditional distribution of $\theta$ given the data:
> 
> $$\theta \sim \pi(\theta) \quad\xrightarrow{\text{collect data}}\quad \theta \mid \text{data} \sim \pi(\theta \mid \text{data})$$
> 
> _The prior differs from frequentist statistics, where parameters are fixed unknowns rather than random variables._

---

### Bayes' Theorem

> **Context:** The posterior is computed via Bayes' theorem, as the joint density of parameter and data divided by the marginal of the data. Since the marginal does not depend on $\theta$, it acts only as a normalizing constant.

> [!theorem] Posterior via Bayes' Theorem
> 
> $$\pi(\theta \mid x_1, \dots, x_n) = \frac{\pi(\theta)\, f(x_1, \dots, x_n \mid \theta)}{f_{X_1 \dots X_n}(x_1, \dots, x_n)}$$
> 
> Dropping the normalizing denominator:
> 
> $$\boxed{\;\pi(\theta \mid x_1, \dots, x_n) \;\propto\; \pi(\theta)\, f(x_1, \dots, x_n \mid \theta)\;}$$
> 
> $$\underbrace{\text{posterior density}}_{} \;\propto\; \underbrace{\text{prior density}}_{} \times \underbrace{\text{likelihood}}_{}$$

---

### Normal Model with Known Variance

> **Context:** Prototype example — a normal sample with unknown mean $\mu$ and known variance $\sigma^2$. Choosing a normal prior on $\mu$ makes the calculations tractable (conjugate setup). Working with precisions $\tau = 1/\sigma^2$ simplifies the algebra.

> [!definition] Model and Prior
> 
> $$X_1, \dots, X_n \mid \mu \overset{\text{i.i.d.}}{\sim} N(\mu, \sigma^2), \qquad \theta = \mu \text{ (since } \sigma^2 \text{ known)}$$
> 
> Normal prior on the mean:
> 
> $$\mu \sim N(\mu_0, \sigma_0^2)$$
> 
> **Precision reparameterization:**
> 
> $$\tau_0 = \frac{1}{\sigma_0^2} \text{ (prior precision)}, \qquad \tau = \frac{1}{\sigma^2} \text{ (sample precision)}$$

---

### Normal–Normal Posterior

> **Context:** Multiplying the normal prior by the normal likelihood and completing the square yields a normal posterior — confirming conjugacy. The posterior depends on the data only through the sample mean $\bar{x}$.

> [!theorem] Posterior Distribution of $\mu$
> 
> $$\boxed{\;\mu \mid x_1, \dots, x_n \sim N\!\left( \frac{\tau_0 \mu_0 + n\tau \bar{x}}{\tau_0 + n\tau},\; \frac{1}{\tau_0 + n\tau} \right)\;}$$
> 
> where $\bar{x} = \dfrac{\sum x_i}{n}$ is the sample mean.

---

### Key Properties of the Posterior

> **Context:** The normal–normal posterior reveals two interpretable structural features: precisions add, and the posterior mean is a convex combination of prior and data.

> [!definition] Posterior Precision
> 
> The posterior precision is the sum of the prior precision and $n$ times the sample precision:
> 
> $$\boxed{\;\frac{1}{\mathrm{Var}(\mu \mid x_1, \dots, x_n)} = \tau_0 + n\tau\;}$$
> 
> $$\underbrace{\text{posterior precision}}_{} = \underbrace{\text{prior precision}}_{\tau_0} + n \cdot \underbrace{\text{sample precision}}_{\tau}$$

> [!definition] Posterior Mean (Bayesian Estimator)
> 
> A weighted average of prior mean and sample mean:
> 
> $$E(\mu \mid x_1, \dots, x_n) = \underbrace{\frac{\tau_0}{\tau_0 + n\tau}}_{1-\text{weight}}\,\mu_0 + \underbrace{\frac{n\tau}{\tau_0 + n\tau}}_{\text{weight}}\,\bar{x}$$
> 
> - **Large-sample limit:** as $n \to \infty$, the data weight $\dfrac{n\tau}{\tau_0 + n\tau} \to 1$ — only the data speak, the prior is forgotten.

---

### Non-Informative Prior Limit

> **Context:** The mirror image of the large-sample case is the limit of vanishing prior precision. This corresponds to infinite prior variance and represents a completely non-informative prior.

> [!definition] Non-Informative Limit
> 
> As $\tau_0 \to 0$ (equivalently $\sigma_0^2 \to \infty$):
> 
> $$\frac{n\tau}{\tau_0 + n\tau} \xrightarrow[\tau_0 \to 0]{} 1, \qquad \mu \sim N(\mu_0, \infty)$$
> 
> _This prior is not literally admissible, but is a valid limiting scenario of a completely **non-informative prior**._

---

### Graphical Representation

> **Context:** The model can be represented as a directed graphical model: the parameter $\mu$ generates the observations, with the arrows encoding the dependence structure underlying Bayes' theorem.

```
         (μ)                              (μ)
        / | \                              |
       ▼  ▼  ▼              or             ▼
    (X₁)(X₂)…(Xₙ)                    ┌──────────┐
                                    │   (Xᵢ)   │
                                    └──────────┘
                                       i = 1…n
```

- Arrows from $\mu$ to the $X_i$ encode **Bayes' theorem**.
- The plate (box) on the right is the loop / "sheet" notation for $i = 1, \dots, n$.

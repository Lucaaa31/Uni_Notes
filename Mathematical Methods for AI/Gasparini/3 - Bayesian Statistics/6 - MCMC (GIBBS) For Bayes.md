## Recap 

The graphical model: $\mu, \tau \to \text{data}$.

1. **Condition** on $x_1, \dots, x_n$, the observed data.
2. **Find the posterior** $\pi(\mu, \tau \mid \text{data})$.
3. **Use Gibbs**, since $\mu \mid \text{data}, \tau$ is available and $\tau \mid \text{data}, \mu$ is available.

---
## Generalizing the idea

> [!important] This idea can be widely used for much more complex **Bayesian Networks**, where:
> 
> 1. Some of the nodes represent **unknown parameters** (or missing values, or hypotheses).
> 2. Some of the nodes will be **observed** (the data!) and we **condition on** them.
> 3. We use **MCMC** (Gibbs, Metropolis, Variational, …) to **visit the posterior distribution of (1) given (2)**.

![[7 - MCMC (GIBBS) For Bayes-1780587193151.webp]]

---

## Example: the RATS hierarchical model

$$ Y_{ij} = \text{weight of rat } i \text{ at age } x_{ij} $$
![[7 - MCMC (GIBBS) For Bayes-1780587259507.webp]]

> [!question] Why is this not simple regression? 
> Because there are **multiple observations related to the same rat**; these are **not independent**. Instead, **different rats may be considered independent**.

### Likelihood

$$ Y_{ij} \sim \text{Normal}\left( \underbrace{\alpha_i + \beta_i (x_{ij} - \bar{x})}_{\mu_{ij}},\ \tfrac{1}{\tau_c} \right) $$
![[7 - MCMC (GIBBS) For Bayes-1780587344857.webp|678]]
> [!tip] $\alpha_i$ and $\beta_i$ **change from rat to rat**.

### Graphical model (BUGS-style)
![[7 - MCMC (GIBBS) For Bayes-1780587367224.webp]]
> [!note] All external nodes refer to **hyperparameters**, i.e. characteristics of the distributions of $\alpha_i, \beta_i$.

Annotations on the diagram:

- **`mu[i,j]`** is a **deterministic transformation** (as opposed to `◄` conditional dependence) — it is a deterministic node, not a random one.
- **`x[j]`** is a **constant node**, as opposed to a random node.
- The plate `for (j IN 1 : T)` and the outer plate `for (i IN 1 : N)`.
- **Two innovations in BUGS w.r.t. usual Bayesian Networks:** deterministic nodes and constant nodes.

---

## Completing the model — random effects

The previous equation $$ Y_{ij} \sim N\left( \alpha_i + \beta_i (x_{ij} - \bar{x}),\ \tfrac{1}{\tau_c} \right) $$ must be completed with, for $i = 1, \dots, N$:

$$ \alpha_i \overset{ciid}{\sim} N\left( \alpha_c,\ \tfrac{1}{\tau_\alpha} \right) \qquad \beta_i \overset{ciid}{\sim} N\left( \beta_c,\ \tfrac{1}{\tau_\beta} \right) $$

> [!definition] Random Effects
> - $\alpha_i$ — **random intercept**
> - $\beta_i$ — **random slope**
> - $\alpha_c$ — **overall mean intercept**, $\beta_c$ — **overall mean slope** (due to similarities among rats).
> 
> Random effects $\alpha_i$ and $\beta_i$ are called **random effects** (each rat has its own intercept and slope).

---

## What we actually want to estimate

 We are **not** interested in a single rat, but rather in the **average intercept** $\alpha_c$ and **average slope** $\beta_c$.

In addition, we want to know about $$ \sigma = \frac{1}{\sqrt{\tau_c}} $$ for **uncertainty quantification**.

> [!note] $\sigma$ is the spread of each rat's measurements around its own private regression line — assumed **constant and unknown**.

---
## Bayesian updating

Having set this large **hierarchical Bayesian model**, we now proceed to **Bayesian updating**, i.e. include the actual data $Y_{ij} \to y_{ij}$.

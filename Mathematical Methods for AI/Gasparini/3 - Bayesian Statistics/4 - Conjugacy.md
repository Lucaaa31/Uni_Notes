
> [!definition] Conjugacy
> **Conjugacy**: when the posterior is in the same class as the prior.

## Example: Normal Sample with Known Variance

_(recap from last class)_

**Setup:** $$X_1, \ldots, X_n \overset{\text{c.i.i.d.}}{\sim} \mathcal{N}(\mu, \sigma^2) \quad \text{with } \sigma^2 \text{ known}$$

**Prior:** $\mu \sim \mathcal{N}(\mu_0, \sigma_0^2)$

**Posterior:** $\mu \mid x_1, \ldots, x_n \sim \mathcal{N}(\mu_n, \sigma_n^2)$

> [!note] Conjugacy — it simplifies calculations (no deep meaning).

---

## Counterexample: Normal Sample with Unknown Variance

If $\sigma^2$ is **unknown**, things get more complicated.

Suppose $\mu$ and $\sigma^2$ are **independent a priori**:

**DAG:**

```
   (μ)   (σ²)
     \   /
    ┌──────┐
    │ (Xᵢ) │  i=1,…,n
    └──────┘
```

**Setup:** $$X_1, \ldots, X_n \overset{\text{c.i.i.d.}}{\sim} \mathcal{N}\left(\mu,, \sigma^2 = \frac{1}{\tau}\right)$$

$$\text{independent} \rightarrow \begin{cases}\mu \sim \mathcal{N}(\mu_0, \sigma_0^2) \\ \sigma^2 \sim \text{Gamma}(a, b)\end{cases} $$
![[Conjugacy-1780580695313.webp]]

_(a possibility, since $\sigma^2$ lives on the positive axis)_

Then we **joint prior on $(\mu, \sigma^2)$:**

$$\pi(\mu, \sigma^2) \propto \underbrace{\exp\left\{-\frac{1}{2\sigma_0^2}(\mu-\mu_0)^2\right\}}_{\text{kernel of normal prior on } \mu} \cdot \underbrace{(\sigma^2)^{a-1} e^{-b\sigma^2}}_{\text{kernel of gamma prior on } \sigma^2}$$

**Posterior:**
$$\pi(\mu, \sigma^2 \mid x_1,\ldots,x_n) \propto \exp\left\{-\frac{1}{2\sigma_0^2}(\mu-\mu_0)^2\right\} (\sigma^2)^{a-1} e^{-b\sigma^2}  (\sigma^2)^{-n/2} \prod_{i=1}^n \exp\left\{-\frac{1}{2\sigma^2}(x_i - \mu)^2\right\}$$

$\neq$ a product of a normal on $\mu$ × a gamma on $\sigma^2$

→ To explore the posterior, we need **numerical methods**.

![[4 - Conjugacy-1780581444696.webp]]
_(The joint posterior over $(\mu, \sigma^2)$ has an irregular, asymmetric 2D contour shape.)_

---

## Example: Conjugate Prior for the Normal — Inverse-Gamma-Normal

There **is** a conjugate prior: the **inverse-gamma–normal** prior.

**Setup:** $$X_1, \ldots, X_n \overset{\text{c.i.i.d.}}{\sim} \mathcal{N}\left(\mu, \sigma^2 - \frac{1}{\tau}\right)$$

$$\mu \mid \tau \sim \mathcal{N}\left(\mu_0, \frac{1}{\tau\tau_0}\right)$$
$$\tau \sim \text{Gamma}(a, b)$$

**DAG:**

```
    (μ) ← (τ)
        \  /
       ┌──────┐
       │ (Xᵢ) │  i=1,…,n
       └──────┘
```

It is easy to prove that:
$$\tau \mid x_1,\ldots,x_n \sim \text{Gamma}(a_n, b_n)$$ $$\mu \mid \tau, x_1,\ldots,x_n \sim \text{Normal}\left(\mu_n,, \frac{1}{\tau_n}\right)$$
_(where $a_n, b_n, \mu_n, \tau_n$ are to be computed)_

> [!important] **Conjugacy is preserved**, since the structure of the posterior is the same as the prior, i.e. inverse-gamma–normal.

The advantage is that we can **interpret the posterior** as we interpreted the prior. 

---

## Binary Random Sample

$$X_1, \ldots, X_n \overset{\text{c.i.i.d.}}{\sim} \text{Bernoulli}(p)$$

### Discrete Prior (trivial conjugacy)

If $p$ has a **discrete** prior:
$p_1, \ldots, p_K$ are the possible values with respective probabilities $\pi(p_1), \ldots, \pi(p_K)$.

The posterior is:

$$\pi(p_k \mid x_1,\ldots,x_n) = \frac{\pi(p_k)\displaystyle\prod_{i=1}^n p_k^{x_i}(1-p_k)^{1-x_i}}{\displaystyle\sum_{j=1}^K \pi(p_j)\prod_{i=1}^n p_j^{x_i}(1-p_j)^{n-x_i}}, \quad k=1,\ldots,K$$

$$= \frac{\pi(p_k) p_k^{\sum x_i}(1-p_k)^{n-\sum x_i}}{\displaystyle\sum_{j=1}^K \pi(p_j), p_j^{\sum x_i}(1-p_j)^{n-\sum x_i}} \propto \pi(p_k), p_k^{\sum x_i}(1-p_k)^{n-\sum x_i}$$
_(which is the simple discrete form of Bayes' theorem)_

The posterior is discrete on $p_1, \ldots, p_K$ with probabilities $\pi(p_1 \mid x_1,\ldots,x_n),\ldots,\pi(p_K \mid x_1,\ldots,x_n)$.

Trivially, the posterior is conjugate.

> [!note] The likelihood of data $x_1, \ldots, x_n$ is $p_k^{\sum x_i}(1-p_k)^{n-\sum x_i}$.
> 
> For example, the likelihood of data $(0, 1, 0, 0, 1, 1)$ with $n=6$ is:
> 
> $$\mathcal{P}(X_1=0, X_2=1, \ldots, X_6=1) = p_k^0 p_k^1 p_k^0 p_k^0 p_k^1 p_k^1$$
> 
> **Notice:** the posterior would be the same given the **sufficient statistic** $\sum X_i$ (number of successes). 

---

### Continuous Prior: The Beta Distribution

In practice, we prefer to treat $p$ as a **continuous** variable, since the choice of $p_1, \ldots, p_K$ in the discrete prior is too arbitrary:
![[4 - Conjugacy-1780581942781.webp]]

What's a good candidate for $\pi(p)$ on $[0,1]$?

E.g. a uniform: $p \sim U(0,1)$, giving some sense of non-informativeness.

> [!warning] **Caveat:** Uniform on $p$ does **not** mean uniform on $\log\frac{p}{1-p}$, for example. So people using $\log\frac{p}{1-p}$ would not have this interpretation. → **Non-invariance of non-informativeness** of non-informative priors with respect to transformations of the parameter.

We use a prior on $p$ which **includes the uniform as a special case**:

> [!definition] Beta Distribution
> $$p \sim \text{Beta}(a, b)$$
> 
> with density
> 
> $$\pi(p) = \frac{p^{a-1}(1-p)^{b-1}}{B(a,b)}$$
> 
> where $B(a,b) = \displaystyle\int_0^1 t^{a-1}(1-t)^{b-1} dt$ is **Euler's beta function**.
> 
> $B(a,b)$ can be computed for some values of $a, b$, but for most values we have to computer it numerically:
> - $B(1,1) = 1$
> - $B(2,1) = \frac{1}{2}$
> - $B(2.5, 0.3) = ?$ → need a computer!
> $$
> \text{beta density} = \pi(p) = \frac{p^{a - 1} (1 - p)^{b - 1}}{\text{Beta function}}
> $$
> 
> It is easy to prove:
> $$E(p) = \frac{a}{a+b} \qquad \text{Var}(p) = \frac{ab}{(a+b)^2(a+b+1)}$$

#### Shapes of the Beta density
![[4 - Conjugacy-1780582312986.webp|353]]
Often one of these shapes will express your prior uncertainty about $p$.

---

### Beta–Bernoulli Conjugacy

> [!theorem] Beta–Bernoulli Conjugacy
> If: $$X_1, \ldots, X_n \overset{\text{c.i.i.d.}}{\sim} \text{Bernoulli}(p) \qquad p \sim \text{Beta}(a, b)$$
> 
> then:
> 
> $$\pi(p \mid x_1,\ldots,x_n) \propto p^{a-1}(1-p)^{b-1} \cdot p^{\sum x_i}(1-p)^{n-\sum x_i}$$
> 
> $$= p^{(a + \sum x_i) - 1}(1-p)^{(b + n - \sum x_i) - 1} = p^{a_n - 1}(1-p)^{b_n - 1}$$
> 
> which tells us:
> 
> $$\boxed{p \mid x_1,\ldots,x_n \sim \text{Beta}(a_n, b_n)} \qquad \text{(conjugacy)}$$
> 
> where $a_n = a + \sum x_i$ and $b_n = b + n - \sum x_i$.

---

### Posterior Mean (Bayesian Estimate)

$$E(p \mid x_1,\ldots,x_n) = \frac{a_n}{a_n + b_n} = \frac{a + \sum x_i}{a + \sum x_i + b + n - \sum x_i} = \frac{a + \sum x_i}{a + b + n}$$

$$= \frac{a+b}{a+b+n} \cdot \underbrace{\frac{a}{a+b}}_{\substack{\text{prior mean} \ E(p)}} + \frac{n}{a+b+n} \cdot \underbrace{\frac{\sum x_i}{n}}_{\substack{\text{sample mean} \ \text{(MLE of } p)}}$$

> [!important] As in the normal case, the **Bayes estimate is a weighted average** of the prior estimate and the sample estimate, with prior weight $\frac{a+b}{a+b+n} \to 0$ as $n \to \infty$ — the prior is forgotten as data grow.

---

### Noninformative Limit

The same phenomenon (prior forgotten) happens if formally $a = b = 0$, but unfortunately:

$$\pi(p) = \frac{p^{0-1}(1-p)^{0-1}}{B(0,0)} \quad \text{is not a density}$$

> [!note] You can only think of this as a **limit** $a \to 0, b \to 0$, giving $\pi(p) \propto \frac{1}{p(1-p)}$, which diverges at $0$ and $1$ — a U-shaped improper prior.

![[4 - Conjugacy-1780582495087.webp|462]]

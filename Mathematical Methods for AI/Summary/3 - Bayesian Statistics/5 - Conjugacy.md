- **Prior:** initial hypothesis, based on past data
- **Posterior:** actual data obtained after the experiment
- **Conjugacy:** Distribution that links Prior and Posterior
$$\text{Posterior} \propto \text{Verosimiglianza} \times \text{Prior}$$

### The Conjugacy Principle

> **Context:** In Bayesian inference, updating a prior distribution via the likelihood function can lead to mathematically intractable posteriors. When the resulting posterior distribution belongs to the exact same parametric family as the prior, the relationship is termed conjugate, which significantly simplifies sequential updates.

> [!definition] Parametric Conjugacy
> 
> A prior distribution $\pi(\theta)$ is said to be **conjugate** with respect to a specific likelihood function $f(X \mid \theta)$ if the resulting posterior distribution $\pi(\theta \mid X)$ belongs to the identical probability distribution family as the prior:
> 
> $$\pi(\theta) \in \mathcal{F} \implies \pi(\theta \mid X) \in \mathcal{F}$$
> 
> _Note: Conjugacy is an algebraic property of specific prior-likelihood pairs designed to ensure closed-form analytical updates._

### Non-Conjugate Independence Realities

> **Context:** Assuming prior independence between parameters in a multi-parameter model often breaks conjugacy. For instance, in a normal sampling framework where both the mean and variance are unknown, choosing independent prior distributions results in a non-standard, mathematically irregular joint posterior.

```
   (μ)   (σ²)
     \   /
    ┌──────┐
    │ (Xᵢ) │  i=1,…,n
    └──────┘
```

> [!definition] Independent Factored Normal System (Non-Conjugate Counterexample)
> 
> Let a sample follow a normal distribution with an unknown mean $\mu$ and an unknown variance parameterized by precision $\tau = \frac{1}{\sigma^2}$:
> 
> $$X_1, \ldots, X_n \overset{\mathrm{i.i.d.}}{\sim} \mathcal{N}\left(\mu, \sigma^2 = \frac{1}{\tau}\right)$$
> 
> Assuming independent prior structures, $\pi(\mu, \sigma^2) = \pi(\mu)\cdot\pi(\sigma^2)$, such that $\mu \sim \mathcal{N}(\mu_0, \sigma_0^2)$ and $\sigma^2 \sim \mathrm{Gamma}(a, b)$, the joint prior kernel is:
> 
> $$\pi(\mu, \sigma^2) \propto \underbrace{\exp\left\{-\frac{1}{2\sigma_0^2}(\mu-\mu_0)^2\right\}}_{\text{kernel of normal prior on } \mu} \cdot \underbrace{(\sigma^2)^{a-1} e^{-b\sigma^2}}_{\text{kernel of gamma prior on } \sigma^2}$$
> 
> When multiplied by the Gaussian likelihood, the joint posterior kernel takes the non-factorizable form:
> 
> $$\pi(\mu, \sigma^2 \mid X) \propto \exp\left\{-\frac{1}{2\sigma_0^2}(\mu-\mu_0)^2\right\} (\sigma^2)^{a-n/2-1} e^{-b\sigma^2} \prod_{i=1}^n \exp\left\{-\frac{1}{2\sigma^2}(x_i - \mu)^2\right\}$$
> 
> $$\pi(\mu, \sigma^2 \mid X) \neq \pi(\mu \mid X) \times \pi(\sigma^2 \mid X)$$
> 
> _The resulting joint distribution develops an irregular, asymmetric geometry that cannot be expressed as a product of standard independent distributions._

### Restoring Joint Structure

> **Context:** To restore in multi-parameter normal problems, we must account for the structural dependency between the parameters. By conditioning the prior distribution of the mean on the variance or precision parameter, we form a joint Normal-Gamma framework that remains conjugate.

```
    (μ) ← (τ)
        \  /
       ┌──────┐
       │ (Xᵢ) │  i=1,…,n
       └──────┘
```

> [!definition] Joint Normal-Gamma Conjugate Framework
> 
> To preserve parameter family identity when both parameters are unknown, we specify a conditional dependent prior structure where precision $\tau = \frac{1}{\sigma^2}$:
> 
> $$\tau \sim \mathrm{Gamma}(a, b) \qquad \mu \mid \tau \sim \mathcal{N}\left(\mu_0, \frac{1}{\tau \tau_0}\right)$$
> 
> Under this dependency structure, the prior and posterior share the same joint algebraic family:
> 
> $$\tau \mid X \sim \mathrm{Gamma}(a_n, b_n) \qquad \mu \mid \tau, X \sim \mathcal{N}\left(\mu_n, \frac{1}{\tau_n}\right)$$
> 
> where the updated parameters $a_n, b_n, \mu_n, \tau_n$ are functions of the sample data.

### Finite Parameter Spaces

> **Context:** When the parameter space is restricted to a finite set of discrete values, Bayes' theorem can be written using a discrete summation. In this specific scenario, conjugacy is preserved because both the prior and posterior are discrete probability mass functions.

> [!definition] Discrete Parameter Conjugacy
> 
> If a parameter $p$ is restricted to a finite set of discrete candidate values $\{p_1, \ldots, p_K\}$ with prior probabilities $\pi(p_k)$, the discrete posterior formulation following a Bernoulli trial is:
> 
> $$\pi(p_k \mid X) = \frac{\pi(p_k) p_k^{\sum x_i}(1-p_k)^{n-\sum x_i}}{\sum_{j=1}^K \pi(p_j) p_j^{\sum x_i}(1-p_j)^{n-\sum x_i}} \propto \pi(p_k) p_k^{\sum x_i}(1-p_k)^{n-\sum x_i}$$
> 
> The setup is conjugate as the posterior remains a discrete distribution across the identical finite support.

### Bounded Continuous Priors

> **Context:** To model a continuous success probability $p \in [0,1]$ without the arbitrary constraints of a discrete set, we require a flexible continuous distribution bounded on the unit interval. The Beta distribution serves this purpose and acts as the natural conjugate prior for binomial data.

> [!definition] The Beta Distribution
> 
> A continuous probability distribution defined on the bounded interval $[0, 1]$, parameterized by shape hyperparameters $a > 0$ and $b > 0$:
> 
> $$\pi(p) = \frac{p^{a-1}(1-p)^{b-1}}{B(a,b)}$$
> 
> where $B(a,b)$ represents Euler's beta normalization function:
> 
> $$B(a,b) = \int_0^1 t^{a-1}(1-t)^{b-1} \, dt$$
> 
> **Moment Properties:**
> 
> $$\boxed{E(p) = \frac{a}{a+b}} \qquad \boxed{\mathrm{Var}(p) = \frac{ab}{(a+b)^2(a+b+1)}}$$

### The Beta-Bernoulli Update Rule

> **Context:** When modeling continuous independent Bernoulli outcomes, multiplying a Beta prior by the binomial likelihood preserves the polynomial structure of the kernel. This results in a direct conjugate update of the shape hyperparameters.

> [!theorem] Beta-Bernoulli Conjugacy
> 
> Given a sequence of independent Bernoulli observations and a Beta prior distribution on the success parameter $p$:
> 
> $$X_1, \ldots, X_n \overset{\mathrm{i.i.d.}}{\sim} \mathrm{Bernoulli}(p) \qquad p \sim \mathrm{Beta}(a, b)$$
> 
> The analytical posterior distribution is derived as follows:
> 
> $$\pi(p \mid X) \propto \pi(p) \cdot L(p \mid X) \propto \left[ p^{a-1}(1-p)^{b-1} \right] \cdot \left[ p^{\sum x_i}(1-p)^{n-\sum x_i} \right]$$
> 
> Combining exponents yields:
> 
> $$\pi(p \mid X) \propto p^{(a + \sum x_i) - 1}(1-p)^{(b + n - \sum x_i) - 1}$$
> 
> $$\boxed{p \mid X \sim \mathrm{Beta}(a_n, b_n)}$$
> 
> where the updated posterior shape parameters are defined as:
> 
> $$\boxed{a_n = a + \sum_{i=1}^n x_i} \qquad \boxed{b_n = b + n - \sum_{i=1}^n x_i}$$



> **Context:** Evaluating the posterior mean of a Beta-Bernoulli model reveals a core property of conjugate exponential families: the point estimate can be written as a linear combination of the prior belief and the sample data.

> [!definition] Posterior Mean Decomposition
>
> The expected value of the updated Beta posterior distribution can be expanded algebraically:
> $$E(p \mid X) = \frac{a_n}{a_n + b_n} = \frac{a + \sum x_i}{a + b + n}$$
> 
> Factoring this expression isolates the prior mean and the sample Maximum Likelihood Estimator (MLE):
>
> $$E(p \mid X) = \left(\frac{a+b}{a+b+n}\right) \cdot \left[\frac{a}{a+b}\right] + \left(\frac{n}{a+b+n}\right) \cdot \left[\frac{\sum x_i}{n}\right]$$
> 
> - **Prior Mean:** $E(p) = \frac{a}{a+b}$
>     
> - **Sample Mean (MLE):** $\hat{p} = \frac{\sum x_i}{n}$
>     
> - **Asymptotic Convergence:** As the sample size grows ($n \to \infty$), the data weight approaches 1 ($\frac{n}{a+b+n} \to 1$), causing the prior influence to vanish.


### Non-Informative Limits and Parameter Transformation Non-Invariance

> **Context:** Attempting to build an entirely non-informative prior within a parameter family can lead to edge cases. Furthermore, a prior that appears uniform or non-informative for a parameter $p$ will generally not remain uniform if that parameter undergoes a non-linear transformation.

> [!definition] Improper Limits & Transformation Non-Invariance
> 
> - **The Haldane Limit:** Setting the shape parameters $a \to 0, b \to 0$ to eliminate prior weight yields an improper, non-normalizable boundary kernel that diverges at the bounds:
>     
>     $$\pi(p) \propto \frac{1}{p(1-p)} = p^{-1}(1-p)^{-1}$$
>     
> - **Transformation Non-Invariance:** A flat, uniform prior assignment $p \sim \mathcal{U}(0,1)$, which corresponds to $\mathrm{Beta}(1,1)$, does not express uniform uncertainty if the model is reparameterized. For example, applying a log-odds transformation $\psi = \log\left(\frac{p}{1-p}\right)$ shifts the probability mass non-linearly, showing that non-informativeness is dependent on the chosen parameter scale.


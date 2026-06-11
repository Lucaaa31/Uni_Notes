### Pseudo-Random Numbers

> **Context:** True randomness is difficult to achieve in deterministic computer architecture. Instead, systems use algorithms to generate pseudo-random numbers that mimic a continuous Uniform distribution $\mathcal{U}[0,1]$, under the constraint that the sequence is fully replicable given the same starting seed.

> [!definition] Pseudo-Realization and Uniform Approximations
> 
> A number $u = 0.231167$ is not a true random drawing, but a **pseudo-realization** of $\mathcal{U}(0,1)$.
> 
> - **Reproducibility:** Setting a specific **SEED** allows the exact sequence of numbers to be recreated deterministically.
>     
> - **Discrete Approximation:** While $\mathcal{U}[0,1]$ is theoretically continuous, digital computers approximate it via a discrete uniform distribution bounded by finite machine precision (e.g., values spanning from $0.000001$ to $0.999999$ due to decimal limitations).
>     

### Simulating Categorical Variables

> **Context:** To simulate a discrete random variable like a Bernoulli trial using a standard uniform generator, we partition the unit interval $[0,1]$ into segments whose lengths perfectly match the probability parameters of the target distribution.

> [!definition] Bernoulli Simulation Algorithm
> 
> To simulate a random variable $X \sim \text{Bernoulli}(p)$ where $p = 0.2$ using a uniform value $u = \boxed{\text{RND}}$:
> 
> **Standard Approach:**
> 
> $$\begin{cases} \text{if } u \leq 0.2, & \text{assign } x = 1 \\ \text{if } u > 0.2, & \text{assign } x = 0 \end{cases}$$
> 
> **Symmetric Alternative:**
> 
> Because $U$ and $1-U$ share the identical distribution, the algorithm can be written efficiently as:
> 
> $$\begin{cases} \text{if } u > 0.8, & \text{assign } x = 1 \\ \text{if } u \leq 0.8, & \text{assign } x = 0 \end{cases}$$

### The Inverse Transform Concept

> **Context:** The geometric logic used to partition the Bernoulli distribution can be generalized to any continuous or discrete **cumulative distribution function** (funzione di ripartizione) (CDF). By defining a generalized inverse function, known as the quantile function, we can map uniform numbers to any target distribution.

> [!definition] Quantile Function
> 
> The generalized inverse of a cumulative distribution function $F(x)$, which returns the smallest value of $x$ for which the probability exceeds or equals $u$:
> 
> $$\boxed{Q(u) = \inf \{x : F(x) \geq u\}}$$
> 
> _Note: If $F(x)$ is strictly increasing and continuous, the quantile function simplifies to the standard mathematical inverse $Q(u) = F^{-1}(u)$._

### Probability Integral Transform

> **Context:** The mathematical foundation enabling random variate generation is the Probability Integral Transform theorem. It proves that passing a standard uniform random variable through a target quantile function yields an exact realization from that target profile.

> [!theorem] Inversion Method (Inverse Transform Sampling)
> 
> If a random variable $X$ has a continuous cumulative distribution function $F(\cdot)$ and an associated quantile function $Q(\cdot)$, and if $U \sim \mathcal{U}[0,1]$, then the transformed random variable $Y = Q(U)$ shares the exact same cumulative distribution function $F(\cdot)$.
> 
> **Proof:**
> 
> $$F_Y(y) = P(Y \leq y) = P(Q(U) \leq y) = P(U \leq F(y)) = F(y)$$
> 
> _(Since the CDF of a standard uniform variable evaluated at a point $k \in [0,1]$ is simply $P(U \leq k) = k$)._

### Analytical Inversion Applications

> **Context:** When a distribution possesses a continuous CDF that can be algebraically inverted, we can design fast, highly efficient simulation loops that bypass complex numerical approximations.

> [!definition] Closed-Form Continuous Simulation: The Exponential Case
> 
> For a random variable $X \sim \text{Exponential}(\lambda)$, the cumulative distribution function is $F(x) = 1 - e^{-\lambda x}$. Setting $u = F(x)$ allows us to isolate $x$ algebraically:
> 
> $$u = 1 - e^{-\lambda x} \implies 1 - u = e^{-\lambda x} \implies \log(1-u) = -\lambda x \implies x = -\frac{\log(1-u)}{\lambda}$$
> 
> **Optimized Algorithm:**
> 
> 1. Draw a uniform random number $u = \boxed{\text{RND}}$.
>     
> 2. Compute the value using the distributional identity of $u \overset{d}{=} 1-u$:
>     
>     $$\boxed{x = -\frac{\log(u)}{\lambda}}$$
>     

### Software Interface Standards

> **Context:** R organizes statistical distribution tasks using a unified interface notation. Every distribution family shares a generic base keyword, prefixed with a letter indicating whether the task is evaluation, integration, mapping, or generation.

> [!definition] R Statistical Function Nomenclature
> 
> For any specified distribution family (e.g., `norm`, `binom`, `exp`), R provides four structural functions:

|**Function Prefix**|**Meaning**|**Mathematical Notation**|
|---|---|---|
|**`d`**`name`|Density / Probability Mass Function|$f(x)$ or $P(X = x)$|
|**`p`**`name`|Cumulative Distribution Function (CDF)|$F(x) = P(X \leq x)$|
|**`q`**`name`|Quantile Function (Inverse CDF)|$Q(u) = F^{-1}(u)$|
|**`r`**`name`|Random Variate Generation (Simulation)|Draws $n$ i.i.d. realizations|

### Algorithmic Engine Foundations

> **Context:** At the base of software simulation frameworks sits a low-level algorithm tasked with generating the initial deterministic sequence of integers. The classical approach relies on modular integer arithmetic loops to create long cycles of pseudo-random numbers.

> [!definition] Linear Congruential Generator (LCG)
> 
> An algorithmic technique that generates a sequence of pseudo-random integers using a linear modular recurrence relation.
> 
> Given large chosen parameters—**modulo $M$**, **multiplier $a$**, **increment $c$**, and an initial integer **$\text{SEED } \mathcal{L}_0$**—the sequence is computed iteratively:
> 
> $$\mathcal{L}_i = (a \cdot \mathcal{L}_{i-1} + c) \pmod M$$
> 
> The resulting integers are scaled to the open unit interval $(0,1)$ to yield pseudo-uniform values:
> 
> $$\boxed{u_i = \frac{\mathcal{L}_i}{M}}$$
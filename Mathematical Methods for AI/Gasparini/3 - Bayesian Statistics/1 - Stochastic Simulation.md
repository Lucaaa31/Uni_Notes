It involves random variables.
e.g. $\rightarrow$ `0.231167` = a realization of $U(0,1)$ = an instance, a simulation.

**Not exactly:**
1. It's a **pseudo** realization — you can reproduce it deterministically as long as you set the same **SEED**
2. $U \sim U[0,1]$ is a **continuous** variable
    ![[1 - Stochastic Simulation-1780519722441.webp]]
$$f(u) \text{ [flat on } [0,1]\text{]} \quad \leftarrow \text{ this is an ideal}$$

The actual implementation of it is an **approximation**: Discrete Uniform between $0.000001$ and $0.999999$, because I only have 6 digits.

---

## Simulating from a Bernoulli

Suppose you have $\boxed{\text{RND}}$ available. How do you simulate $X \sim \text{Bernoulli}(0.2)$, $p = 0.2$?

> [!example] Algorithm
> 1. get $u = \boxed{\text{RND}}$
> 2. $$ \begin{cases} \text{if } u \leq 0.2, \text{assign } x = 1 \\ \text{if } u > 0.2, \text{assign } x = 0 \end{cases}$$

**Graphically:** 
![[1 - Stochastic Simulation-1780519922256.webp]]
Since $1-U$ has the same distribution as $U$, we can write equivalently the algorithm above as follows:
   a. get $u = \boxed{\text{RND}}$
   b. if $u > 0.8$, assign $X = 1$; if $u \leq 0.8$, assign $x = 0$

Basically, we enter $U \sim U[0,1]$ on the $y$-axis and "invert" the c.d.f. of $X$ to get $X \sim F(x)$.

We need a more general definition of "inverse":

> [!definition] Quantile Function
> $$Q(u) = \inf \{x : F(x) \geq u\}$$
> called the **QUANTILE FUNCTION** _(actual inverse if $F$ is continuous: $Q = F^{-1}$)_

---

## Inversion Method — General Case

It turns out that the "inversion" of the c.d.f. works with **ANY** $F(x)$ and associated inverse $Q(u)$.
![[1 - Stochastic Simulation-1780520153110.webp]]
It turns out that, if $U \sim U[0,1]$, then:
$$Z = Q(U) = \Phi^{-1}(U) \sim \mathcal{N}(0,1) $$

This is a consequence of the following theorem.

---

### Theorem

> [!theorem] Inversion Method
> **If** $X$ has c.d.f. $F(\cdot)$ and quantile function $Q(\cdot)$, and if $U \sim U[0,1]$, **then** $Y = Q(U)$ has c.d.f. $F(\cdot)$.

> [!proof]
> $Y = Q(U)$ is a transformation of the random variable $U$. 
> Let's compute the cdf of $Y$:
> 
> $$F_Y(y) = P(Y \leq y) \overset{\text{def of }T}{=} P(Q(U) \leq y) \overset{\text{def of }Q}{=} P(U \leq F(y)) \overset{\text{Since }Y \text{ is uniform}}{=} F(y)$$

---

## Example: $X \sim \text{Exponential}(\lambda)$
![[1 - Stochastic Simulation-1780520306461.webp]]

> [!example] Explicit Inversion for the Exponential
> (A case where we can do explicit analytical computations)
> 
> In this case $Q$ is the **actual inverse** of $F$ ($Q = F^{-1}$) and can be computed explicitly:
> $$u = 1 - e^{-\lambda x}$$ $$1 - u = e^{-\lambda x}$$ $$\log(1-u) = -\lambda x$$ $$x = -\frac{\log(1-u)}{\lambda}$$
> 
> Therefore the algorithm to simulate $X \sim \text{Exponential}(\lambda)$ is:
> 1. get $u = \boxed{\text{RND}}$
> 2. compute $x = -\dfrac{\log(1-u)}{\lambda}$
> 
> Equivalently, since $u$ and $1-u$ have the same distribution:
>    2*) compute $x = -\dfrac{\log(u)}{\lambda}$

> [!tip] $2^*$ is an **efficiency gain**. 
> Similar ad-hoc tricks are well known in the literature (since the 1940s) for specific distributions.

---

## R Naming Conventions

In practice, in R, for a distribution called `name`:

|Function|Meaning|
|---|---|
|`dname`|density of kind `name`|
|`pname`|cdf of `name`|
|`qname`|quantile function of `name`|
|`rname`|simulation device|

> [!example] R functions for the normal distribution
> - `dnorm(x, μ, σ)` → $f(x) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left\{-\frac{1}{2\sigma^2}(x-\mu)^2\right\}$
> - `pnorm(x, μ, σ)` → $F(x) = \int_{-\infty}^{x} f(x) dx$
> - `qnorm(u, μ, σ)` → is $Q(u)$
> - `rnorm(n, μ, σ)` → is a simulation of $n$ independent realizations of $X_1, \ldots, X_n$ i.i.d. $F$ 

---

## Next Two Problems

1. How to actually code the generation of $U$? What's behind $\boxed{\text{RND}}$?
2. How to simulate **random vectors**

---

## 1) Linear Congruential Generators

> [!definition] Linear Congruential Generator
> To get a realization of $U \sim U[0,1]$, we actually write a **deterministic** code that starts from a **SEED** and produces $u$ that looks as if it were random $U[0,1]$ — but it actually is not, since it can be reproduced using the same code and SEED.
> 
> **(Pseudorandomness)**
> 
> **Idea:** use the remainder of an integer division!
> 
> - Choose (large) integer $M$ and integers $a, c$
> - Choose $\text{SEED} = \mathcal{L}_0$, another integer
> - Define and loop, for $i = 1, 2, \ldots$:
> 
> $$\mathcal{L}_i = (a, \mathcal{L}_{i-1} + c) \bmod M$$
> 
> ("remainder of integer division $(a\mathcal{L}_{i-1}+c) : M$")
> 
> $$= \text{set } u_i = \frac{\mathcal{L}_i}{M}$$
> 
> so that $0 < u_i < 1$ and it behaves like a realization of $U$.

> [!warning] There are **good and bad generators** (depending on $M, a, c$).

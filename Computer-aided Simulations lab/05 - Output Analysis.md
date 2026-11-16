## Simulation results
The simulation:
- receives as input a set of random samples, called **traces**
- produces as outputs, observations of random quantities (**variables**) we are interested in

For each random variable we want to compute:
- an estimation of the **mean value** for the variable
- an **estimation of the error** we commit when we compute the mean value for the variable

## Types of Simulations
---
### Steady-state
The system runs continuously (or at least over a very long period of time).
We want to study long-run properties of the system.
Dependency on the initial condition should be quickly vanish.

$$
X = \lim_{n \rightarrow \infty} \frac{1}{n} \sum^n_{i = 1} x_i
$$
- $X$ is the metric under study
- $x_1, x_2, ..., x_n$ are independent instances of $X$ collected observing the running system over long periods

### Confidence Intervals
> **Estimation of the error** we make when we estimate the average value $X$ of a random quantity using an averaging process over a limited number of samples and replications.

We have to make some assumptions:
- the observed process is **stationary**
- $X$ has average $m$ and variance $s^2$
- The n observations of $X$ are i.i.d variables

### Mean $m$
$m$ is given from the arithmetic mean of the observations:
$$
\hat x = \frac{1}{n} \sum_{i = 1}^n x_i
$$
$$
E[\hat x] = m \qquad Var(\hat x) = \frac{s^2}{n}
$$
From central limit theorem:
$$
\hat x \sim N(m, \frac{s}{\sqrt{n}})
$$
### Variance 
The variance of the estimation decreases with $n$.
$$
z = \frac{\hat x - m}{\frac{s}{\sqrt{n}}}
$$
We use the variable z to evaluate the error in the estimation.
![[05 - Output Analysis-1794750075778.webp|551]]
$$
\begin{align*}
&P\left\{ -z_{1-\alpha/2} \le z \le z_{1-\alpha/2} \right\} = 1-\alpha \\[6pt]
&P\left\{ -z_{1-\alpha/2} \le \frac{\hat{x}-m}{s/\sqrt{n}} \le z_{1-\alpha/2} \right\} = 1-\alpha \\[6pt]
&P\left\{ -z_{1-\alpha/2}\, s/\sqrt{n} \le \hat{x}-m \le z_{1-\alpha/2}\, s/\sqrt{n} \right\} = 1-\alpha \\[12pt]
&\boxed{P\left\{ \hat{x} - z_{1-\alpha/2}\, s/\sqrt{n} \le m \le \hat{x} + z_{1-\alpha/2}\, s/\sqrt{n} \right\} = 1-\alpha}
\end{align*}
$$

The interval
$$
I = [ \hat{x} - z_{1-\alpha/2}\, s/\sqrt{n},  \hat{x} + z_{1-\alpha/2}\, s/\sqrt{n}]
$$
is called **confidence interval**.
$1 - \alpha$ is called **confidence level**, common values are:
- Confidence 90% ($\alpha =0.1$)
- Confidence 95% ($\alpha =0.05$)
- Confidence 99% ($\alpha =0.01$)

### Definition
> If we repeat the procedure many times, random interval $I$ will actually contain the value $m$ we are trying to estimate ($1 - \alpha$) of the times.
![[05 - Output Analysis-1794752909206.webp|580]]

### Standard Deviation $s$
Besides the estimate of the mean, to apply the variable transform providing $z$ or $t$ , we need the estimate of the **standard deviation $s$**.
$$
s^2 = \frac{1}{n-1}\sum_{i=1}^{n}\left(x_i - \hat{x}\right)^2 = \frac{1}{n-1}\left(\sum_{i=1}^{n} x_i^2 - n\hat{x}^2\right)
$$

### Dimensionality
Given an experiment, a higher level of confidence corresponds to a wider confidence interval:![[05 - Output Analysis-1794753094508.webp|562]]

Fixing the level of confidence $1 - \alpha$, to **shrink** the confidence interval we **must add new samples**, incrementing $n$.
![[05 - Output Analysis-1794753194559.webp|578]]

The central limit theorem is valid only for large $n$.

When $n < 30$, we must substitute the normal distribution with the Student’s $t$ distribution with $n-1$ degrees of freedom.

We define the variable $t$:
$$
t = \frac{\hat x - m}{s / \sqrt{n}}
$$

## Student's Distribution $t$
If $W$ is a random variable with $N(0, 1)$ and V is chi-squared distributed with $k$ degrees of freedom, the ratio
$$
\frac{W}{\sqrt{V/k}}
$$
is **$t$ distributed with $k$ degrees of freedom.**

### Density
$$
f(x) = \frac{1}{B\left(1/2,\, k/2\right)\sqrt{k}} \left(1 + \frac{x^2}{k}\right)^{-\left(\frac{k+1}{2}\right)}
$$
where $B(\alpha,\beta)$ is the Beta function
$$
B(\alpha,\beta) = \int_0^1 t^{\alpha-1}(1-t)^{\beta-1}\,dt
$$
This distribution has mean $0$ and variance $\frac{k}{k-2}$.


# Overview
---
Simulators need generators of random variables with specific distributions.
The procedure is:
1. We generate a sequence $X$ of integer random numbers
2. Using $X$, we generate a sequence $U$ of instances of a random variable uniform in $[0,1]$
3. Using $U$, we generate instances of the chosen random variable

# Inverse-transform technique
---
We want to generate instances of a random variable $X$ with **cumulative distribution $F(x)$ (funzione di ripartizione)**.

The inverse-transform technique is based on the fact that $U=F(X)$ is a r.v. uniformly distributed in $[0,1]$.

Therefore, we can generate an instance of $u$ uniform and then compute
$$
X = F^{-1} (U)
$$

## Theory
- Assume $U$ uniformly distributed in $[0,1]$; 
	- i.e., $P(U \le x) = F_U(x) = x, \quad \forall x \in (0,1)$; 
- Assume $F_X(x)$ to be a strictly increasing (invertible) cumulant; 

Now, $\forall x$:$$ \begin{aligned} P(X \le x) &= P\left(F_X^{-1}(U) \le x\right) = P\left(U \le F_X(x)\right) = \\ &= F_U\left(F_X(x)\right) = F_X(x) \end{aligned} $$
![[07 - Random-Variable Generators-1794841353367.webp|465]]

- If $\Delta y_1 = \Delta y_2$, the probability of generating an instance of $X$ in $\Delta x_1$ or in $\Delta x_2$ is the same 
- The density of samples in $\Delta x_1$ is larger than those in $\Delta x_2$, because $\Delta x_1$ is narrower

Intervals on the $x$ axis are narrower if the derivative of $F(x)$ is higher, i.e. $f(x)$ is larger:
![[07 - Random-Variable Generators-1794841607679.webp|562|429x204]]

## Exponential
We want to generate instances of a random variable with exponential distribution and rate $a$, $x≥0$:
$$
f(x) = a e^{-ax} \quad F(x) = 1 - e^{-ax}
$$
![[07 - Random-Variable Generators-1794842495543.webp|557|491x232]]

### Inverse formula
The inverse of $F(x)$ is
$$
\begin{align}
y &= 1 - e^{-ax} \\
e^{-ax} &= 1 - y \\
-ax &= \ln(1 - y) \\
x &= -\frac{1}{a} \ln(1 - y)
\end{align}
$$

### Generation
1. Generate $u = U(0, 1)$
2. Calculate $x = -\frac{1}{a} \ln(1 - u)$
3. Return $x$

Since both $u$ and $1-u$ are uniformly distributed in $(0,1)$, $x$ can also be calculated as
$$
x = - \frac{1}{a} \ln(u)
$$

## Uniform
We want to generate instances of a uniform random variable with support $[a,b]$
$$
f(x) = \frac{1}{b-a} \quad F(x) = \frac{x-a}{b-a}, \quad a \le x \le b
$$
![[07 - Random-Variable Generators-1794843170337.webp|520]]

### Inverse formula
The inverse of $F(x)$ is
$$
\begin{align}
y &= \frac{x - a}{b - a} \\
(b - a)\,y &= x - a \\
x &= a + (b - a)\,y
\end{align}
$$

### Generation
1. Generate $u = U(0, 1)$
2. Calculate $x = a + (b-a)u$
3. Return $x$

## Pareto
Pareto distribution (k=1):

$$f(x) = a\,x^{-(a+1)} \quad F(x) = 1 - x^{-a}, \quad x \ge 1$$

### Inverse formula  
The inverse of $F(x)$ is

$$\begin{aligned} y &= 1 - x^{-a} \\ x^{-a} &= 1 - y \\ x &= \frac{1}{(1 - y)^{1/a}} \end{aligned}$$

### Generation
1. Generate $u = U(0, 1)$
2. Calculate $x = \dfrac{1}{u^{1/a}}$
3. Return $x$

## Weibull
Weibull distribution:

$$F(x) = 1 - e^{-(x/a)^b}$$

### Inverse formula
The inverse of $F(x)$ is

$$\begin{aligned} y &= 1 - e^{-(x/a)^b} \\ e^{-(x/a)^b} &= 1 - y \\ -\left(\frac{x}{a}\right)^b &= \ln(1 - y) \\ x &= a\left[-\ln(1 - y)\right]^{1/b} \end{aligned}$$

### Generation
1. Generate $u = U(0, 1)$
2. Calculate $x = a\left(-\ln(u)\right)^{1/b}$
3. Return $x$

## Bernoulli  
Bernoulli distribution:

$$P(X = x_1) = p, \quad P(X = x_2) = 1 - p, \quad 0 < p < 1$$

It represents events with two possible outcomes (success/failure, true/false).

### Generation
1. Generate $u = U(0, 1)$
2. If $u \le p$ return $x = x_1$
3. Otherwise return $x = x_2$

![[07 - Random-Variable Generators-1794844003826.webp|245]]

## General discrete distributions
We want to generate instances of a random variable $X$ that holds the values $x_1, x_2, \dots, x_k$ with probability $p_1, p_2, \dots, p_k$.

The cumulative distribution is empirically derived by the measured or approximated values.
$$
\begin{array}{|c|c|c|c|c|c|} \hline \text{x values} & x_1 & x_2 & x_3 & \dots & x_k \\ \hline \text{Probability} & p_1 & p_2 & p_3 & \dots & p_k \\ \hline \text{CDF, } F(x) & p_1 & p_1 + p_2 & p_1 + p_2 + p_3 & \dots & 1 \\ \hline \end{array}
$$

### Generation
1. Generate $u=U(0,1)$
2. Return $x$, according to which condition is satisfied by $u$ in the following table
$$
\begin{array}{|c|c|c|c|c|} \hline \text{Returned value of x} & x_1 & x_2 & \dots & x_k \\ \hline \text{Condition} & 0 < u \le p_1 & p_1 < u \le p_1 + p_2 & \dots & p_1 + \dots + p_{k-1} < u \le 1 \\ \hline \end{array}
$$

## Geometric
Geometric distribution:

$$P(X = n) = p\,(1-p)^n, \quad n = 0, 1, 2, \dots, \quad 0 < p < 1$$

The CDF is
$$P(X \le n) = 1 - (1-p)^{n+1}$$

### Inverse formula
Extending the domain of the function to $\mathbb{R}$, the inverse of $F(x)$ is

$$\begin{aligned} y &= 1 - (1-p)^{x+1} \\ (1-p)^{x+1} &= 1 - y \\ (x+1)\ln(1-p) &= \ln(1 - y) \\ x &= \frac{\ln(1-y)}{\ln(1-p)} - 1 \end{aligned}$$

Since $X$ is discrete, we round up to the next integer (and replace $1-y$ with $u$):
$$
x = \left\lceil \frac{\ln(u)}{\ln(1-p)} - 1 \right\rceil
$$
![[07 - Random-Variable Generators-1794850040639.webp|393]]

### Generation
1. Generate $u = U(0, 1)$
2. Calculate $x = \left\lceil \dfrac{\ln(u)}{\ln(1-p)} - 1 \right\rceil$
3. Return $x$

For the geometric with probability $f(x) = p\,(1-p)^{x-k}$, with $x = k, k+1, k+2, \dots$:
$$
x = k + \left\lceil \frac{\ln(u)}{\ln(1-p)} - 1 \right\rceil
$$

## Discrete uniform
We want to generate an instance $x$ of $X$, with $x \in \{1, 2, \dots, k\}$ and $P(X = x) = 1/k$.
![[07 - Random-Variable Generators-1794850131505.webp|304]]
We have
$$F(x) = \frac{x}{k}, \quad x = 1, 2, \dots, k$$
![[07 - Random-Variable Generators-1794850151993.webp|346]]

### Inverse formula
The inverse is $x = yk$, so $X$ can be calculated as
$$
x = \lceil ku \rceil
$$

Indeed, after generating $u$ uniform in $(0,1)$, we return $x$ if
$$\begin{aligned} \frac{x-1}{k} < u \le \frac{x}{k} \end{aligned}$$
that is$$\begin{aligned}x - 1 &< ku \le x \\ ku &\le x < ku + 1\end{aligned}$$
### Generation
1. Generate $u = U(0, 1)$
2. Calculate $x = \lceil ku \rceil$
3. Return $x$

## Remarks
- To apply it, we must be able to **derive the inverse of $F(x)$**
- We just need to generate a single random number $U(0,1)$
- The **computational cost depends on the computational complexity of the inverse function**

# Convolution method
---
It is used when a random variable can be expressed as a sum of other r.v. that can be easily generated:
$$
x = y_1 + y_2 + \dots + y_n
$$
- We generate an instance for each $y_i$
- Adding all the $y_i$ we obtain an instance of $x$

## Erlang-K
The Erlang-K with mean $1/m$ is the sum of $K$ r.v. with exponential distribution with mean $1/(Km)$:
$$
X = \sum_{i=1}^{K} X_i
$$

### Generation
An instance $x$ of $X$ is obtained by summing instances $x_i$ of $X_i$:
$$
x = \sum_{i=1}^{K} \left( -\frac{1}{Km} \ln(u_i) \right) = -\frac{1}{Km} \ln\left( \prod_{i=1}^{K} u_i \right)
$$

1. Generate $K$ instances $u_i = U(0,1)$
2. Calculate $x = -\frac{1}{Km} \ln\left( \prod_{i=1}^{K} u_i \right)$
3. Return $x$

If $K$ is large, it might be inefficient.

# Acceptance/Rejection technique
---
The acceptance/rejection technique can be applied to random variables with **continuous pdf $f(x)$ defined over a finite support $[a,b]$**. Let $c$ be the maximum value of $f(x)$.

Generating $x_i = U(a,b)$ and $y_i = U(0,c)$ means generating a random point $P$ in the rectangle delimited by $(a,b)$ on the x-axis and $(0,c)$ on the y-axis:
- if $P$ falls under $f(x)$ → accept (return $x_i$)
- otherwise → reject
![[07 - Random-Variable Generators-1794850393869.webp|362]]

The acceptance probability in an interval $(x_1, x_1 + dx)$ of the x-axis is proportional to $f(x_1)$.
![[07 - Random-Variable Generators-1794850414260.webp|371]]
## Generation
1. Generate $x_i = U(a,b)$, uniform in $[a,b]$
2. Generate $y_i = U(0,c)$, uniform in $[0,c]$
3. If $y_i \le f(x_i)$ return $x_i$, otherwise go back to step 1

## Efficiency
The efficiency of the technique depends on the area of the rectangle delimiting $f(x)$, i.e., on how well the rectangle approximates $f(x)$:
- total area: $c(b-a)$
- area under $f(x)$: $1$

The acceptance probability $P$ for a randomly generated point is the ratio between the area under $f(x)$ and the total area:
$$
P = \frac{1}{c(b-a)}
$$
![[07 - Random-Variable Generators-1794850513662.webp|517]]
![[07 - Random-Variable Generators-1794850491332.webp|519]]

The average number of points generated to obtain an instance of $X$ is
$$
N = \sum_{i=1}^{\infty} i\,(1-P)^{i-1} P = \frac{1}{P}
$$

The technique can be applied to variables with infinite support only by approximation, truncating the support.

## General form
In general, if there exists a pdf $g(x)$ such that $k\,g(x) \ge f(x)$, we can obtain instances of $X$ (with pdf $f(x)$) with the procedure:
1. Generate $x_i$ from distribution $g(x)$
2. Generate $y_i = U(0, k\,g(x_i))$, uniform in $[0, k\,g(x_i)]$
3. If $y_i \le f(x_i)$ return $x_i$, otherwise go back to step 1

In this general form, the acceptance probability is $P = 1/k$.

# Composition method
---
This method is applied to random variables whose CDF can be expressed as a weighted sum of other CDFs:
$$
F(x) = \sum_{i=1}^{n} p_i F_i(x), \quad \text{with } p_i > 0, \; \sum_{i=1}^{n} p_i = 1
$$

## Generation
1. Generate an instance of the r.v. $I$, such that $P(I = i) = p_i$
2. Generate an instance of the r.v. with CDF $F_i(x)$

The method can also be applied to the pdf $f(x)$.

## Laplace
We want to generate $X$ with pdf
$$
f(x) = \frac{1}{2a} e^{-|x|/a}
$$
![[07 - Random-Variable Generators-1794850608388.webp|478]]

The Laplace distribution is the composition of two exponential r.v.: with probability $1/2$ it is positive and with the same probability it is negative.

### Generation
1. Generate $u_1 = U(0,1)$ and $u_2 = U(0,1)$
2. If $u_1 < 1/2$ return $x = a \ln(u_2)$
3. Otherwise return $x = -a \ln(u_2)$


**N.B.:** This distribution can be generated more efficiently with the inverse-transform technique.

# Other methods
---
There are a few other methods applied to some special distributions. These *ad hoc* methods are usually based on mathematical properties that are satisfied by some random variables.

## Normal
We want to generate $X$ with normal (Gaussian) distribution, mean $0$ and variance $1$, $N(0,1)$:
$$
F(x) = \int_{-\infty}^{x} \frac{1}{\sqrt{2\pi}} e^{-t^2/2}\,dt
$$
We cannot apply the inverse-transform technique because the inverse cannot be written in closed form.

### Polar coordinates
We consider 2 independent normal r.v. as the Cartesian coordinates of a point in a plane.
![[07 - Random-Variable Generators-1794850688688.webp|437]]

Expressing the point in polar coordinates:
$$
Z_1 = B \cos\theta \quad Z_2 = B \sin\theta \quad B^2 = Z_1^2 + Z_2^2
$$
We generate $B$ and $\theta$ separately, then we calculate $Z_1$ and $Z_2$:
- $B^2$ is distributed according to a chi-square with 2 degrees of freedom, i.e. it is an **exponential with mean 2**. It is generated with the inverse-transform:
	1. Generate $u = U(0,1)$
	2. Calculate $B = \sqrt{-2\ln(u)}$
	3. Return $B$
- $\theta$ is **uniformly distributed in $(0, 2\pi)$** and independent from $B$. It is also generated with the inverse-transform:
	1. Generate $u = U(0,1)$
	2. Calculate $\theta = 2\pi u$
	3. Return $\theta$

### Generation
1. Generate $u = U(0,1)$
2. Generate $v = U(0,1)$
3. Calculate $B = \sqrt{-2\ln(u)}$
4. Calculate $\theta = 2\pi v$
5. Calculate $Z_1 = B \cos(\theta)$
6. Calculate $Z_2 = B \sin(\theta)$

The procedure generates pairs of instances of a normal r.v. at the cost of generating two instances of a uniform r.v. and some algebraic operations.

Given an instance $Z$ of a normal r.v. $N(0,1)$, instances of a r.v. $X$ normal with mean $\mu$ and variance $\sigma^2$ are obtained from
$$
X = \mu + \sigma Z
$$

### Approximation with the Central Limit Theorem
The previous method might not be efficient (logarithms and trigonometric functions are complex). If an approximation is sufficient, we can use the Central Limit Theorem:
$$
N(\mu, \sigma) \approx \mu + \sigma \frac{\left(\sum_{i=1}^{n} u_i\right) - n/2}{(n/12)^{1/2}}
$$
Generally, $n = 12$ is used.

## Poisson
We want to generate instances of a r.v. $X$ with Poisson distribution and mean $a$:
$$
p(n) = P(X = n) = \frac{e^{-a} a^n}{n!}, \quad n = 0, 1, 2, \dots
$$
$X$ represents the number of arrivals of a Poisson process in the time unit, when $a$ is the average number of arrivals in the time unit.

$X = n$ if and only if
$$
A_1 + A_2 + \dots + A_n \le 1 < A_1 + A_2 + \dots + A_n + A_{n+1}
$$
where $A_i$ is the $i$-th interarrival time.
![[07 - Random-Variable Generators-1794850791197.webp|568]]

Interarrival times $A_i$ are distributed according to an exponential distribution with mean $1/a$. Hence, we can generate an instance of $X$ by generating interarrival times and counting how many arrivals we have in the time unit.

### Derivation
$$\begin{aligned} A_1 + \dots + A_n &\le 1 < A_1 + \dots + A_n + A_{n+1} \\ \sum_{i=1}^{n} -\frac{1}{a}\ln(u_i) &\le 1 < \sum_{i=1}^{n+1} -\frac{1}{a}\ln(u_i) \\ \sum_{i=1}^{n} \ln(u_i) &\ge -a > \sum_{i=1}^{n+1} \ln(u_i) \\ \ln\left(\prod_{i=1}^{n} u_i\right) &\ge -a > \ln\left(\prod_{i=1}^{n+1} u_i\right) \\ \prod_{i=1}^{n} u_i &\ge e^{-a} > \prod_{i=1}^{n+1} u_i \end{aligned}$$

### Generation
1. Initialization: $n = 0$, $q = 1$
2. Generate $u_{n+1} = U(0,1)$
3. $q = q \cdot u_{n+1}$
4. If $q < e^{-a}$ return $n$, otherwise $n = n+1$ and go back to step 2

### Efficiency
The average number of instances of a uniform r.v. needed to generate one instance of $X$ is
$$\begin{aligned} N &= \sum_{n=0}^{\infty} (n+1)\,p(n) = \sum_{n=0}^{\infty} (n+1) \frac{e^{-a} a^n}{n!} = \\ &= \sum_{n=0}^{\infty} n \frac{e^{-a} a^n}{n!} + \sum_{n=0}^{\infty} \frac{e^{-a} a^n}{n!} = \sum_{n=1}^{\infty} \frac{e^{-a} a^n}{(n-1)!} + 1 = \\ &= a \sum_{n=0}^{\infty} \frac{e^{-a} a^n}{n!} + 1 = a + 1 \end{aligned}$$

## Binomial
We want to generate instances of a r.v. $X$ with binomial $(p, n)$ distribution:
$$
f(x) = \binom{n}{x} p^x (1-p)^{n-x}, \quad x = 0, 1, 2, \dots, n
$$
$X$ represents the number of successes among $n$ independent Bernoulli experiments with success probability $p$.

### Generation method 1 (convolution)
1. Generate $n$ instances $u_i = U(0,1)$
2. Count the number $x$ of variables $u_i$ that are smaller than $p$
3. Return $x$

### Generation method 2 (inverse-transform)
1. Calculate $F(x)$ and store its values in vector $A$
2. Generate $u = U(0,1)$
3. Look up $x$ such that $A[x] \le u < A[x+1]$
4. Return $x$

### Generation method 3 (geometric)
It is based on the observation that a geometric with parameter $(1-p)$ assuming the value $i+1$ corresponds to $i$ failed Bernoulli experiments.

E.g. if $g_1 + g_2 < n$ and $g_1 + g_2 + g_3 > n$ → return $2$
![[07 - Random-Variable Generators-1794850831506.webp|509]]

1. Initialization: $m = 1$, $q = 0$
2. Generate $u_m = U(0,1)$
3. Generate the geometric $g_m = \left\lceil \dfrac{\ln(u_m)}{\ln(1-p)} \right\rceil$
4. Calculate $q = q + g_m$
5. If $q > n$ return $m - 1$, otherwise $m = m+1$ and go back to step 2

# Wrap-up
---
- Good generation of instances of random variables is fundamental for simulation; it must be:
	- efficient
	- accurate
- A few general methods:
	- inverse-transform
	- acceptance-rejection
- Several specific methods
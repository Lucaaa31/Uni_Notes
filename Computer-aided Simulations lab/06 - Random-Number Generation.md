Simulators need generators of random variables with specific distributions, so that we can generate:
- **Synthetic traces** for the input processes of the simulator
- **Aggregate behavior for part of the system** that are not simulated in details

## Rationale
---
Generators should:
- Generate sequences of numbers whose **statistical properties approximate with enough accuracy** the theoretical ones
- Make the **experiments repeatable**
- Generate a very large number of different and “uncorrelated” instances
- Be efficient
- Be portable

The actual procedure is:
1. We generate a sequence $X$ of integer “random” numbers
2. Using $X$, we generate a sequence $Z$ of instances of a random variable uniform in $(0,1)$
3. Using $Z$, we generate instances of the chosen random variable

### Pseudo-random numbers
The generation algorithm is **deterministic**, so that if we know the algorithm parameters we can generate the same sequence and **repeat the experiment**.


## Linear congruential generators
---
Based on **module** operator, they are the most commonly used.
They generate a sequence of integer numbers $X={X_i}$ in the range $[0,m-1]$ through the iterative relation
$$
X_{i+1} = (aX_i + c) \text{ mod } m
$$
- $X_0$ is the seed
- $a$ the multiplier
- $c$ the increment
	- $c = 0$ in multiplicative congruential generators
	- $c \ne 0$ in mixed congruential generators
- $m$ the module

### Sequence in $[0, 1]$
Given a generator of sequences $X$ of integer numbers in $[0,m-1]$, a sequence $Z$ of values in $[0,1)$ or in $[0,1]$ is generated from
$$
Z_i = \frac{X_i}{m} \text{ or } Z_i = \frac{X_i}{m-1}
$$
Depending on the normalization used and the value of $c$ chosen, the extremes, $0$ and $1$, can either be included or not.

### Sequence are not random
The algorithm is deterministic; however, the generated sequences look random.

The sequence cannot assume all the values in $(0,1)$: values different from $\frac{1}{m}$ are not possible, but for $m$ large enough the sequence is very dense.

## Sequence Period
---
The length of the generated sequence is called **period.**
A generator with module $m$ has **maximum period** equal to
$$
\begin{cases}
m & \text{if } c \ne 0 \\
m-1 & \text{if } c = 0
\end{cases}
$$
Not all the generators can achieve a maximum period:
![[06 - Random-Number Generation-1794763858974.webp|435]]

### Computational issues
Linear congruential generators may suffer from the following issues:
- The used programming language must perform the operations with integer math
- The product $aX_i$ may overflow, since its representation might exceed the internal used integer representation of $a$ and $X_i$ alone


## Other congruential generators
---
A few techniques permit to increase the period of the sequence.
General congruential generator are in the form
$$
X_i = g(X_{i-1}, X_{i-2}, ...) \text{ mod } m
$$
where $g()$ is a generic function.

### Good generators
Python uses the **Mersenne Twister** as the core generator.
It produces sequences with a period of $2^{19937-1}$.
It is one of the most extensively tested random number generators in existence.

## Tests of RNGs
---
Each number in the sequence $Z_i$ should be an instance of a random variable with uniform distribution in $(0,1)$.

The sequence should have two properties:
- uniformity
- indipendence

### Uniformity test
Identify subintervals of $(0,1)$, check that the statistical occurrence of instances in that interval is coherent with the probability that a uniform instance falls in the subinterval.
![[06 - Random-Number Generation-1794764393639.webp|700]]
![[06 - Random-Number Generation-1794764433642.webp]]

### Serial test (independence)
Apply the chi-square test to a sequence of numbers.
Divide the interval $(0,1)$ in $n$ sub-intervals $I_i$ and consider the $t$-dimensional spaces $T_j$ obtained by combining the intervals $I_i$:
$$
T_j = \left(I_{k_1}, I_{k_2}, \ldots, I_{k_t}\right)
$$
for each combination of $k_1, k_2, \ldots, k_t$ with $k_i = 1, 2, \ldots, n$.

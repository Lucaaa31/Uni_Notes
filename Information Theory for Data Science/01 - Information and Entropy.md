# Information Content

How much information do we gain when we observe an event?

## Definitions

### Model
For a random variable $X$ with $M$ different outcomes, the alphabet is:
$$ \Omega_X = \{x_1, x_2, ..., x_M\} $$

The probability mass function is:
$$ p_i = \mathbb{P}(X = x_i) \qquad 0 \le p_i \le 1, \qquad \sum_{i=1}^M p_i = 1 $$

### Event
An event is a subset of possible outcomes:
$$ A \subseteq \Omega_X $$

We want to quantify how much information we gain when we observe that event $A$ occurred.

> **Key Idea:** Rare events carry more information.
> $$ \text{Low probability} \Rightarrow \text{High surprise} \Rightarrow \text{High information content} $$

### Desired Properties of Information Content
Let $h(A)$ denote the information content of event $A$.
1. $h(A)$ should decrease when $\mathbb{P}(A)$ increases.
2. If $\mathbb{P}(A)=1$, then $h(A)=0$.
3. If $A$ and $B$ are statistically independent, $h(A \cap B) = h(A) + h(B)$.

---

## Shannon Information Content

It is measured in **bits**.
$$ h(A) = \log_2 \frac{1}{\mathbb{P}(A)} = -\log_2 \mathbb{P}(A) $$

![[Entropy-1790773377317.webp]]

As we can see, smaller probability $\Rightarrow$ larger surprise.

### Example with 1 bit
Consider a question with two possible answers:
$$ \mathbb{P}(\text{YES}) = \frac{1}{2} \qquad \mathbb{P}(\text{NO}) = \frac{1}{2} $$

Both have information content:
$$ h = -\log_2 \frac{1}{2} = 1 \text{ bit} $$
*Idea: A binary answer carries exactly 1 bit when the two answers are equiprobable.*

### Can a binary answer carry more than 1 bit?
A submarine is hidden uniformly in one of 64 cells.

Probability that the shot is a hit: 
$$ \mathbb{P}(\text{hit}) = \frac{1}{64} $$
$$ h(\text{hit}) = \log_2 \frac{1}{\mathbb{P}(\text{hit})} = \log_2 64 = 6 \text{ bits} $$

Probability that the shot is a miss: 
$$ \mathbb{P}(\text{miss}) = \frac{63}{64} $$
$$ h(\text{miss}) = \log_2 \frac{1}{\mathbb{P}(\text{miss})} = \log_2 \frac{64}{63} \approx 0.0227 \text{ bits} $$

---

## Properties of Information Content

### Events with no information
If an event $A$ is certain, so $\mathbb{P}(A)=1$, from the definition of information content:
$$ h(A) = -\log_2 \mathbb{P}(A) $$
we obtain:
$$ h(A) = -\log_2 1 = 0 $$
A certain event carries no information.

### Independent events
If $A$ and $B$ are statistically independent ($\mathbb{P}(A \cap B) = \mathbb{P}(A) \mathbb{P}(B)$):
$$
\begin{aligned}
h(A \cap B) &= \log_2 \frac{1}{\mathbb{P}(A \cap B)} \\
&= \log_2 \frac{1}{\mathbb{P}(A)} + \log_2 \frac{1}{\mathbb{P}(B)} \\
&= h(A) + h(B)
\end{aligned}
$$

---

# Entropy

For outcome $x_i$:
$$ h_i = \log_2 \frac{1}{p_i} $$
Different outcomes may have different probabilities and therefore different information contents.

## Definition of Entropy
Entropy is the expected information content of the outcomes of $X$:
$$
\begin{aligned}
H(X) &= \sum_{i=1}^M p_i h_i = \sum_{i=1}^M p_i \log_2 \frac{1}{p_i} \\
&= - \sum_{i=1}^M p_i \log_2 p_i
\end{aligned}
$$

### Edge Cases
- **What if $p_i=0$?**
  We use the continuous extension:
  $$ \lim_{p \rightarrow 0^+} p \log_2 \frac{1}{p} = 0 $$
  So that a never-occurring outcome contributes zero entropy.
  
- **What if $p_i=1$?**
  A certain outcome carries no information:
  $$ p_i = 1 \Rightarrow p_i \log_2 \frac{1}{p_i} = 0 $$

### When is Entropy zero?
We have $H(X)=0$ if and only if one outcome has probability 1 and all others have probability 0.
$$ \text{No uncertainty} \Longleftrightarrow \text{No information is gained by observing } X $$

## Entropy is Non-Negative

For every $i$ such that $p_i > 0$:
$$ 0 < p_i \le 1 \Longrightarrow \frac{1}{p_i} \ge 1 $$

Therefore:
$$ \log_2 \frac{1}{p_i} \ge 0 $$

Hence:
$$ H(X) = \sum_{i=1}^M p_i \log_2 \frac{1}{p_i} \ge 0 $$

---

# Binary and Ternary Entropy

## Binary Random Variable
$$ \Omega_X = \{x_1, x_2\}, \qquad \mathbb{P}(X=x_1) + \mathbb{P}(X = x_2) = 1 $$

Let $\mathbb{P}(X = x_1) = p$ and $\mathbb{P}(X=x_2)=1-p$, then:
$$ H_2(p) = p \log_2 \frac{1}{p} + (1-p) \log_2 \frac{1}{1-p} $$

**Binary Entropy Function:**
![[Entropy-1790958533326.webp]]

**Information Content vs. Entropy:**
![[Entropy-1790959104804.webp]]

Maximum uncertainty for a binary variable: 
$$ p = \frac{1}{2} \Longrightarrow H_2(p) = 1 \text{ bit} $$
That means the two outcomes are equally likely.

## Ternary Random Variable
$$ \Omega_X = \{x_1, x_2, x_3\}, \qquad p_1 + p_2 + p_3 = 1 $$

---

# Maximum Entropy

It is the probability distribution that maximizes entropy among all those consistent with a given set of constraints or prior information. 

## Maximum Possible Entropy
For a random variable with $M$ possible outcomes:
$$ 0 \le H(X) \le \log_2 M $$

We can give it two different interpretations:
1. Maximum entropy corresponds to maximum uncertainty.
2. More possible outcomes can create more uncertainty.

---

# Constrained Optimization

**Lagrange Optimization:** It is an alternative proof of the maximum-entropy theorem. **The same tool lets us maximize entropy under additional constraints.**

## Unconstrained Optimization
For a differentiable function $f(x_1, ..., x_M)$, a candidate maximum/minimum satisfies:
$$ \frac{\partial f}{\partial x_1} = \dots = \frac{\partial f}{\partial x_M} = 0 $$

## Constrained Optimization
Given $f(x_1, ..., x_M)$, we now impose a **constraint**:
$$ g(x_1, ..., x_M) = 0 $$

We want to maximize/minimize $f$ only under the constraint $g$.

### Example of Constrained Optimization
In the Maximum Entropy, we want to maximize:
$$ H(X) = - \sum_{i=1}^M p_i \log_2 p_i $$
subject to the constraint:
$$ \sum_{i=1}^M p_i - 1 = 0 $$

---

## Lagrange Multiplier

Given the constrained optimization problem to optimize $f(x_1, ..., x_M)$ subject to $g(x_1, ..., x_M)=0$.
We introduce a new variable $\lambda$ and define:
$$ \Delta (x_1, ..., x_M, \lambda) = f(x_1, ..., x_M) + \lambda g(x_1, ..., x_M) $$

A constrained stationary point satisfies:
$$
\begin{cases} 
\frac{\partial \Delta}{\partial x_1} = 0 \\ 
\frac{\partial \Delta}{\partial x_2} = 0 \\ 
\quad \vdots \\ 
\frac{\partial \Delta}{\partial x_M} = 0 \\ 
\frac{\partial \Delta}{\partial \lambda} = 0 
\end{cases}
$$

The last equation gives exactly the original constraint:
$$ \frac{\partial \Delta}{\partial \lambda} = 0 \Longleftrightarrow g(x_1, ..., x_M) = 0 $$

### Lagrangian for Entropy
$$ \Delta(p, \lambda) = - \sum_{i=1}^M p_i \log_2 p_i + \lambda \left(\sum_{i=1}^M p_i - 1\right) $$

#### Stationarity
For every $i$:
$$ \frac{\partial \Delta}{\partial p_i} = - \log_2 p_i - \log_2 e + \lambda = 0 $$

Therefore:
$$ \log_2 p_i = \lambda - \log_2 e $$
which is the same for every $i$.

All probabilities must be equal:
$$ p_i = c \quad \forall i $$

---

# Uniform Distribution

From the last partial derivative:
$$ \frac{\partial\Delta}{\partial\lambda} = \sum_{i=1}^M p_i - 1 = 0 \implies \sum_i p_i = 1 $$

Since all probabilities must be equal ($p_i = c \quad \forall i$):
$$ Mc = 1 \implies p_i = \frac{1}{M} $$

### Why is the stationary point the maximum?
Inside the probability simplex ($p_i > 0$):
$$ \frac{\partial^2 H}{\partial p_i^2} = -\frac{1}{p_i \ln 2} < 0 $$
$$ \frac{\partial^2 H}{\partial p_i \partial p_j} = 0 \quad (i \ne j) $$

The Hessian matrix is negative definite, meaning entropy is strictly concave on the interior of the simplex. Therefore, under the constraint $\sum_{i=1}^M p_i = 1$, an interior stationary point is the unique global maximum.

### Maximum Entropy Theorem
$$ H(X) \le \log_2 M $$
with equality if and only if $p_i = \frac{1}{M} \forall i$.

---

# Principle of Maximum Entropy

> **Meaning:** Infer probability distribution from the mean.

Suppose $X$ can take $M$ known numerical values $\Omega_X = \{x_1, ..., x_M\}$, but the pmf $\{p_i\}$ is unknown. If the mean $\mu = \mathbb{E}[X]$ is known (e.g., from experimental observations), we can infer the unknown pmf using only this information.

**Key Idea:** Among all probability distributions compatible with the known mean value, choose the one with maximum entropy. Since entropy measures average information, the MaxEnt distribution is the pmf satisfying the mean constraint with the largest average information. 
*Important:* It is a modeling principle. It selects one admissible distribution but does not guarantee that this is the true data-generating distribution.

### The constraints alone do not determine a unique distribution
Consider $\Omega_X = \{0, 1, 2\}$ with the constrained mean $\mathbb{E}[X] = 1$. 
Many different probability distributions satisfy both constraints ($\sum_i p_i = 1$ and $\sum_i p_i x_i = 1$):
*   $(0, 1, 0) \implies H = 0 \text{ bit}$
*   $(\frac{1}{2}, 0, \frac{1}{2}) \implies H = 1 \text{ bit}$
*   $(\frac{1}{3}, \frac{1}{3}, \frac{1}{3}) \implies H = \log_2 3 \text{ bit}$

All of them have a mean of 1. MaxEnt selects the uniform distribution (the one with the largest entropy).

---

## Constrained Optimization with a Known Mean

We want to maximize:
$$ H(X) = - \sum_i p_i \log_2 p_i $$
Subject to:
1. $\sum_i p_i = 1$
2. $\sum_i p_i x_i = \mu$

*Feasibility requires $\min_i x_i \le \mu \le \max_i x_i$.*

### Two-Constraint Lagrangian
$$ \Delta = - \sum_i p_i \log_2 p_i + \lambda_1 \left(\sum_i p_i - 1\right) + \lambda_2 \left(\sum_i p_i x_i - \mu\right) $$

Partial derivatives with respect to $p_i$:
$$ \frac{\partial\Delta}{\partial p_i} = - \log_2 p_i - \log_2 e + \lambda_1 + \lambda_2 x_i = 0 $$

Therefore:
$$ \log_2 p_i = c + \lambda_2 x_i $$
$$ p_i = 2^{c + \lambda_2 x_i} = 2^c 2^{\lambda_2 x_i} = \alpha \beta^{x_i} $$

### Exponential Form of the pmf
Define constants $\alpha > 0$ and $\beta > 0$. Then $p_i = \alpha \beta^{x_i}$.
Normalization $\sum_i p_i = 1$ gives $\alpha = \frac{1}{\sum_j \beta^{x_j}}$.

Therefore:
$$ p_i = \frac{\beta^{x_i}}{\sum_j \beta^{x_j}} $$
This is an exponential-family model.

### Determine $\beta$ from the mean
The constraint $\sum_i p_i x_i = \mu$ gives:
$$ \frac{\sum_i x_i \beta^{x_i}}{\sum_i \beta^{x_i}} = \mu $$

Equivalently, solve this equation for $\beta$:
$$ \sum_i (x_i - \mu) \beta^{x_i} = 0 $$

> **Maximum entropy and exponential families**
> Suppose we know several averages $\mathbb{E}[f_k(X)] = F_k$ for $k=1,...,K$. MaxEnt gives a distribution of the form $p(x) = \frac{1}{Z} \exp\left(\sum_{k=1}^K w_k f_k(x)\right)$. Exponential-family models appear throughout statistics and machine learning; maximum entropy provides one route to them from constraints.

### Why does MaxEnt give a global maximum?
We maximize $H(p) = -\sum_{i=1}^M p_i \log_2 p_i$ subject to the linear constraints $\sum_i p_i = 1$ and $\sum_i p_i x_i = \mu$. 
Key properties:
*   Entropy $H(p)$ is strictly concave.
*   The constraints are linear in the probabilities $p_i$.

Therefore, a stationary point satisfying the constraints is the unique global maximum.

---

# MaxEnt Examples

## Warm-up: Two outcomes 0 and 1
Let $\Omega_X = \{0, 1\}$.
$$ p_0 = \frac{\beta^0}{\beta^0 + \beta^1} = \frac{1}{1+\beta}, \quad p_1 = \frac{\beta^1}{\beta^0 + \beta^1} = \frac{\beta}{1+\beta} $$

The mean constraint $(0-\mu)\beta^0 + (1-\mu)\beta^1 = 0$ yields $\beta = \frac{\mu}{1-\mu}$.
*Why the binary case is special:* Since $\mu = p_1$ and $p_0 + p_1 = 1$, we automatically have $p_0 = 1-\mu$ and $p_1 = \mu$. The normalization and mean constraints already determine the pmf uniquely, leaving no family of admissible pmfs to choose from. If $\mu=1/2$, the unique pmf is uniform.

## A nontrivial MaxEnt example
Let $\Omega_X = \{0, 1, 2\}$ and $\mu = \frac{4}{7}$.
The MaxEnt distribution has the form $p_i = \frac{\beta^{x_i}}{1 + \beta + \beta^2}$.
The parameter $\beta$ is determined by the mean constraint:
$$ \sum_i (x_i - \mu)\beta^{x_i} = 0 \implies -\frac{4}{7} + \frac{3}{7}\beta + \frac{10}{7}\beta^2 = 0 \implies 10\beta^2 + 3\beta - 4 = 0 $$

Since $\beta > 0$, we choose the solution $\beta = \frac{1}{2}$.
The normalization factor is $1 + \beta + \beta^2 = 1 + \frac{1}{2} + \frac{1}{4} = \frac{7}{4}$.
Hence:
$$ p_0 = \frac{4}{7} \qquad p_1 = \frac{2}{7} \qquad p_2 = \frac{1}{7} $$
*Interpretation:* Since $\mu <$ arithmetic average, MaxEnt assigns larger probabilities to smaller outcomes.

---

# General Behavior of the MaxEnt pmf

Assume the possible values are ordered as $x_1 < x_2 < ... < x_M$.
Let $\overline{x} = \frac{1}{M}\sum_{i=1}^M x_i$ be the arithmetic average of the possible values.
The MaxEnt probabilities vary exponentially with $x_i$:
*   If $\mu = \overline{x}$, the MaxEnt pmf is **uniform**. (e.g., $\mu=1 \implies \beta=1$).
*   If $\mu < \overline{x}$, smaller values of $x_i$ have **larger** probabilities. (e.g., $\mu=\frac{4}{7} \implies \beta=0.5$).
*   If $\mu > \overline{x}$, larger values of $x_i$ have **larger** probabilities. (e.g., $\mu=\frac{10}{7} \implies \beta=2$).

For equally spaced values $x_i$, the probabilities form a geometric progression.

---

# Lecture Takeaways
1.  **Information content:** $h(A) = -\log_2 \mathbb{P}(A)$ and properties of $h$.
2.  **Entropy:** $H(X) = \mathbb{E}[h(X)]$ and properties of $H$.
3.  **Bounds:** $0 \le H(X) \le \log_2 M$.
4.  **Maximum:** The maximum is attained by the uniform pmf and increases with cardinality $M$.
5.  **Constraints:** The MaxEnt principle selects the admissible pmf with the largest entropy. Mean constraints lead naturally to exponential-family distributions.
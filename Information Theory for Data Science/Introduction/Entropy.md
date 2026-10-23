# Information Content
How much information do we gain when we observe an event?
## Some definitions
### Model
Random variable $X$ with $M$ different outcomes.
Alphabet $\Omega_X = \{x_1, x_2, ..., x_M\}$.

The probability function is:
$$
p_i = P(X = x_i) \qquad 0 \le p_i \le 1, \qquad \sum_{i=1}^M p_i = 1
$$
### Event
An event is a subset of possible outcomes:
$$
A \subset \Omega_X
$$
We want to quantify how much information we gain when we observe that event $A$ occurred.

**Key Idea:** Rare events carry more information
$$
\text{Low probability} \Rightarrow \text{High surprise} \Rightarrow \text{High information content}
$$

## Desired properties of information content
Let $h(A)$ denote the information content of event $A$.
1. $h(A)$ should decrease when $P(A)$ increases
2. If $P(A)=1$, then $h(A)=0$
3. If $A$ and $B$ are statistically independent $$h(A \cap B) = h(A) + h(B)$$
### Shannon information content
It is measured in **bits**.
$$
h(A) = \log_2 \frac{1}{P(A)} = -\log_2P(A)
$$
![[Entropy-1790773377317.webp]]
As we can see, smaller probability $\Rightarrow$ larger surprise.

### Example with 1 bit
Consider a question with two possible answers:
$$P(YES) = \frac{1}{2} \qquad P(NO) = \frac{1}{2}$$

Both have information content:
$$
h = -\log_2 \frac{1}{2} = 1 \text{ bit}
$$
**Idea:** A binary answer carries exactly $1$ bit when the two answers are equiprobable.

#### Can a binary answer carry more than 1 bit?
A submarine is hidden uniformly in one of $64$ cells.
![[Entropy-1790777311213.webp|228]]
Probability that the shot is a hit: $\mathbb{P}(\text{hit}) = \frac{1}{64}$
$$h(\text{hit}) = \log_2 \frac{1}{\mathbb{P}(\text{hit})} = \log_2 64 = 6 \text{ bits.}$$
Probability that the shot is a miss $\mathbb{P}(\text{miss}) = 63/64$
$$h(\text{miss}) = \log_2 \frac{1}{\mathbb{P}(\text{miss})} = \log_2 \frac{64}{63} \approx 0.0227 \text{ bits.}$$
### Events with no information
If an event $A$ is certain, so $P(A)=1$, from the definition of information content:
$$
h(A) = -\log_2P(A)
$$
we obtain
$$
h(A) = -\log_2 1 = 0
$$
### Independent events
If $A$ and $B$ are statistically independent:
$$
P(A \cap B) = P(A) P(B)
$$
Therefore,
$$
\begin{aligned}
h(A \cap B) &= \log_2 \frac{1}{\mathbb{P}(A \cap B)} \\
&= \log_2 \frac{1}{\mathbb{P}(A)} + \log_2 \frac{1}{\mathbb{P}(B)} \\
&= h(A) + h(B).
\end{aligned}
$$
--- 
# Entropy
For outcome $x_i$
$$
h_i = \log_2 \frac{1}{p_i}
$$
Different outcomes may have different probabilities and therefore different information contents.

## Definition of Entropy
Entropy is the **expected information content** of the outcomes of $X$:
$$
\begin{aligned}
H(X) &= \sum_{i=1}^M p_i h_i = \sum_{i=1}^M p_i \log_2 \frac{1}{p_i} \\
&= - \sum_{i=1}^M p_i \log_2 p_i
\end{aligned}
$$
### What if $p_i=0$?
We use the continuous extension
$$
\lim_{p \rightarrow 0^+} p \log_2 \frac{1}{p} = 0
$$
so that, **a never-occuring outcome contributes zero entropy.**
### What if $p_i=1$?
A certain outcome carries no information:
$$
p_i = 1 \Rightarrow p_i \log_2 \frac{1}{p_i} = 0
$$
### When is Entropy zero?
We have $H(X)=0$ if and only if one outcome has probability $1$ and all others have probability $0$.
$$
\text{No uncertainity} \Longleftrightarrow \text{no information is gained by observing } X
$$
### Entropy is non-negative
![[Entropy-1790781122764.webp|261]]
For every $i$ s.t. $p_i > 0$
$$
0 < p_i \le 1 \Longrightarrow \frac{1}{p_i} \ge 1.
$$
Therefore,
$$
\log_2 \frac{1}{p_i} \ge 0
$$
Hence
$$
H(X) = \sum_{i=0}^M p_i \log_2 \frac{1}{p_i} \ge 0
$$
---
# Binary and Ternary Entropy

## Binary Random Variable
$$
\Omega_X = \{x_1, x_2\}, \qquad P(X=x_1) + P(X = x_2) = 1
$$
Let $$P(X = x_1) = p \text{ and } P(X=x_2)=1-p$$ then
$$
H_2(p) = p \log_2 \frac{1}{p} + (1-p) \log_2 \frac{1}{1-p}
$$
### Binary Entropy Function
![[Entropy-1790958533326.webp]]
#### Information Content vs. Entropy
![[Entropy-1790959104804.webp]]**Maximum uncertainty for a binary variable:** $$p = \frac{1}{2} \Longrightarrow H_2(p) = 1 \text{ bit}$$
That means that every outcomes are equally likely.

### Ternary Random Variable
$$
\Omega_X = \{x_1, x_2, x_3\}, \qquad p_1 + p_2 + p_3 = 1
$$
![[Entropy-1790960193487.webp|347]]

## Maximum Entropy
It is the probability distribution that maximizes entropy among all those consistent with a given set of constraints or prior information.

### Maximum Possible Entropy
For a random variable with $M$ possible outcomes
$$
0 \le H(X) \le \log_2 M
$$We can give it two different interpretations:
1. Maximum entropy corresponds to maximum uncertainty
2. More possible outcomes can create more uncertainty

---
# Constrained Optimization
**Lagrange Optimization:** It is an alternative proof of the maximum-entropy theorem. **The same tool lets us maximize entropy under additional constraint.**

### Unconstrained Optimization
For a differentiable function
$$
f(x_1, ..., x_M)
$$
a candidate maximum/minimum satisfies
$$
\frac{\partial f}{\partial x_1} = ... = \frac{\partial f}{\partial x_M} = 0
$$
### Constrained Optimization
Given
$$
f(x_1, ..., x_M)
$$
we now impose a **constraint**
$$
g(x_1, ..., x_M) = 0
$$
We want to maximize/minimize $f$ only under the constraint $g$.

#### Example of Constrained Optimization
In the Maximum Entropy, we want to maximize
$$
H(X) = - \sum_{i=1}^M p_i \log_2 p_i
$$
subject to constraint
$$
\sum_{i=1}^M p_i - 1 = 0
$$
## Lagrange Multiplier
Given the same problem to optimize $f(x_1, ..., x_M)$ subject to $g(x_1, ..., x_M)=0$.
We introduce a new variable $\lambda$ and define:
$$
\Delta (x_1, ..., x_M, \lambda) = f(x_1, ..., x_M) + \lambda g(x_1, ..., x_M)
$$
A constrained stationary point is obtained by satisfying
$$\begin{cases} \frac{\partial \Delta}{\partial x_1} = 0 \\ \frac{\partial \Delta}{\partial x_2} = 0 \\ \quad \vdots \\ \frac{\partial \Delta}{\partial x_M} = 0 \\ \frac{\partial \Delta}{\partial \lambda} = 0 \end{cases}$$
Because the last equation gives
$$
\frac{\partial \Delta}{\partial \lambda} = g(x_1, ..., x_M) = 0
$$
that is exactly the original constraint:
$$
\frac{\partial \Delta}{\partial \lambda}=0 ⇐⇒ g(x_1, ..., x_M) = 0
$$
### Lagrangian for Entropy
$$
\Delta(p, \lambda) = - \sum_{i=1}^M p_i \log_2 p_i + \lambda(\sum_{i=1}^M p_i - 1)
$$
#### Stationarity
For every $i$,
$$
\frac{\partial \Delta}{\Delta p_i} = - \log_2 p_i - \log_2 e + \lambda = 0
$$
Therefore
$$
\log_2 p_i = \lambda - \log_2 e,
$$
which is the same for every $i$.
All probabilities must be equal:
$$
p_i = c \quad \forall i
$$
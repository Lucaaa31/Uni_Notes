# 1. Source coding
Model systems are digital, so information in represented by bits that are easy to transmit and process.

## Symbol by symbol encoding
For every symbol $x_i$, we want to assign a binary codeword $c_i$ of length $l_i$:
![[02 - Source coding and entropy of multiple variables-1794245515513.webp|289]]

## Fixed-length code
The simplest choice is to have all the lengths equal:
$$
l_1 = l_2 = ... = l_M = l.
$$
$$
l = ⌈log_2 M ⌉
$$

Pros:
- very simple encoding and decoding
Cons:
- It ignores the fact that symbols can have very different probabilities.


## Variable-length code
It uses shorter codewords for more likely symbols:
$$
p_i \text{ large } \Rightarrow l_i \text{ small}
$$
$$
p_i \text{ small } \Rightarrow l_i \text{ large}
$$
It has the problem of ambiguity:
![[02 - Source coding and entropy of multiple variables-1794245884990.webp]]
In this example the sequence $10$ can be interpreted as $x_3$ or $x_1 x_2$.
### Prefix condition
In order to avoid these ambiguities we introduce the following rule:
> No codeword can be the initial part (prefix) of another codeword.


## Goal: minimize the average number of bits
Our goal is to minimize the average code length $\bar l$:
$$
\bar l = \sum_{i=l}^M p_i l_i
$$


## Source coding algorithms
![[02 - Source coding and entropy of multiple variables-1794246259017.webp]]
### Shannon–Fano algorithm
- Build a binary tree.
- At each node, split the current set of symbols into two subsets.
- Make the two subset probabilities as close as possible.

### Source coding theorem: link with entropy
$$
H(X) \le \bar l_{opt} < H(X)+1
$$
for an optimal binary prefix code.
Entropy determines the average number of bits needed to represent a source symbol.

## 2. Entropy and data processing

### Label invariance
- Random variable $X$
$$\Omega_X = \{x_1, x_2, \dots, x_M\}$$
$$P_X = \{p_1, p_2, \dots, p_M\}$$

- Random variable $Y$
$$\Omega_Y = \{y_1, y_2, \dots, y_M\}$$
$$P_Y = \{p_1, p_2, \dots, p_M\}$$
The alphabets are **different**, but the probability distributions are the **same**:
$$P_X = P_Y.$$
Therefore,
$$H(X) = -\sum_{i=1}^{M} p_i \log_2 p_i = H(Y).$$
$$\boxed{H(X) = H(Y)}$$

> **Entropy depends only on the probabilities, not on the labels of the outcomes.**

### Functions of a random variable
Now $Y = f(X)$:
$$
\Omega_X = \{x_1, x_2, \dots, x_M\}
$$
$$
P_Y
$$
In general, different values of $X$ may be mapped into the same value of $Y$.
Therefore, in general, the size of $\Omega_Y$ may be different from the size of $\Omega_X$ , and
$$
\boxed{H(Y = f(X)) \ne H(X)}
$$

### General Result
> Deterministic processing cannot increase the entropy of a discrete random variable
$$
\boxed{0 \le H(f(X))\le H(X)} 
$$
### Non-Injective mapping
General result:
$$
H(f(X)) < H(X)
$$
### Why collisions reduce entropy
Suppose two outcomes are merged:
$$
f(x_1) = f(x_2), \qquad p_1, p_2 > 0
$$
Before merging, their entropy contribution is
$$
p_1 \log_2 \frac{1}{p_1} + p_2 \log_2 \frac{1}{p_2}
$$
After merging
$$
(p_1 + p_2) \log_2 \frac{1}{p_1 + p_2}
$$

We have a **merge inequality**:
$$
p_1 \log_2 \frac{1}{p_1} + p_2 \log_2 \frac{1}{p_2} \ge (p_1 + p_2) \log_2 \frac{1}{p_1 + p_2}
$$
where each collision can only reduce entropy.
## 3. Joint entropy
Consider $X$ and $Y$ with joint pmf:
$$
p(x, y), \qquad \sum_{x, y} p(x, y) = 1
$$
### Definition 
$$
H(X, Y) = \sum_{x, y} p(x, y) \log_2 \frac{1}{p(x, y)}
$$
### Range
$$
0 \le H(X, Y) \le \log_2 (| \Omega_X ||\Omega_Y |)
$$

### Joint entropy vs. marginal entropies
- General result:
$$
H(X, Y) \le H(X) + H(Y)
$$

- If $X$ and $Y$ are **statistically independent** $$H(X, Y) = H(X) + H(Y)$$ Observing $Y$ provides **no information** about $X$.

- If X and Y are **not statistically independent**$$H(X, Y) < H(X) + H(Y)$$Observing $Y$ provides **some information** about $X$.

## 4. Conditional entropy
**Definition:** It is the uncertainty that remains about $X$ after observing $Y$.
$$
H(X | Y) = \sum_{x, y} p(x, y) \log_2 \frac{1}{p(x | y)}
$$
### Chain rule
$$
H(X, Y) = H( X | Y) + H(Y)
$$
By symmetry
$$
H(X, Y ) = H(Y |X) + H(X)
$$



### Three cases of Conditional Information
> Conditioning decrease entropy. 

Lets's see three cases:
- $X$ and $Y$ are statistically independent$$H(X | Y) = H(X)$$After observing $Y$ we do not have information about $X$.
- General property:$$H(X | Y) \le H(X)$$ It means that observing $Y$ can never increase the uncertainty about $X$.
- Complete information about $X$: $$H(X | Y) = 0$$ $Y$ contains all the information we needed to determinate $X$.


## 5. Mutual information (Information gain)
> **Definition:** how much information about $X$ we gain when we observe $Y$.
$$
\boxed{I (X; Y) = H(X) - H(X | Y)}
$$
- $H(X) =$ original uncertainty about $X$
- $H(X | Y)$ remaining uncertainty about $X$ after we observe $Y$ 

### Equivalent formulas and symmetry
From the chain rule
$$
H(X | Y) = H(X, Y) - H(Y)
$$
we obtain:
$$\begin{aligned}
I(X;Y) &= H(X) - H(X \mid Y) \\
       &= \boxed{H(X) + H(Y) - H(X,Y)}
\end{aligned}$$

Similarly:
$$
\boxed{I(X; Y) = H(Y) - H(Y | X)}
$$

### Symmetry
> The information gained about $X$ by observing $Y$ equals the information gained about $Y$ by observing $X$.
$$
I(X ; Y) = I(Y ; X)
$$
### Information diagram![[02 - Source coding and entropy of multiple variables-1794326065748.webp|381]]

### Application: feature selection (normalized information gain)

## 6. Kullback–Leibler divergence

### Definition: comparing data and model

### Properties (non-negativity, no upper bound, not a distance)

### Use for model selection

### Mutual information as a KL divergence
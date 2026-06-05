**Origin:** The term was coined by R. Bellman in 1957 to describe the difficulty of finding an optimum in high-dimensional spaces via exhaustive search, and to promote dynamic programming approaches.

---
## High Dimensional Spaces are Empty

### The Neighborhood Problem
Supervised classification and regression often rely on **local averages**:
- **Classification:** Given $n$ labeled points, classify a new point $x$ by majority vote in its neighborhood.
![[The Curse of Dimensionality-1780666756023.webp]]
- **Regression:** Observe $n$ i.i.d. pairs $(x^i, y^i)$ from the model:

$$y^i = f(x^i) + \epsilon_i$$

Estimate $f(x)$ as the average of all $y_i$ for the $k$ nearest neighbors of $x$.
![[The Curse of Dimensionality-1780666829906.webp]]
> This works in low dimension. **Not so much when $p$ increases.**

### Why Neighborhoods Break Down

Assume data lives in $[0,1]^p$. To capture a neighborhood covering a fraction $s$ of the hypercube's volume, the required edge length is:
$$\ell = s^{\frac{1}{p}}$$

| $s$  | $p$ | $\ell = s^{\frac{1}{p}}$ |
| ---- | --- | ------------------------ |
| 0.1  | 10  | 0.63                     |
| 0.01 | 10  | 0.80                     |
![[The Curse of Dimensionality-1780666922962.webp]]
**Key insight:** To capture even 1% of the volume in $p=10$ dimensions, you need a neighborhood spanning 80% of each axis.

> [!danger] **Neighborhoods are no longer local.**

### Sparsity Grows Exponentially

The volume of a hypercube with edge $r = 0.1$ is $0.1^p \to 0$ as $p$ grows. The probability of capturing any data point in a small region becomes negligible.

> [!danger] **Points in high dimensional spaces are isolated.**

To overcome this, the required sample size grows **exponentially** with $p$.
![[The Curse of Dimensionality-1780667044336.webp]]

---
## Nearest Neighbors
Let $X, Y$ be two independent variables with uniform distribution on $[0,1]^p$. The squared distance satisfies:

$$\mathbb{E}[\|X - Y\|^2] = p/6 \qquad \text{and} \qquad \text{Std}[\|X - Y\|^2] \simeq 0.2\sqrt{p}$$
![[The Curse of Dimensionality-1780667174803.webp]]
As $p$ grows, **all pairwise distances concentrate** around the same value — the histogram of distances becomes a narrow spike.

|Dimension|Distance distribution|
|---|---|
|$p = 2$|Spread across $[0, 1.2]$|
|$p = 100$|Concentrated near 4|
|$p = 1000$|Very tightly concentrated near 12|

>[!danger] **The notion of nearest neighbors vanishes**
> There is no meaningful distinction between the "closest" and "farthest" point.

---

## Classification in High Dimension

Because high-dimensional spaces are nearly empty:

- It becomes easier to **separate classes** with an adapted classifier.
- The larger $p$, the higher the probability that a **separating hyperplane** exists.

![[The Curse of Dimensionality-1780667315782.webp]]
However, this comes at a cost:

> [!danger] **Overfitting**
> A classifier that perfectly separates training data in high dimension will often generalize poorly to new data.

---

## Concentration Phenomena

### Volume of the Ball

The volume of a $p$-dimensional ball of radius $r$ is:

$$V_p(r) = r^p \cdot \frac{\pi^{p/2}}{\Gamma(p/2 + 1)}$$
![[The Curse of Dimensionality-1780667375805.webp]]
This volume **peaks around $p \approx 5$** and then rapidly collapses to 0 as $p \to \infty$.

**Consequence:** To cover $[0,1]^p$ with $n$ unit balls, you need:

$$n \geq \frac{1}{V_p} = \frac{\Gamma(p/2+1)}{\pi^{p/2}} \overset{p \to \infty}{\sim} \left(\frac{p}{2\pi e}\right)^{p/2} \sqrt{p\pi}$$

For $p = 100$: $n = 42 \times 10^{39}$ balls required. 🤯

---

### Volume of the Shell

The probability that a uniformly distributed point on the unit ball lies in the **thin outer shell** between radii $0.9$ and $1$ is:
$$P(X \in S_{0.9}(p)) = 1 - 0.9^p \xrightarrow{p \to \infty} 1$$
![[The Curse of Dimensionality-1780668700113.webp]]
In high dimensions, **almost all the volume of a ball is concentrated near its surface**.

---

### Samples Near the Boundary

Let $X_1, \ldots, X_n$ be i.i.d. with uniform distribution on the unit ball in $\mathbb{R}^p$. The **median distance** from the origin to the closest data point is:

$$\text{med}(p, n) = \left(1 - \frac{1}{2^{1/n}}\right)^{\frac{1}{p}}$$

For $n = 500$, $p = 10$: $\text{med} = 0.52$

> Most data points are closer to the **edge** of the ball than to its **center**.


---

## Summary

| Phenomenon                       | Effect                                      |
| -------------------------------- | ------------------------------------------- |
| Sparse neighborhoods             | Local methods (kNN, kernel smoothing) fail  |
| Uniform distance concentration   | No meaningful "nearest" neighbor            |
| Volume near the shell            | Data lies on the boundary, not the interior |
| Exponential sample growth needed | Practical datasets are always too small     |
| Easy linear separation           | High risk of overfitting                    |

> **The curse of dimensionality** is not just one problem — it is a family of interrelated geometric phenomena that make statistical learning in high dimensions fundamentally harder.

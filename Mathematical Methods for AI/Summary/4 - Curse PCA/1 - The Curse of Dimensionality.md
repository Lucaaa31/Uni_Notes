

> **Context:** Statistical learning techniques often rely on localized data structures to perform classification and regression. When moving from low-dimensional to high-dimensional settings, the geometric properties of the search space alter drastically, making traditional estimation methods inefficient or mathematically unviable.

> [!definition] Origin of the Term
> 
> The term "Curse of Dimensionality" was coined by R. Bellman in 1957 to describe the difficulty of finding an optimum in high-dimensional spaces via exhaustive search, and to promote dynamic programming approaches.

### The Neighborhood Problem

> **Context:** Non-parametric supervised learning techniques, such as classification and regression, intrinsically depend on the assumption that **local averages** of neighboring points provide an accurate estimation of the underlying target function.

> [!definition] Neighborhood-Based Supervised Learning
> 
> Given $n$ observations, estimation at a target point $x$ depends on localized data:
>
>- **Classification:** Given $n$ labeled points, a new point $x$ is classified by majority vote in its neighborhood.
 >   ![[The Curse of Dimensionality-1780666756023.webp|224x207]]
>- **Regression:** Given $n$ i.i.d. pairs $(x^i, y^i)$ from the model:
>     $$y^i = f(x^i) + \epsilon_i$$
>     
>   ![[The Curse of Dimensionality-1780666829906.webp|379x164]]
>    
>
>
>    The function $f(x)$ is estimated as the average of all $y_i$ for the $k$ nearest neighbors of $x$.


> [!theorem] Breakdown of Local Neighborhoods
> 
> Assume data lives in the hypercube $[0,1]^p$. To capture a neighborhood covering a fraction $s$ of the hypercube's volume, the required edge length $\ell$ is defined by:
> 
> $$\boxed{\ell = s^{\frac{1}{p}}}$$
>
>![[The Curse of Dimensionality-1780666922962.webp|385x255]]
> **Consequence:** In higher dimensions, neighborhoods lose their local property. For instance, to capture a volume fraction of $s = 0.01$ in a $p = 10$ dimensional space, the edge length must be $\ell = 0.01^{\frac{1}{10}} \simeq 0.80$, meaning the neighborhood must span $80\%$ of each axis.

### Exponential Sparsity and Distance Concentration

> **Context:** As the number of dimensions increases, the available sample space expands exponentially. This geometric expansion causes data points to become isolated, altering the behavior of pairwise Euclidean distances and invalidating the concept of proximity.

> [!theorem] Exponential Sparsity
> 
> The volume of a hypercube with edge $r = 0.1$ is $0.1^p$, which satisfies:
> 
> $$\lim_{p \to \infty} 0.1^p = 0$$
>![[The Curse of Dimensionality-1780667044336.webp|553x246]]
>
> Consequently, the probability of capturing any data point in a small region becomes negligible, implying that points in high-dimensional spaces are isolated. To overcome this sparsity, the required sample size grows exponentially with $p$.

> [!theorem] Distance Concentration
> 
> Let $X, Y \in [0,1]^p$ be two independent random variables with a uniform distribution. The squared distance satisfies:
> 
> $$\mathbb{E}[\|X - Y\|^2] = \frac{p}{6} \qquad \text{and} \qquad \text{Std}[\|X - Y\|^2] \simeq 0.2\sqrt{p}$$
> ![[The Curse of Dimensionality-1780667174803.webp]]
> 
> As $p$ grows, all pairwise distances concentrate around the same value, causing the histogram of distances to become a narrow spike. This removes any meaningful distinction between the "closest" and "farthest" point, meaning the notion of nearest neighbors vanishes.
>
>| **Dimension** | **Distance Distribution**           |
| ------------- | ----------------------------------- |
| $p = 2$       | Spread across $[0, 1.2]$            |
| $p = 100$     | Concentrated near $4$               |
| $p = 1000$    | Very tightly concentrated near $12$ |
>


### Classification in High Dimensions

> **Context:** The empty nature of high-dimensional spaces shifts the statistical challenge from finding boundaries to managing the structural complexity and generalization capabilities of models.

> [!theorem] Linear Separability and Overfitting
> 
> Because high-dimensional spaces are nearly empty:
> -  it becomes easier to separate classes with an adapted classifier.
> - The larger $p$, the higher the probability that a separating hyperplane exists.
>
>![[The Curse of Dimensionality-1780667315782.webp|512x239]]
> 
> However, this induces a high risk of **Overfitting**: a classifier that perfectly separates training data in high dimensions will often generalize poorly to new data.

### Concentration Phenomena
> **Context:** High-dimensional geometry exhibits counter-intuitive volume concentrations where standard intuitions regarding solids, boundaries, and centers no longer apply.

> [!definition] Volume of a p-Dimensional Ball
> 
> The volume of a $p$-dimensional ball of radius $r$ is given by:
> 
> $$\boxed{V_p(r) = r^p \cdot \frac{\pi^{p/2}}{\Gamma(p/2 + 1)}}$$
> ![[The Curse of Dimensionality-1780667375805.webp]]
> 
> where $\Gamma$ denotes the Gamma function. This volume peaks around $p \approx 5$ and rapidly collapses to $0$ as $p \to \infty$.

> [!theorem] Unit Ball Covering Bound
> 
> To cover a unit hypercube $[0,1]^p$ with $n$ unit balls, the required number of balls $n$ must satisfy:
> 
> $$n \geq \frac{1}{V_p} = \frac{\Gamma(p/2+1)}{\pi^{p/2}} \overset{p \to \infty}{\sim} \left(\frac{p}{2\pi e}\right)^{p/2} \sqrt{p\pi}$$

> [!theorem] Volume Concentration in the Shell
> 
> The probability that a uniformly distributed point $X$ on the unit ball lies in the thin outer shell between radii $0.9$ and $1$ is defined by:
> 
> $$\boxed{P(X \in S_{0.9}(p)) = 1 - 0.9^p \xrightarrow{p \to \infty} 1}$$
> ![[The Curse of Dimensionality-1780668700113.webp]]
> 
> This proves that in high dimensions, almost all the volume of a ball is concentrated near its surface.

> [!theorem] Distribution of Samples Near the Boundary
> 
> Let $X_1, \ldots, X_n$ be i.i.d. random variables with a uniform distribution on the unit ball in $\mathbb{R}^p$. The median distance from the origin to the closest data point is given by:
> 
> $$\boxed{\text{med}(p, n) = \left(1 - \frac{1}{2^{1/n}}\right)^{\frac{1}{p}}}$$
> 
> This implies that most data points are closer to the edge of the ball than to its center.

### Summary of High-Dimensional Geometric Phenomena

> **Context:** The curse of dimensionality represents a collection of interrelated geometric properties that fundamentally impede mathematical modeling and statistical learning in high dimensions.

|**Phenomenon**|**Effect**|
|---|---|
|Sparse neighborhoods|Local methods (kNN, kernel smoothing) fail|
|Uniform distance concentration|No meaningful "nearest" neighbor|
|Volume near the shell|Data lies on the boundary, not the interior|
|Exponential sample growth needed|Practical datasets are always too small|
|Easy linear separation|High risk of overfitting|
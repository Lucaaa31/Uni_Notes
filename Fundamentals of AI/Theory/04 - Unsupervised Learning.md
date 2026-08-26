Given $x_1, x_2, ..., x_n$, without labels, output hidden structures behind the $x$'s.
Example:
![[04 - Unsupervised Learning-1787039329651.webp|590]]
# K-means and Gaussian Mixture Model
## Clustering
> The process of grouping a set of objects into classes of similar objects.

Properties:
- high intra-class similarity
- low inter-class similarity
- it is subjective
## K-means Clustering Problem
Given a set of observations $(x_1, x_2, ..., x_n)$, where $x_i \in \mathbb{R}^d$.
The problem consists into:
$$
\arg \min_S \sum_{i=1}^K \sum_{x_j \in S_j} \| x_j - \mu_i \|^2
$$
where:
- **$S = \{S_1, S_2, \dots, S_K\}$** (**Partition / Cluster Set**) — The set of all $K$ disjoint clusters partitioning the dataset.
- **$K$** (**Number of Clusters**) — Hyperparameter specifying the total count of groups to form.
- **$\sum_{i=1}^K$** (**Outer Summation**) — Iterates through each cluster from $1$ to $K$.
- **$\sum_{x_j \in S_i}$** (**Inner Summation**) — Iterates through every individual data point $x_j$ assigned to cluster $S_i$.    
- **$\mu_i$** (**Centroid**) — The mean vector (center of mass) of all points belonging to cluster $S_i$.
- **$\Vert{} x_j - \mu_i \Vert{}^2$** (**Squared Euclidean Distance**) — Measures the distance squared between data point $x_j$ and centroid $\mu_i$.
### Example with $K=3$
![[04 - Unsupervised Learning-1787040003234.webp|588]]
### Complexity
The problem is Non-deterministic Polynomial time (NP), but there are good heuristic algorithms that seem to work well in practice:
- K-means algorithm
- mixture of Gaussians
### Algorithm
> [!important] K-Means Algorithm
> 
> **Input**
> * Data + Desired number of clusters, $K$
> 
> **Initialize**
> * The $K$ cluster centers (randomly if necessary)
> 
> **Iterate**
> 1. **Assignment:** Decide the class memberships of the $n$ objects by assigning them to the nearest cluster centers
> 2. **Update:** Re-estimate the $K$ cluster centers (centroid or mean), by assuming the memberships found above are correct
> 
> **Termination**
> * If none of the $n$ objects change membership (or a maximum number of iterations is reached)

**K-means is guaranteed to terminate** in a finite number of iterations:
1. Each step reduces the within-cluster sum of squares (WCSS)
2. There are a finite number of possible cluster partitionings.
However, it is not guaranteed to find the global optimum, only a local one.
### Seed Choice
The results of the K-means Algorithm can vary based on random seed selection.
- Some seeds can result in **poor convergence rate**, or convergence to **sub-optimal** clustering
- K-means algorithm can get stuck easily in **local minima**.

---
## Alternating Optimization
### K-means more formally
* **Randomly initialize k centers**$$\mu^0 = (\mu_1^0, \dots, \mu_K^0)$$
* **Classify:** At iteration $t$, assign each point $j \in \{1, \dots, n\}$ to nearest center:$$C^t(j) \leftarrow \arg\min_i \|\mu_i^t - x_j\|^2$$
* **Recenter:** $\mu_i$ is the centroid of the new sets:$$\mu_i^{(t+1)} \leftarrow \arg\min_\mu \sum_{j : C^t(j) = i} \|\mu - x_j\|^2$$
### Optimization
As we can see in the algorithm, there are two quantity that are similar:
$$
\|\mu_i^t - x_j\|^2 \qquad \text{ and } \qquad \|\mu - x_j\|^2
$$
We can write them into a single equation and minimize only it.
#### Implementation
Define the following potential function $F$ of centers $\mu$ and point allocation $C$.
$$\mu = (\mu_1, \dots, \mu_K)$$
$$C = (C(1), \dots, C(n))$$
$$\begin{aligned}
F(\mu, C) &= \sum_{j=1}^{n} \|\mu_{C(j)} - x_j\|^2 \\
&= \sum_{i=1}^{K} \sum_{j : C(j) = i} \|\mu_i - x_j\|^2
\end{aligned}
$$
**Optimal solution of the K-means problem:**
$$\boxed{\min_{\mu, C} F(\mu, C)}$$
--- 
## Gaussian Mixture Model
### Generative Approach
$$p(x_1, \dots, x_n \mid \theta) \stackrel{\substack{\text{i.i.d. data}}}{=} \prod_{i=1}^{n} p(x_i \mid \theta)$$
- $\theta$ is a latent parameter, it is not observable and it governs data
- all the observed data $x_i$, are drawn based on the parameter $\theta$

**Problem:** If data have more subgroups or distinct cluster, the approximation using only the parameter $\theta$ may result wrong.

**Solution:** Instead of having only 1 parameter we divide the domain in partitions and assignee, for each part, a parameter: $[\theta_1, \theta_2, ..., \theta_n]$, the technique used are:
- Mixture modelling
- Partitioning algorithm
	- K-means
### Differences of the two approaches
- **K-means**
	- **hard assignment:** assign each object to one cluster $$\theta_i \in \{\theta_1, ..., \theta_k\}$$
- **Mixture modelling**
	- **soft assignment:** assign a probability to each object of belonging to a cluster $$(\pi_1, \dots, \pi_K), \quad \pi_i \ge 0, \quad \sum_{i=1}^{K} \pi_i = 1$$
## GMM
**Mixture of $K$ Gaussians distribution**, that is a multimodal distribution:
- There are $K$ components that belongs to different clusters
- Each component has associated a mean vector $\mu_i$

A component $i$ generates data from $\mathcal{N}(\mu_i, \sum_i)$:
- $\mathcal{N}$ normal (or gaussian) distribution
- $\mu_i$ centroids
- $\Sigma_i$ is the covariance matrix
![[04 - Unsupervised Learning-1787210720732.webp|343]]
### Generation process
Each data point is generated using this process:
1. Choose component $i$ with probability $\pi_i = P(y=1)$
2. Datapoint $x \sim \mathcal{N}(\mu_i, \Sigma_i)$

### Formulas
$$p(x \mid y = i) = \mathcal{N}(\mu_i, \Sigma_i)$$
- **$y$**: Hidden variable

$$p(x) = \sum_{i=1}^{K} p(x \mid y = i) P(y = i)$$

- **$p(x)$**: Observed data
- **$p(x \mid y = i)$**: Mixture component
- **$P(y = i)$**: Mixture proportion

### Mixture of Gaussians Clustering
**Assume that**:
- $\Sigma_i = \sigma^2 \mathbf{I}$, for simplicity. 
- $p(x|y=i) = N(\mu_i, \sigma^2 \mathbf{I})$ 
- $p(y=i) = \pi_i$ * $\mu_1, \dots, \mu_K, \sigma^2, \pi_1, \dots, \pi_K$ are known. 
**Cluster x based on posteriors:** $$ \begin{aligned} \log \frac{P(y=i|x)}{P(y=j|x)} &= \log \frac{p(x|y=i) P(y=i) / p(x)}{p(x|y=j) P(y=j) / p(x)} \\ &= \log \frac{p(x|y=i) \pi_i}{p(x|y=j) \pi_j} = \log \frac{\pi_i \exp(\frac{-1}{2\sigma^2} \|x - \mu_i\|^2)}{\pi_j \exp(\frac{-1}{2\sigma^2} \|x - \mu_j\|^2)} = w^T x \end{aligned} $$
- $w$ depends on $\mu_1, ..., \mu_k, \sigma^2, \pi_1, ..., \pi_k$.
![[04 - Unsupervised Learning-1787213982640.webp|357]]
It is a **Linear Decision Boundary** because the second-order terms are cancel out.

#### Maximum Likelihood Estimate for GMM
If we don't know $\mu_1, ..., \mu_k, \sigma^2, \pi_1, ..., \pi_k$ we can use the MLE to estimate them:
$$
\theta = [\mu_1, \dots, \mu_K, \sigma^2, \pi_1, \dots, \pi_K]
$$
$$\begin{aligned} \arg\max_{\theta} & \prod_{j=1}^{n} P(x_j\vert{}\theta) \\ &= \arg\max_{\theta} \prod_{j=1}^{n} \sum_{i=1}^{K} P(y_j = i, x_j\vert{}\theta) \\ &= \arg\max_{\theta} \prod_{j=1}^{n} \sum_{i=1}^{K} P(y_j = i\vert{}\theta) \, p(x_j \mid y_j = i \, \theta) \\ &= \arg\max_{\theta} \prod_{j=1}^{n} \sum_{i=1}^{K} \pi_i \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left( \frac{-1}{2\sigma^2} \Vert{}x_j - \mu_i\Vert{}^2 \right) \end{aligned}$$

---
## Expectation Maximization
EM is an optimization strategy for objective functions that can be interpreted as **likelihoods in the presence of missing data**.
Unlike gradient methods we no need to choose the step size.
It is an iterative algorithm with two linked steps:
- **E-step:** fill-in hidden values using inference
- **M-step:** apply standard MLE/MAP method to completed data
This procedure improves the likelihood. EM always converges to a local optimum of the
likelihood.
### Expectation (E) Step
The **Expectation (E) step** estimates the posterior probability distribution over hidden/latent cluster assignments given the current parameter estimates $\theta^{t-1} = [\mu_1^{t-1}, \dots, \mu_K^{t-1}]$.
- **Auxiliary Function $Q$**: Constructs the expected complete-data log-likelihood:
  $$Q(\theta \mid \theta^{t-1}) = \sum_{j=1}^n \sum_{i=1}^K P(y_j = i \mid x_j, \theta^{t-1}) \log P(x_j, y_j \mid \theta^t)$$
- **Soft Assignment Calculation**: Computes the responsibility $P(y_j = i \mid x_j, \theta^{t-1})$ using Bayes' rule with a Gaussian observation model:
  $$P(y_j = i \mid x_j, \theta^{t-1}) = \frac{\exp\left(-\frac{1}{2\sigma^2}\|x_j - \mu_i^{t-1}\|^2\right)\pi_i}{\sum_{l=1}^K \exp\left(-\frac{1}{2\sigma^2}\|x_j - \mu_l^{t-1}\|^2\right)\pi_l}$$
This step is equivalent to assigning data points to clusters in K-Means, but in a **soft/probabilistic** manner rather than hard assignment.

### Maximization (M) Step
The **Maximization (M) step** updates the parameters $\theta^t$ by maximizing the expected log-likelihood function $Q(\theta \mid \theta^{t-1})$ constructed during the E-step.

- **Weight Substitution**: Plugs in the computed posterior probabilities $R_{i,j}^{t-1} = P(y_j = i \mid x_j, \theta^{t-1})$ as fixed weights.
- **Optimization via Derivatives**: Takes the gradient of $Q$ with respect to the cluster parameters $\mu_i^t$ and sets it to zero:
  $$\frac{\partial}{\partial \mu_i^t} Q(\mu_i^t \mid \theta^{t-1}) = 0 \implies \sum_{j=1}^n R_{i,j}^{t-1} (x_j - \mu_i^t) = 0$$
- **Parameter Update Rule**: Solves for the new cluster center $\mu_i^t$ as a weighted average of all data points:
$$\mu_i^t = \sum_{j=1}^n w_j x_j \quad \text{where} \quad w_j = \frac{R_{i,j}^{t-1}}{\sum_{l=1}^n R_{i,l}^{t-1}}$$
This step is equivalent to updating the cluster centers in K-Means, where each centroid is recomputed using soft-weighted contributions from all data points.














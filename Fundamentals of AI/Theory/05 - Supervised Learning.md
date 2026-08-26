# K-Nearest Neighbors
Given $\{x_i, y_i\}$:
- $x_i$, the training example
- $y_i$, the label
The algorithm:
- Compute a certain distance $D(x, x_i)$ to every training example $x_i$
- Select $k$ closest instances $x_{i1}, ..., x_{ik}$ and their labels $y_{i1}, ..., y_{ik}$
- Output of the class $y^*$ which is most frequent in $y_{i1}, ..., y_{ik}$
![[05 - Supervised Learning-1787304108768.webp|399]]
## Choosing the value of K
![[05 - Supervised Learning-1787304196141.webp]]
The value of K has strong affect on KNN performances:
- **Large K**, everything is classified as the most probable class, $P(y)$
- **Small K**, highly variable and unstable decision boundary
### p-fold Cross Validation
For selecting the value we have to:
- set aside a portion of the training set
- vary k, observe training validation error
- choose the k with the better performances
It is used the **Cross Validation** in order to do this.
### Choosing the right Distance
The Distance Measure is a key component of the KNN, because it defines which components are similar and which not, so it strongly affects the performance of the algorithm.

- **Minkowski Distance ($L_p$ norm)** (General)
  $$L_p(x, y) = \left( \sum_{i=1}^{d} |x_i - y_i|^p \right)^{\frac{1}{p}}$$
- **$p=1$: Manhattan / City Block Distance ($L_1$ norm)**
  $$L_1(x, y) = \sum_{i=1}^{d} |x_i - y_i|$$
- **$p=2$: Euclidean Distance ($L_2$ norm)**
  $$L_2(x, y) = \left( \sum_{i=1}^{d} (x_i - y_i)^2 \right)^{1/2}$$
- **$p=\infty$: Chebyshev Distance (Chess board)**
  $$L_\infty(x, y) = \max_i |x_i - y_i|$$
- **$p=0$: Hamming Distance**
  $$L_H(x, y) = \sum_{i=1}^{d} \mathbf{1}_{x_i \neq y_i}$$
## KNN Pros and Cons
### Pros
Only few assumptions about data:
- **Smoothness Assumption**, nearby regions of space have the same class
- The distance function
Non-parametric approach:
- Nothing to infer from the data, except K and the Distance Function
- For updating we only need to add new item to training set
- No issue if we want to add a new class
Good generalization guarantee for large number of samples ($< 2 *$Bayes error).
### Cons
Computationally Expensive:
- space, we need to store all training examples
- time, need to compute distance to all examples
- $O(ND)$
	- $N=\#$ training examples
	- $D=$ cost of computing distance
When $N$ grows the system become slower.
## KNN tricks
We will see three method used for reducing the complexity of the KNN.
### 1) Partial distance calculation
In the partial distance algorithm, we calculate the distance using some subset $r$ of the full $d$ dimensions. If this partial distance is too great, we stop computing.

The partial distance based on $r$ selected dimensions is:
$$D_r(\mathbf{a}, \mathbf{b}) = \left( \sum_{k=1}^r (a_k - b_k)^2 \right)^{1/2}$$

Where $r < d$.
The partial distance method assumes that the dimensional subspace we define is indicative of the full data space.

### 2) Pre-structuring / Searching for Prototypes
Explore similarities between samples to represent data as search trees of *prototypes*.![[05 - Supervised Learning-1787320994451.webp|434]]
- **Advantages:** Complexity decreases
- **Disadvantages:**
  - finding good search tree is not trivial
  - will not necessarily find the closest neighbor, and thus **not** guaranteed that the decision boundaries stay the same
### 3) Editing
Editing prune useless point during training. A simple method is to remove all the points that have identical class to all their K nearest neighbors.
![[05 - Supervised Learning-1787321199693.webp|213]]
Thanks to this the complexity can be reduced without reducing accuracy.
This algorithm does not guarantee a minimal set of points is found and prevents the addition of training data later on because it would invalidate the earlier data pruning.
### 4) Separate feature normalization
Notice the 2 features are on different scales:
  - feature 1 takes values between 1 or 2
  - feature 2 takes values between 100 to 200
If $X$ is a random variable of mean $\mu$ and variance $\sigma^2$, then $(X - \mu)/\sigma$ has mean 0 and variance 1

We could normalize each feature to be between of mean 0 and variance 1

Thus for each feature vector $\mathbf{x}_i$, compute its sample mean and variance, and let the new feature be $[\mathbf{x}_i - \text{mean}(\mathbf{x}_i)] / \sqrt{\text{var}(\mathbf{x}_i)}$

---
# Perceptron
![[05 - Supervised Learning-1787498274585.webp|523]]
$$
f(x) = \sigma (\langle w, x \rangle + b)
$$
Components breakdown:
-  **$x$**: **Input Vector** — The vector containing the feature values for a given instance. 
- **$w$**: **Weight Vector**  — The vector of parameters (learned weights) that assign relative importance to each feature in $x$. 
- **$\langle w, x \rangle$**: **Dot Product** — The scalar inner product $\sum_{i=1}^d w_i x_i$, representing the linear weighted sum of the inputs. 
- **$b$**: **Bias Term** ($b \in \mathbb{R}$) — A scalar parameter that shifts the decision boundary independently of the inputs. 
- **$\langle w, x \rangle + b$**: **Pre-Activation Value** (Logit) — The linear combination of inputs, weights, and bias prior to applying the non-linear activation. 
- **$\sigma(\cdot)$**: **Sigmoid Activation Function** — The non-linear activation function, defined as $\sigma(z) = \frac{1}{1 + e^{-z}}$, which squeezes any real-valued number into the range $(0, 1)$. 
- **$f(x)$**: **Output Value** — The final model prediction, typically interpreted as the conditional probability $P(y=1 \mid x)$ in binary classification.

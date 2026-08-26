# Risk Theory

Given a distribution $\mathcal{D}$ over space of labeled examples $\mathcal{X} \times \mathcal{Y}$, $\mathcal{D}$ is unknown and it represents the population we care about. Assume the labels are produced by a function $f : \mathcal{X} \rightarrow\mathcal{Y}$ and $Y = f(X)$.

## Loss

We define the loss $L(h(X), Y)$ of the classifier $h$ as the function that measures the error between the assigned prediction and the prediction of the correct labeling function.

Here's an example of 0-1 loss:
$$L(h(X), f(X)) = L(h(X), Y) = \begin{cases} 1, & \text{if } h(X) \ne Y \\ 0, & \text{if } h(X) = Y \end{cases}$$

## True Risk

The true risk of a classifier $h$ is the probability that it does not predict the correct label on a random data point drawn for $\mathcal{D}$:
$$R_\mathcal{D}(h) := \mathbb{P}[h(X) \ne f(X)] = P[h(X) \ne Y]$$

In other words, the true risk of the learner $h$ is the expected value of the loss:
$$R_\mathcal{D}(h) := \mathbb{E}_{X \sim \mathcal{D}}\{L(h(X), Y)\}$$

$$\text{GenLoss}(h) = \sum_{(X, Y) \in \mathcal{D}}L(h(X), Y) P(X, Y)$$

---

# PAC Learning Theory

## Introduction

### Batch Learning
In practical cases we do not know $f$ and we need to estimate $h$ from data.

So:
* **The learner's input:** Training data, $S = \{(\mathbf{x}_1, y_1) \cdots (\mathbf{x}_m, y_m)\} \in (\mathcal{X} \times \mathcal{Y})^m$
* **The learner's output:** A prediction rule, $h : \mathcal{X} \rightarrow \mathcal{Y}$
* **Goal:** $h$ should be correct on future examples.

When is there hope for finding $h$ with high accuracy?

### IID Condition
Data $(x_1, y_1), (x_2, y_2), ..., (x_n, y_n)$ are identically and independently distributed random labeled example with distribution $D$.

This assumption is the connection between what we've seen in the past to what we expect to see in the future.

### Empirical Risk
It is the average loss across the entire dataset:
$$R_S(h) = \frac{1}{m} \sum_{i=1}^m \mathbb{1} [h(x_i) \ne y_i]$$

where:
$$\mathbb{1}[\text{condition}] = \begin{cases} 1 & \text{the condition is TRUE} \\ 0 & \text{the condition is FALSE} \end{cases}$$

## Probably Approximately Correct (PAC) Learning

PAC Learning is a theoretical framework in computational learning theory that formalizes the mathematical definition of learnability. In this model, a learning algorithm (learner) attempts to learn an unknown target concept from a data distribution $\mathcal{D}$ without direct knowledge of $\mathcal{D}$ or the optimal Bayes predictor.

### Key Parameters

* **Accuracy Parameter ($\epsilon$):** Sets the maximum allowable risk/error bound for the learned hypothesis ($R_{\mathcal{D}}(h) \le \epsilon$).
* **Confidence Parameter ($\delta$):** Sets the maximum probability of failure. The algorithm must succeed with a confidence of at least $1 - \delta$.
* **Sample Complexity ($m(\epsilon, \delta)$):** The required size of the training dataset $S$. It depends solely on $\epsilon$ and $\delta$, remaining independent of the underlying distribution $\mathcal{D}$ or the target function.

### Mathematical Formalization

A concept class is PAC learnable if the learner outputs a hypothesis $h$ such that:
$$\mathbb{P}_{S \sim \mathcal{D}^m} \left[ R_{\mathcal{D}}(h) \le \epsilon \right] \ge 1 - \delta$$

The learner is guaranteed to be **Probably** (with confidence $\ge 1 - \delta$) **Approximately** (with error/risk $\le \epsilon$) **Correct**.

---

# Realizable Setting

## Realizability Assumption

In binary classification ($\mathcal{Y} = \{0, 1\}$), the Realizability Assumption states that the chosen hypothesis class $\mathcal{H}$ contains a "perfect" function $h^*$.

### Formal Definition
$$\exists \, h^* \in \mathcal{H} \quad \text{s.t.} \quad R_{\mathcal{D}}(h^*) = 0$$

### Key Takeaways
* **Bayes Optimal Classifier:** Since $R_{\mathcal{D}}(h^*) = 0$, $h^*$ is equivalent to the Bayes classifier (zero generalization error, no noise in the target labeling).
* **Learner Knowledge:** The learning algorithm knows the hypothesis class $\mathcal{H}$, but does not know $h^*$ or the distribution $\mathcal{D}$.
* **Common Examples of Hypothesis Classes ($\mathcal{H}$)**

## Learning in the Realizable Setting

In the realizable setting, a intuitive strategy for a learning algorithm is to output any hypothesis $h \in \mathcal{H}$ that perfectly fits the training dataset $S$ (i.e., achieves zero empirical error, $R_S(h) = 0$).

### Empirical Risk Minimization (ERM)
This approach is a special case of Empirical Risk Minimization (ERM), where the chosen hypothesis $h_S$ minimizes the empirical loss over $m$ training samples:
$$h_S = \arg\min_{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^{m} \mathbb{1}[h(\mathbf{x}_i) \neq y_i]$$

### Key Concepts
* **Consistent Classifier:** A classifier $h \in \mathcal{H}$ that makes no errors on the training data ($R_S(h) = 0$). As the training set size increases, its performance approaches the Bayes risk.
* **Bounded True Risk:** By finding a hypothesis with minimal empirical risk ($R_S(h) = 0$), we guarantee that the true risk $R_{\mathcal{D}}(h)$ over the whole distribution remains bounded.

### Formal Definition of PAC Learnability (Realizable Case)
A hypothesis class $\mathcal{H}$ is PAC learnable if there exists a learning algorithm and a sample complexity function $m_{\mathcal{H}}(\epsilon, \delta)$ such that:

For any target accuracy $\epsilon \in (0,1)$, confidence $\delta \in (0,1)$, and distribution $\mathcal{D}$ satisfying the realizability assumption:

When trained on $m \ge m_{\mathcal{H}}(\epsilon, \delta)$ i.i.d. examples from $\mathcal{D}$, the algorithm outputs a hypothesis $h$ satisfying:
$$\mathbb{P}_{\mathcal{D}} \left[ R_{\mathcal{D}}(h) \le \epsilon \right] \ge 1 - \delta$$

#### Key Takeaways
* **Sample Complexity ($m_{\mathcal{H}}$):** The function determining the minimum number of training samples needed to guarantee PAC bounds. It depends only on $\epsilon$ and $\delta$.
* **Core Question Answered:** PAC learnability determines whether a hypothesis class $\mathcal{H}$ can be learned under realizability, and precisely how many samples are required.

---

# Agnostic PAC Learning

## From Realizable to Agnostic PAC Learning

### 1. Sample Complexity for Finite Hypothesis Classes (Realizable Case)
For a finite hypothesis class $\mathcal{H}$ under the realizability assumption, the ERM rule guarantees PAC learnability with sample complexity:
$$m_{\mathcal{H}}(\epsilon, \delta) \le \frac{\ln(|\mathcal{H}|/\delta)}{\epsilon}$$

**Open Questions:**
* What happens when the hypothesis class $\mathcal{H}$ is infinite?
* Does PAC learning still make sense if the realizability assumption does not hold?

### 2. Waiving the Realizability Assumption
In real-world applications, the realizability assumption rarely holds because datasets often contain noise or $\mathcal{H}$ does not contain the true Bayes classifier.

* **Realizable Goal:** Find $h \in \mathcal{H}$ such that $R_{\mathcal{D}}(h) \le \epsilon$.
* **Agnostic Goal:** Find $h \in \mathcal{H}$ whose error is not much worse than the best possible predictor in $\mathcal{H}$:
$$R_{\mathcal{D}}(h) \le \min_{h' \in \mathcal{H}} R_{\mathcal{D}}(h') + \epsilon$$

### 3. Agnostic PAC Learning Corollary
Let $\mathcal{H}$ be a finite hypothesis class and $L$ a loss function bounded in $[0, 1]$. Then $\mathcal{H}$ is Agnostic PAC learnable using the ERM learning rule.

**Sample Complexity Bound:**
$$m_{\mathcal{H}}(\epsilon, \delta) \le \frac{\ln\left(\frac{2|\mathcal{H}|}{\delta}\right)}{2\epsilon^2}$$

* **Key Difference:** The sample complexity grows with $\frac{1}{\epsilon^2}$ (instead of $\frac{1}{\epsilon}$) because we can no longer guarantee finding a zero-error hypothesis on the training data.

---

# Shattering and VC-Dimension

To analyze infinite hypothesis classes, we measure their expressive capacity using the Vapnik-Chervonenkis (VC) Dimension.

## 1. Shattering
Let $S$ be a dataset of points. The restriction of $\mathcal{H}$ to $S$, denoted as $\mathcal{H}[S]$, is the set of all possible labelings/splittings that functions in $\mathcal{H}$ can assign to $S$.

$\mathcal{H}$ shatters $S$ if $|\mathcal{H}[S]| = 2^{|S|}$.

* **Meaning:** $\mathcal{H}$ is flexible enough to express all $2^{|S|}$ possible binary labelings over the points in $S$.

## 2. VC-Dimension Definition
The VC-dimension of a hypothesis space $\mathcal{H}$ is the cardinality (size) of the largest finite set $S$ that can be shattered by $\mathcal{H}$:
$$\text{VCdim}(\mathcal{H}) = \max \{ |S| : \mathcal{H} \text{ shatters } S \}$$

If $\mathcal{H}$ can shatter arbitrarily large finite sets, then $\text{VCdim}(\mathcal{H}) = \infty$.

> **Fundamental Result:** A hypothesis class $\mathcal{H}$ is PAC-learnable if and only if its VC-dimension is finite ($\text{VCdim}(\mathcal{H}) < \infty$).

---

# Not PAC Learnable & Infinite Hypothesis Classes

## The Fundamental Limit (Infinite VC-Dimension)

* **Theorem:** If a hypothesis class $\mathcal{H}$ has an infinite VC-dimension ($\text{VCdim}(\mathcal{H}) = \infty$), then $\mathcal{H}$ is not PAC learnable.
* **Intuition:** If $\mathcal{H}$ can shatter a set of size $2m$, learning with $m$ examples fails because the observed samples provide zero predictive information about the remaining instances.
* **Overfitting Principle:** Every possible label assignment for the unseen data can still be justified by some hypothesis in $\mathcal{H}$, destroying generalization capability ("a theory that can explain everything and its contrary is worthless").

## The Discretization Trick (Bridging Theory and Practice)

* **Computer Representation:** Real-world algorithms run on computers using finite precision (e.g., 32-bit floating-point numbers for parameters).
* **Finite Cardinality:** Because parameters are discretized, an infinite theoretical hypothesis class becomes finite in practice. For $d$ parameters using 32-bit representation, the number of possibilities is bounded by:
$$|\mathcal{H}_{\text{discrete}}| \le 2^{32d}$$
* **The Rule of Thumb:** Since $\ln|\mathcal{H}|$ scales linearly with $d$, the sample complexity bounds yield $\ln|\mathcal{H}| \approx 10d$, justifying the common heuristic in machine learning: use at least 10 times as many training samples as parameters.

---
# Regularization
Solution to the problem of overfitting.
Control complexity by penalizing complex models in learning.

Given training data  $S$ with $m$ samples, return any $h \in \mathcal{H}$ s.t.
$$
R_S(h) = 0
$$
This is a special case of **Empirical Risk Minimization** (ERM), that is:
$$
h_S = \arg\min_{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^{m} \mathbb{1}[h(\mathbf{x}_i) \neq y_i] \textcolor{red}{+ \lambda \text{ Complexity}(h)}
$$
where:
- $\lambda$ is a positive number that serves as a conversion rate when the loss and the hypothesis complexity do not have the same scale
- the form of the complexity function depends on the hypothesis space
- we still need cross validation and in this case we need to include the parameter $\lambda$ select the one that gives the best validation score
---
# Linear Regression
Regression analysis considers prediction problems where **label space $Y$ is continuous**.
Measuring quality of predictor $h$ using prediction error $P[h(X) \ne Y]$ does not make sense, so the goal is to find $h: X \rightarrow Y$, from some class $H$, minimizing error measured using a loss function $L: Y \times Y \rightarrow \mathbb{R}$:
$$
\mathbb{E}[L(Y, h(X))]
$$
A standard choice for the loss function in regression is the **Squared Loss**:
$$
L(\hat y, y) = (\hat y - y)^2
$$
We consider a parametric function $h_w(x)$ the **Empirical Loss** of the function $y=h_w(x)$ on a set S is:
$$
L_S(w) = \frac{1}{N} \sum_{i=1}^N L(h_w(x_i), y_i)
$$
## Linear Fitting to Data
We want to fit a linear function to an observed set of points $X = [x_1, ..., x_N]$ with associated labels $Y = [y_1, ..., y_N].$

### Least Squares Criterion (LSQ)
Given pairs of input/output values, find $(b, w)$ to minimize the prediction errors (on average), using square loss on $\mathcal{H} = \{h_{(b, w)}: b \in \mathbb{R}, w \in \mathbb{R}^d\}$:  squared error on $(x, y)$ is $(y - (b + \langle \mathbf{w}, \mathbf{x} \rangle))^2$.
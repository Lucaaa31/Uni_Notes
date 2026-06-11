### What is Machine Learning?

> **Context:** Machine learning consists of computational methods that use experience to improve performance. Since experience is a data-driven task, the field draws on statistics, probability, and optimization, while the computer-science angle contributes learning algorithms, complexity analysis, and theoretical guarantees.

> [!definition] Broad ML Tasks
> 
> - **Classification** — assign a category to each item (e.g. document classification).
>     
> - **Regression** — predict a real value for each item (e.g. stock values).
>     
> - **Ranking** — order items by some criterion (e.g. relevant web pages).
>     
> - **Clustering** — partition data into homogeneous regions.
>     
> - **Dimensionality reduction** — find a lower-dimensional manifold preserving some properties of the data.
>     

### Terminology and Data Types

> **Context:** The basic vocabulary of supervised learning distinguishes the items, their attributes, and their targets, as well as the different roles that labeled data can play during learning and evaluation.

> [!definition] Core Terminology
> 
> - **Example** — an item, an instance of the data used.
>     
> - **Features** — attributes associated with an item, often represented as a vector.
>     
> - **Labels** — the category (classification) or real value (regression) associated with an item.
>     
> 
> **Data types:**
> 
> - **Training data** — typically labeled.
>     
> - **Test data** — labeled, but labels not seen.
>     
> - **Validation data** — labeled, used for tuning parameters.
>     

### General Learning Scenarios

> **Context:** Learning settings differ in how the data arrives and how the learner interacts with it, and standard batch scenarios are distinguished by what kind of data is available and where predictions are made.

> [!definition] Settings and Queries
> 
> - **Batch** — learner receives the full training sample, then predicts on unseen points.
>     
> - **Online** — learner receives one sample at a time and predicts for that sample.
>     
> - **Active** — the learner can request the label of a point.
>     
> - **Passive** — the learner receives labeled points.
>     

> [!definition] Standard Batch Scenarios
> 
> | Scenario | Labeled | Unlabeled | Predicts on |
> | --- | :-: | :-: | --- |
> | **Unsupervised** | — | ✓ | — |
> | **Supervised** | ✓ | — | Unseen points |
> | **Semi-supervised** | ✓ | ✓ | Unseen points |
> | **Transduction** | ✓ | ✓ | **Seen** points |

### Formal Set-Up

> **Context:** The supervised learning problem is formalized through input/output spaces, a loss function quantifying prediction cost, and a hypothesis set encoding prior knowledge from which the learner selects its hypothesis.

> [!definition] Spaces, Loss, and Hypothesis Set
> 
> - **Input space** $\mathcal{X}$, **output space** $\mathcal{Y}$.
>     
> - **Loss function** $L : \mathcal{Y} \times \mathcal{Y} \to \mathbb{R}$, where $L(\hat{y}, y)$ is the cost of predicting $\hat{y}$ instead of $y$.
>     
>     - Binary classification (0–1 loss): $L(y, y') = \mathbb{1}_{y \neq y'}$.
>         
>     - Regression: $L(y, y') = (y' - y)^2$, with $\mathcal{Y} \subseteq \mathbb{R}$.
>         
> - **Hypothesis set** $\mathcal{H} \subseteq \mathcal{Y}^{\mathcal{X}}$ — the functions from which the learner selects its hypothesis; represents prior knowledge about the task.
>     

> [!definition] Supervised Learning Problem
> 
> Training data is a sample $S$ of size $m$ drawn i.i.d. from $\mathcal{X} \times \mathcal{Y}$ according to a distribution $\mathcal{D}$:
> 
> $$S = \big((x_1, y_1), \dots, (x_m, y_m)\big)$$
> 
> The problem is to find a hypothesis $h \in \mathcal{H}$ with small generalization error.
> 
> - **Deterministic case:** the label is a deterministic function of input, $y = f(x)$.
>     
> - **Stochastic case:** the output is a probabilistic function of input.
>     

### Errors

> **Context:** Different notions of error measure performance: the generalization error over the true distribution, the empirical error on a finite sample, and the Bayes error as the irreducible minimum, with noise capturing the inherent uncertainty.

> [!definition] Error Definitions
> 
> - **Generalization error:** $$R(h) = \mathbb{E}_{(x,y) \sim \mathcal{D}}\big[L(h(x), y)\big]$$
>     
> - **Empirical error:** $$\widehat{R}(h) = \frac{1}{m} \sum_{i=1}^{m} L(h(x_i), y_i)$$
>     
> - **Bayes error:** $R^\star = \inf_{h \text{ measurable}} R(h)$; in the deterministic case $R^\star = 0$.
> - **Noise** (binary classification): $\text{noise}(x) = \min\{\Pr[1 \mid x],\ \Pr[0 \mid x]\}$, with $\mathbb{E}[\text{noise}(x)] = R^\star$.
 

### Learning ≠ Fitting

> **Context:** Learning is not the same as fitting the training data. The best hypothesis on a sample may not be the best overall, raising the central notion of model complexity and the trade-off that produces underfitting and overfitting.

> [!definition] Generalization
> 
> - The best hypothesis on the sample may **not** be the best overall.
>     
> - **Generalization is not memorization.**
>     
> - Very complex separation surfaces can be poor predictors.
>     
> - **Trade-off:** complexity of the hypothesis set vs. sample size → underfitting / overfitting.
>     
![[1 - Introduction to Machine Learning-1780864721338.webp|461x172]]
### Model Selection

> **Context:** The excess risk of a hypothesis decomposes into an estimation term (which depends on the sample and can be bounded) and an approximation term (which depends only on the hypothesis set). Model selection chooses $\mathcal{H}$ to balance the two.

> [!definition] Estimation–Approximation Decomposition
> 
> For a best-in-class $h^\star \in \mathcal{H}$:
> 
> $$R(h) - R^\star = \underbrace{[R(h) - R(h^\star)]}_{\text{estimation}} + \underbrace{[R(h^\star) - R^\star]}_{\text{approximation}}$$
> 
> - **Approximation** — not a random variable; depends only on $\mathcal{H}$.
>     
> - **Estimation** — the only term we can hope to bound.
>     
> 
> As $\mathcal{H}$ grows, estimation error rises while approximation error falls; the bound has a minimum at some optimal $\mathcal{H}^\star$.

> [!definition] Empirical Risk Minimization (ERM)
> 
> Select a hypothesis set $\mathcal{H}$, then find the hypothesis minimizing empirical error:
> 
> $$h = \arg\min_{h \in \mathcal{H}} \widehat{R}(h)$$
> 
> Limitations: $\mathcal{H}$ may be too complex, or the sample size $m$ may not be large enough.

> [!definition] Generalization Bound and Estimation Error
> 
> A generalization bound is an upper bound on:
> 
> $$\Pr\left[\sup_{h \in \mathcal{H}} \big|R(h) - \widehat{R}(h)\big| > \epsilon\right]$$
> 
> The estimation error of the ERM hypothesis $h_0$ is controlled by:
> 
> $$R(h_0) - R(h^\star) \leq 2 \sup_{h \in \mathcal{H}} \big|R(h) - \widehat{R}(h)\big|$$

> [!definition] Algorithm Families
> 
> - **ERM:** $h = \arg\min_{h \in \mathcal{H}} \widehat{R}(h)$
>     
> - **SRM** (nested $\mathcal{H}_1 \subset \mathcal{H}_2 \subset \cdots$): $h = \arg\min_{h \in \mathcal{H}_n,\, n \in \mathbb{N}} \widehat{R}(h) + \text{penalty}(\mathcal{H}_n, m)$ — strong theoretical guarantees but typically computationally hard.
>     
> - **Regularization-based:** $h = \arg\min_{h \in \mathcal{H}} \widehat{R}(h) + \lambda \|h\|^2$
>     

### Probability Tools — Basic Properties

> **Context:** A handful of elementary probability identities recur throughout the analysis of learning algorithms and the derivation of generalization bounds.

> [!definition] Core Identities
> 
> - **Union bound:** $\Pr[A \lor B] \leq \Pr[A] + \Pr[B]$.
>     
> - **Inversion:** if $\Pr[X \geq \epsilon] \leq f(\epsilon)$, then with probability at least $1 - \delta$, $X \leq f^{-1}(\delta)$.
>     
> - **Jensen's inequality:** if $f$ is convex, $f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)]$.
>     
> - **Expectation:** if $X \geq 0$, $\mathbb{E}[X] = \int_0^{+\infty} \Pr[X > t]\, dt$.
>     

### Basic Inequalities

> **Context:** Markov's and Chebyshev's inequalities give simple tail bounds based on the mean and variance respectively, and serve as building blocks for sharper concentration results.

> [!theorem] Markov's and Chebyshev's Inequalities
> 
> - **Markov** (for $X \geq 0$, $\epsilon > 0$): $$\Pr[X \geq \epsilon] \leq \dfrac{\mathbb{E}[X]}{\epsilon}$$
>     
> - **Chebyshev** (for $\epsilon > 0$): $$\Pr\big[|X - \mathbb{E}[X]| \geq \epsilon\big] \leq \dfrac{\sigma_X^2}{\epsilon^2}$$
>     

### Concentration Inequalities

> **Context:** Concentration inequalities bound how far a sum or function of independent random variables deviates from its expectation. Hoeffding is additive, Chernoff multiplicative, and McDiarmid the most general; Hoeffding is a special case of McDiarmid.

> [!theorem] Hoeffding's Inequality
> 
> Let $X_1, \dots, X_m$ be independent with common expectation $\mu$ and $X_i \in [a, b]$. Then for any $\epsilon > 0$:
> 
> $$\Pr\left[\mu - \frac{1}{m}\sum_{i=1}^m X_i > \epsilon\right] \leq \exp\left(-\frac{2m\epsilon^2}{(b-a)^2}\right)$$
> 
> and symmetrically for the other tail.

> [!theorem] McDiarmid's Inequality
> 
> Let $X_1, \dots, X_m$ be independent values in $\mathcal{U}$ and $f : \mathcal{U}^m \to \mathbb{R}$ satisfy the bounded-differences condition, for all $i$:
> 
> $$\sup_{x_1,\dots,x_m,\,x_i'} \big|f(\dots,x_i,\dots) - f(\dots,x_i',\dots)\big| \leq c_i$$
> 
> Then for all $\epsilon > 0$:
> 
> $$\Pr\Big[\big|f(X_1,\dots,X_m) - \mathbb{E}[f]\big| > \epsilon\Big] \leq 2\exp\left(-\frac{2\epsilon^2}{\sum_{i=1}^m c_i^2}\right)$$
> 
> Hoeffding's inequality is the special case with $f(x_1,\dots,x_m) = \frac{1}{m}\sum_i x_i$ and $c_i = |b_i - a_i|/m$.

> [!theorem] Hoeffding's Lemma
> 
> Let $X \in [a, b]$ with $\mathbb{E}[X] = 0$. Then for any $t > 0$:
> 
> $$\mathbb{E}[e^{tX}] \leq e^{\frac{t^2(b-a)^2}{8}}$$

> [!theorem] Hoeffding's Theorem (general form)
> 
> Let $X_1, \dots, X_m$ be independent with $X_i \in [a_i, b_i]$ and $S_m = \sum_i X_i$. Then for any $\epsilon > 0$:
> 
> $$\Pr[S_m - \mathbb{E}[S_m] \geq \epsilon] \leq e^{-2\epsilon^2 / \sum_{i=1}^m (b_i - a_i)^2}$$
> 
> and symmetrically for the lower tail. It is proved via Chernoff's bounding technique combined with Hoeffding's lemma.

> [!theorem] Hoeffding Corollary and Chernoff's Inequality
> 
> For any $\epsilon > 0$, distribution $\mathcal{D}$, and hypothesis $h : \mathcal{X} \to \{0,1\}$:
> 
> - **Hoeffding corollary:** $\Pr\big[|\widehat{R}(h) - R(h)| \geq \epsilon\big] \leq 2e^{-2m\epsilon^2}$.
>     
> - **Chernoff (multiplicative):** $\Pr[\widehat{R}(h) \geq (1+\epsilon)R(h)] \leq e^{-m R(h)\epsilon^2 / 3}$ and $\Pr[\widehat{R}(h) \leq (1-\epsilon)R(h)] \leq e^{-m R(h)\epsilon^2 / 2}$.
>     

> [!theorem] Weak Law of Large Numbers
> 
> Let $(X_n)$ be independent with common mean $\mu$ and variance $\sigma^2 < \infty$, and $\overline{X}_n = \frac{1}{n}\sum_{i=1}^n X_i$. Then for any $\epsilon > 0$:
> 
> $$\lim_{n \to \infty} \Pr\big[|\overline{X}_n - \mu| \geq \epsilon\big] = 0$$
> 
> It follows from Chebyshev's inequality since $\mathrm{Var}[\overline{X}_n] = \sigma^2/n \to 0$.

> [!theorem] Jensen's Inequality
> 
> Let $X$ be a random variable and $f$ a measurable convex function. Then:
> 
> $$f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)]$$
> 
> It generalizes the convexity property $f(tx + (1-t)y) \leq t f(x) + (1-t)f(y)$ (the chord lies above the curve) to expectations.

## What is Machine Learning?


**Machine Learning:** Computational methods using experience to improve performance.
**Experience** → data-driven task, so it draws on statistics, probability, and optimization.
**Computer science angle** → learning algorithms, analysis of complexity, theoretical guarantees.
**Example:** use document word counts to predict its topic.

### Examples of Learning Tasks

|Domain|Tasks|
|---|---|
|**Text**|Document classification, spam detection|
|**Language (NLP)**|Morphological analysis, POS tagging, context-free parsing, dependency parsing|
|**Speech**|Recognition, synthesis, verification|
|**Image**|Annotation, face recognition, OCR, handwriting recognition|
|**Games**|Chess, backgammon|
|**Control**|Unassisted control of vehicles (robots, cars)|
|**Other**|Medical diagnosis, fraud detection, network intrusion|

### Some Broad ML Tasks

- **Classification** — assign a category to each item (e.g. document classification).
- **Regression** — predict a real value for each item (stock values, economic variables).
- **Ranking** — order items by some criterion (relevant web pages from a search engine).
- **Clustering** — partition data into "homogeneous" regions (analysis of very large datasets).
- **Dimensionality reduction** — find a lower-dimensional manifold preserving some properties of the data.

---

## Objectives of ML

> [!note] General Objectives **Theoretical questions**
> 
> - What can be learned, and under what conditions?
> - Are there learning guarantees?
> - Analysis of learning algorithms.
> 
> **Algorithms**
> 
> - More efficient and more accurate algorithms.
> - Deal with large-scale problems.
> - Handle a variety of different learning problems.

**This course** focuses on: theoretical foundations (learning guarantees, analysis of algorithms), the main mathematically well-studied algorithms (and their extensions), and applications illustrating their use.

---

## Definitions and Terminology

- **Example** — an item, an instance of the data used.
- **Features** — attributes associated with an item, often represented as a vector (e.g. word counts).
- **Labels** — the category (classification) or real value (regression) associated with an item.

> [!info] Types of Data
> 
> - **Training data** — typically labeled.
> - **Test data** — labeled, but labels not seen.
> - **Validation data** — labeled, used for tuning parameters.

---

## General Learning Scenarios

> [!example] Settings
> 
> - **Batch** — learner receives the full training sample, then makes predictions for unseen points.
> - **Online** — learner receives one sample at a time and makes a prediction for that sample.

> [!example] Queries
> 
> - **Active** — the learner can request the label of a point.
> - **Passive** — the learner receives labeled points.

### Standard Batch Scenarios

|Scenario|Labeled data|Unlabeled data|Predicts on|
|---|:-:|:-:|---|
|**Unsupervised**|—|✓|—|
|**Supervised**|✓|—|Unseen points|
|**Semi-supervised**|✓|✓|Unseen points|
|**Transduction**|✓|✓|**Seen** points|

> [!example] SPAM Detection
>  **Problem:** classify each e-mail as SPAM or non-SPAM (binary classification). 
>  **Potential data:** a large collection of labeled SPAM and non-SPAM messages.

### Learning Stages
![[1 - Introduction to Machine Learning-1780864573085.webp]]

---

## Definitions (Formal Set-Up)

> [!definition] Spaces & Functions
> 
> - **Input space** $\mathcal{X}$, **output space** $\mathcal{Y}$.
> - **Loss function** $L : \mathcal{Y} \times \mathcal{Y} \to \mathbb{R}$.
>     - $L(\hat{y}, y)$ = cost of predicting $\hat{y}$ instead of $y$.
>     - **Binary classification (0-1 loss):** $L(y, y') = \mathbb{1}_{y \neq y'}$.
>     - **Regression:** $L(y, y') = (y' - y)^2$, with $\mathcal{Y} \subseteq \mathbb{R}$.

> [!definition]
>  Hypothesis Set $\mathcal{H} \subseteq \mathcal{Y}^{\mathcal{X}}$ — the subset of functions out of which the learner selects its hypothesis.
> 
> - Depends on the features.
> - Represents **prior knowledge** about the task.

### Supervised Learning Set-Up
- **Training data:** sample $S$ of size $m$ drawn i.i.d. from $\mathcal{X} \times \mathcal{Y}$ according to distribution $\mathcal{D}$: $$S = \big((x_1, y_1), \dots, (x_m, y_m)\big).$$
- **Problem:** find a hypothesis $h \in \mathcal{H}$ with small generalization error.
    - **Deterministic case:** output label is a deterministic function of input, $y = f(x)$.
    - **Stochastic case:** output is a probabilistic function of input.

---

## Errors

> [!definition] Generalization Error 
> For $h \in \mathcal{H}$: $$R(h) = \mathbb{E}_{(x,y) \sim \mathcal{D}}\big[L(h(x), y)\big].$$

> [!definition] Empirical Error 
> For $h \in \mathcal{H}$ and sample $S$: $$\widehat{R}(h) = \frac{1}{m} \sum_{i=1}^{m} L(h(x_i), y_i).$$

> [!definition] Bayes Error
>  $$R^\star = \inf_{\substack{h \ h \text{ measurable}}} R(h).$$ In the deterministic case, $R^\star = 0$.

### Noise

In binary classification, for any $x \in \mathcal{X}$: $$\text{noise}(x) = \min\{\Pr[1 \mid x],\ \Pr[0 \mid x]\}.$$ Observe that: $$\mathbb{E}[\text{noise}(x)] = R^\star.$$

---

## Learning ≠ Fitting
![[1 - Introduction to Machine Learning-1780864721338.webp|537]]
> [!warning] Key Distinction 
> Learning is **not** fitting. This raises the central notion of **simplicity / complexity**.
> _How do we define complexity?_

### Generalization

- The best hypothesis on the sample may **not** be the best overall.
- **Generalization is not memorization.**
- Complex rules (very complex separation surfaces) can be poor predictors.
- **Trade-off:** complexity of the hypothesis set vs. sample size → _underfitting / overfitting_.

---

## Model Selection

> [!important] General Equality 
> For any best-in-class $h \in \mathcal{H}$: $$R(h) - R^\star = \underbrace{[R(h) - R(h^\star)]}_{\text{estimation}} + \underbrace{[R(h^\star) - R^\star]}_{\text{approximation}}.$$

- **Approximation** — not a random variable; only depends on $\mathcal{H}$.
- **Estimation** — the only term we can hope to bound.

### Empirical Risk Minimization (ERM)
Select a hypothesis set $\mathcal{H}$, then find the hypothesis minimizing empirical error: $$h = \arg\min_{h \in \mathcal{H}} \widehat{R}(h).$$
> [!caution] Limitations
> 
> - $\mathcal{H}$ may be too complex.
> - The sample size $m$ may not be large enough.

### Generalization Bounds

> [!definition] Generalization Bound 
> An upper bound on: $$\Pr\left[\sup_{h \in \mathcal{H}} \big|R(h) - \widehat{R}(h)\big| > \epsilon\right].$$

Bound on the estimation error for the hypothesis $h_0$ given by ERM: $$ \begin{aligned} R(h_0) - R(h^\star) &= R(h_0) - \widehat{R}(h_0) + \widehat{R}(h_0) - R(h^\star) \\ &\leq R(h_0) - \widehat{R}(h_0) + \widehat{R}(h^\star) - R(h^\star) \\ &\leq 2 \sup_{h \in \mathcal{H}} \big|R(h) - \widehat{R}(h)\big|. \end{aligned} $$

> [!question] How should we choose $\mathcal{H}$?
> ![[1 - Introduction to Machine Learning-1780864846027.webp|416x315]]
>  This is the **model selection problem**. As $\mathcal{H}$ grows, estimation error rises while approximation error falls — the upper bound has a minimum at some $\mathcal{H}^\star$, where $\mathcal{H} = \bigcup_\gamma \mathcal{H}_\gamma$.

### Structural Risk Minimization (SRM)

> [!cite] Consider an **infinite sequence of hypothesis sets ordered by inclusion**:
>  $$\mathcal{H}_1 \subset \mathcal{H}_2 \subset \cdots \subset \mathcal{H}_n \subset \cdots$$ $$h = \arg\min_{h \in \mathcal{H}_n, n \in \mathbb{N}} \widehat{R}(h) + \text{penalty}(\mathcal{H}_n, m).$$
> 
> - **Strong** theoretical guarantees.
> - Typically **computationally hard**.

### General Algorithm Families
- **ERM**
$$h = \arg\min_{h \in \mathcal{H}} \widehat{R}(h)$$
- **SRM** ($\mathcal{H}_n \subseteq \mathcal{H}_{n+1}$)
$$h = \arg\min_{h \in \mathcal{H}_n,, n \in \mathbb{N}} \widehat{R}(h) + \text{penalty}(\mathcal{H}_n, m)$$
- **Regularization-based**
$$h = \arg\min_{h \in \mathcal{H}} \widehat{R}(h) + \lambda |h|^2$$


---

## Probability Tools

### Basic Properties

> [!formula] Core Identities
> 
> - **Union bound:** $\Pr[A \lor B] \leq \Pr[A] + \Pr[B].$
> - **Inversion:** if $\Pr[X \geq \epsilon] \leq f(\epsilon)$ with $f(\epsilon) > 0$, then for any $\delta$, with probability at least $1 - \delta$, $X \leq f^{-1}(\delta)$.
> - **Jensen's inequality:** if $f$ is convex, $f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)].$
> - **Expectation:** if $X \geq 0$, $\mathbb{E}[X] = \int_0^{+\infty} \Pr[X > t], dt.$

### Basic Inequalities

> [!formula] Markov's Inequality 
> If $X \geq 0$ and $\epsilon > 0$: $$\Pr[X \geq \epsilon] \leq \frac{\mathbb{E}[X]}{\epsilon}.$$

> [!formula] Chebyshev's Inequality 
> For any $\epsilon > 0$: $$\Pr\big[|X - \mathbb{E}[X]| \geq \epsilon\big] \leq \frac{\sigma_X^2}{\epsilon^2}.$$

### Hoeffding's Inequality

> [!theorem] Hoeffding's Inequality 
> Let $X_1, \dots, X_m$ be independent random variables with the same expectation $\mu$ and $X_i \in [a, b]$ ($a < b$). Then for any $\epsilon > 0$: $$\Pr\left[\mu - \frac{1}{m}\sum_{i=1}^m X_i > \epsilon\right] \leq \exp\left(-\frac{2m\epsilon^2}{(b-a)^2}\right)$$ $$\Pr\left[\frac{1}{m}\sum_{i=1}^m X_i - \mu > \epsilon\right] \leq \exp\left(-\frac{2m\epsilon^2}{(b-a)^2}\right).$$

### McDiarmid's Inequality

> [!theorem] McDiarmid's Inequality
> Let $X_1, \dots, X_m$ be independent random variables taking values in $\mathcal{U}$, and $f : \mathcal{U}^m \to \mathbb{R}$ a function verifying, for all $i \in [1, m]$: $$\sup_{x_1,\dots,x_m,,x_i'} \big|f(x_1,\dots,x_i,\dots,x_m) - f(x_1,\dots,x_i',\dots,x_m)\big| \leq c_i.$$ Then for all $\epsilon > 0$: $$\Pr\Big[\big|f(X_1,\dots,X_m) - \mathbb{E}[f(X_1,\dots,X_m)]\big| > \epsilon\Big] \leq 2\exp\left(-\frac{2\epsilon^2}{\sum_{i=1}^m c_i^2}\right).$$

---

## Appendix — Proofs

### Markov's Inequality

> [!theorem] Statement 
>Let $X$ be a non-negative random variable with $\mathbb{E}[X] < \infty$. Then for all $t > 0$: $$\Pr[X \geq t,\mathbb{E}[X]] \leq \frac{1}{t}.$$

> [!success] Proof
>  $$ \begin{aligned} \Pr[X \geq t \mathbb{E}[X]] &= \sum_{x \geq t\mathbb{E}[X]} \Pr[X = x] \\ &\leq \sum_{x \geq t\mathbb{E}[X]} \Pr[X = x]\frac{x}{t \mathbb{E}[X]} \\ &\leq \sum_{x} \Pr[X = x] \frac{x}{t \mathbb{E}[X]} \\ &= \mathbb{E}\left[\frac{X}{t \mathbb{E}[X]}\right] = \frac{1}{t}. \end{aligned} $$

### Chebyshev's Inequality

> [!theorem] Statement
>  Let $X$ be a random variable with $\text{Var}[X] < \infty$. Then for all $t > 0$: $$\Pr\big[|X - \mathbb{E}[X]| \geq t,\sigma_X\big] \leq \frac{1}{t^2}.$$

> [!success] Proof 
> Observe that: $$\Pr\big[|X - \mathbb{E}[X]| \geq t\sigma_X\big] = \Pr\big[(X - \mathbb{E}[X])^2 \geq t^2 \sigma_X^2\big].$$ The result follows from Markov's inequality.

### Weak Law of Large Numbers

> [!theorem] Statement
> Let $(X_n)_{n \in \mathbb{N}}$ be a sequence of independent random variables with the same mean $\mu$ and variance $\sigma^2 < \infty$, and let $\overline{X}_n = \frac{1}{n}\sum_{i=1}^n X_i$. Then for any $\epsilon > 0$: $$\lim_{n \to \infty} \Pr\big[|\overline{X}_n - \mu| \geq \epsilon\big] = 0.$$

> [!success] Proof
> Since the variables are independent: $$\text{Var}[\overline{X}_n] = \sum_{i=1}^n \text{Var}\left[\frac{X_i}{n}\right] = \frac{n\sigma^2}{n^2} = \frac{\sigma^2}{n}.$$ Thus, by Chebyshev's inequality: $$\Pr\big[|\overline{X}_n - \mu| \geq \epsilon\big] \leq \frac{\sigma^2}{n\epsilon^2} \xrightarrow{n \to \infty} 0.$$

### Concentration Inequalities — Overview

General tools for error analysis and bounds:
- **Hoeffding's inequality** — additive.
- **Chernoff bounds** — multiplicative.
- **McDiarmid's inequality** — more general.

### Hoeffding's Lemma

> [!theorem] Statement
> Let $X \in [a, b]$ be a random variable with $\mathbb{E}[X] = 0$ and $b \neq a$. Then for any $t > 0$: $$\mathbb{E}[e^{tX}] \leq e^{\frac{t^2(b-a)^2}{8}}.$$

> [!success] Proof 
> By convexity of $x \mapsto e^{tx}$, for all $a \leq x \leq b$: $$e^{tx} \leq \frac{b-x}{b-a}e^{ta} + \frac{x-a}{b-a}e^{tb}.$$ Thus, taking expectations (using $\mathbb{E}[X] = 0$): $$\mathbb{E}[e^{tX}] \leq \frac{b}{b-a}e^{ta} + \frac{-a}{b-a}e^{tb} = e^{\phi(t)},$$ with: $$\phi(t) = \log!\left(\tfrac{b}{b-a}e^{ta} + \tfrac{-a}{b-a}e^{tb}\right) = ta + \log!\left(\tfrac{b}{b-a} + \tfrac{-a}{b-a}e^{t(b-a)}\right).$$
> 
> Taking the derivative: $$\phi'(t) = a - \frac{a,e^{t(b-a)}}{\tfrac{b}{b-a} - \tfrac{a}{b-a}e^{t(b-a)}}.$$ Note that $\phi(0) = 0$ and $\phi'(0) = 0$. Furthermore, with $\alpha = \tfrac{a}{b-a}$: $$\phi''(t) = u(1-u)(b-a)^2 \leq \frac{(b-a)^2}{4},$$ where $u = \tfrac{(1-\alpha)e^{t(b-a)}}{(1-\alpha)e^{t(b-a)} + \alpha}$. By Taylor's theorem there exists $0 \leq \theta \leq t$ such that: $$\phi(t) = \phi(0) + t,\phi'(0) + \frac{t^2}{2}\phi''(\theta) \leq \frac{t^2(b-a)^2}{8}.$$

### Hoeffding's Theorem

> [!theorem] Statement
> Let $X_1, \dots, X_m$ be independent random variables with $X_i \in [a_i, b_i]$. Let $S_m = \sum_{i=1}^m X_i$. Then for any $\epsilon > 0$: $$\Pr[S_m - \mathbb{E}[S_m] \geq \epsilon] \leq e^{-2\epsilon^2 / \sum_{i=1}^m (b_i - a_i)^2}$$ $$\Pr[S_m - \mathbb{E}[S_m] \leq -\epsilon] \leq e^{-2\epsilon^2 / \sum_{i=1}^m (b_i - a_i)^2}.$$

> [!success] Proof
>  Based on **Chernoff's bounding technique**: for any random variable $X$ and $t > 0$, apply Markov's inequality and select $t$ to minimize: $$\Pr[X \geq \epsilon] = \Pr[e^{tX} \geq e^{t\epsilon}] \leq \frac{\mathbb{E}[e^{tX}]}{e^{t\epsilon}}.$$ Using this scheme and independence: $$ \begin{aligned} \Pr[S_m - \mathbb{E}[S_m] \geq \epsilon] &\leq e^{-t\epsilon} \mathbb{E}\big[e^{t(S_m - \mathbb{E}[S_m])}\big] \\ &= e^{-t\epsilon} \prod_{i=1}^m \mathbb{E}\big[e^{t(X_i - \mathbb{E}[X_i])}\big] \\ &\leq e^{-t\epsilon} \prod_{i=1}^m e^{t^2 (b_i - a_i)^2 / 8} \quad \text{(Hoeffding's lemma)} \\ &= e^{-t\epsilon} e^{t^2 \sum_{i=1}^m (b_i - a_i)^2 / 8} \\ &\leq e^{-2\epsilon^2 / \sum_{i=1}^m (b_i - a_i)^2}, \end{aligned} $$ choosing $t = 4\epsilon / \sum_{i=1}^m (b_i - a_i)^2$. The second inequality is proved similarly.

### Hoeffding's Inequality (Corollary)

> [!theorem] Corollary
> For any $\epsilon > 0$, any distribution $\mathcal{D}$, and any hypothesis $h : \mathcal{X} \to {0, 1}$: $$\Pr[\widehat{R}(h) - R(h) \geq \epsilon] \leq e^{-2m\epsilon^2}$$ $$\Pr[\widehat{R}(h) - R(h) \leq -\epsilon] \leq e^{-2m\epsilon^2}.$$ Combining these one-sided inequalities: $$\Pr\big[|\widehat{R}(h) - R(h)| \geq \epsilon\big] \leq 2e^{-2m\epsilon^2}.$$

### Chernoff's Inequality

> [!theorem] Statement
> For any $\epsilon > 0$, any distribution $\mathcal{D}$, and any hypothesis $h : \mathcal{X} \to {0, 1}$: $$\Pr[\widehat{R}(h) \geq (1+\epsilon)R(h)] \leq e^{-m R(h)\epsilon^2 / 3}$$ $$\Pr[\widehat{R}(h) \leq (1-\epsilon)R(h)] \leq e^{-m R(h)\epsilon^2 / 2}.$$
> 
> _Proof based on Chernoff's bounding technique._

### McDiarmid → Hoeffding Connection

> [!note] Comments
> 
> - **Proof** of McDiarmid uses Hoeffding's lemma.
> - **Hoeffding's inequality is a special case of McDiarmid's** with: $$f(x_1, \dots, x_m) = \frac{1}{m}\sum_{i=1}^m x_i \quad \text{and} \quad c_i = \frac{|b_i - a_i|}{m}.$$

### Jensen's Inequality

> [!theorem] Statement
>  Let $X$ be a random variable and $f$ a measurable convex function. Then: $$f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)].$$
> 
> _Proof: definition of convexity, continuity of convex functions, and density of finite distributions._

For a convex $f$ and $t \in [0,1]$: $f(tx + (1-t)y) \leq t f(x) + (1-t)f(y)$ — the chord lies above the curve, which generalizes to expectations.




> [!abstract]
>  Purpose Introduce common concepts and themes in statistical learning:
> 
> - **Supervised learning**
> - **Unsupervised learning**
> - **Assessing predictive performance** of supervised learning

---

## What is Statistical Learning?

A main challenge in data science is the **mathematical analysis of data**. When the goal is to _interpret the model_ and _quantify the uncertainty_ in the data, this analysis is called **statistical learning**.

Two major goals for modeling data:
1. **Prediction** — accurately predict some future quantity of interest, given observed data.
2. **Pattern discovery** — discover unusual or interesting patterns in the data.

---

## Core Setup

Given an input / **feature vector** $\boldsymbol{x}$, a main goal of machine learning is to predict an output / **response variable** $y$.

> [!example] Example 1
> 
> - $\boldsymbol{x}$ = a digitized signature 
> - $y$ = binary variable (genuine vs. false).

> [!example] Example 2
> - $\boldsymbol{x}$ = weight and smoking habits of an expecting mother 
> - $y$ = birth weight of the baby.

This is formalized via a **prediction function** $g$, which takes input $\boldsymbol{x}$ and outputs a guess $g(\boldsymbol{x})$ for $y$ (denoted $\widehat{y}$).

> [!note] Intuition
>  In a sense, $g$ encompasses all information about the relationship between $\boldsymbol{x}$ and $y$, **excluding** the effects of chance and randomness in nature.

---

## Regression vs. Classification

|Type|Response $y$|Goal|
|---|---|---|
|**Regression**|any real value|predict a continuous quantity|
|**Classification**|finite set $y \in {0, \dots, c-1}$|assign $\boldsymbol{x}$ to one of $c$ categories|

When $y$ can only lie in a finite set, predicting $y$ is conceptually the same as **classifying** input $\boldsymbol{x}$ into one of $c$ categories.

---

## Loss

We measure accuracy of a prediction $\widehat{y}$ against a response $y$ via a **loss function** $\mathrm{Loss}(y, \widehat{y})$.

Common loss functions
 - **Squared-error loss** (regression): $$ \mathrm{Loss}(y,\widehat{y}) = (y - \widehat{y})^2 $$
 - **Zero–one (0–1) loss** (classification): $$ \mathrm{Loss}(y,\widehat{y}) = \mathbb{I}{y \neq \widehat{y}} $$incurs a loss of 1 whenever the predicted class $\widehat{y} \neq y$.
 - Other useful losses encountered later: **cross-entropy** and **hinge** loss.

### Error

The word **error** measures distance between a "true" object $y$ and an approximation $\widehat{y}$.

- **Real-valued:** absolute error $|y - \widehat{y}|$ and squared error $(y - \widehat{y})^2$.
- **Vectors:** norm $|\boldsymbol{y} - \widehat{\boldsymbol{y}}|$ and squared norm $|\boldsymbol{y} - \widehat{\boldsymbol{y}}|^2$.

The squared error is just **one example** of a loss function.

---

## Risk

It is unlikely that any function $g$ predicts accurately for all pairs $(\boldsymbol{x}, y)$ — even with the same input $\boldsymbol{x}$, the output $y$ may differ due to randomness.

We adopt a **probabilistic approach**: each pair $(\boldsymbol{x}, y)$ is the outcome of a random pair $(\boldsymbol{X}, Y)$ with joint pdf $f(\boldsymbol{x}, y)$.

Predictive performance is assessed via the **expected loss**, called the **risk**:

$$ \ell(g) = \mathbb{E} \mathrm{Loss}(Y, g(\boldsymbol{X})). \tag{1} $$

---

## Optimal Prediction Function

The goal is to find the prediction function with smallest risk:

$$ g^* = \underset{g}{\arg\min}\ \underbrace{\mathbb{E}\mathrm{Loss}(Y, g(\boldsymbol{X}))}_{\ell(g)}. $$
In the classification case with zero-one loss, the risk equals the probability of misclassification: $\ell(g) = \mathbb{P}[Y \neq g(\boldsymbol{X})]$. 
Here $g$ is called a **classifier**. $$ g^*(\boldsymbol{x}) = \underset{y \in {0,\dots,c-1}}{\arg\max}\ f(y \mid \boldsymbol{x}), $$ where $f(y \mid \boldsymbol{x}) = \mathbb{P}[Y = y \mid \boldsymbol{X} = \boldsymbol{x}]$.

> [!important] Theorem — Optimal Prediction Function (regression) 
> For squared-error loss $\mathrm{Loss}(y,\widehat{y}) = (y - \widehat{y})^2$, the optimal prediction function $g^*$ is the **conditional expectation** of $Y$ given $\boldsymbol{X} = \boldsymbol{x}$: $$ g^*(\boldsymbol{x}) = \mathbb{E}[Y \mid \boldsymbol{X} = \boldsymbol{x}]. $$

### Proof:
Let $g^*(\boldsymbol{x}) = \mathbb{E}[Y \mid \boldsymbol{X} = \boldsymbol{x}]$. For any function $g$: $$ \begin{aligned} \mathbb{E}(Y - g(\boldsymbol{X}))^2 &= \mathbb{E}[(Y - g^*(\boldsymbol{X}) + g^*(\boldsymbol{X}) - g(\boldsymbol{X}))^2] \\ &= \mathbb{E}(Y - g^*(\boldsymbol{X}))^2 + 2,\mathbb{E}[(Y - g^*(\boldsymbol{X}))(g^*(\boldsymbol{X}) - g(\boldsymbol{X}))] + \mathbb{E}(g^*(\boldsymbol{X}) - g(\boldsymbol{X}))^2 \\ &\geq \mathbb{E}(Y - g^*(\boldsymbol{X}))^2 + 2,\mathbb{E}[(Y - g^*(\boldsymbol{X}))(g^*(\boldsymbol{X}) - g(\boldsymbol{X}))] \ &= \mathbb{E}(Y - g^*(\boldsymbol{X}))^2 + 2,\mathbb{E}\big\{(g^*(\boldsymbol{X}) - g(\boldsymbol{X})),\mathbb{E}[Y - g^*(\boldsymbol{X}) \mid \boldsymbol{X}]\big\}. \end{aligned} $$ Using the **tower property** and the definition of conditional expectation, $\mathbb{E}[Y - g^*(\boldsymbol{X}) \mid \boldsymbol{X}] = 0$. Hence $\mathbb{E}(Y - g(\boldsymbol{X}))^2 \geq \mathbb{E}(Y - g^*(\boldsymbol{X}))^2$, so $g^*$ yields the smallest squared-error risk. $\blacksquare$

---

## Training Set
The catch $g^*$ depends on the (typically **unknown**) joint distribution of $(\boldsymbol{X}, Y)$, so it is **not available in practice**.

In practice, we only have a finite number of (usually) independent realizations from $f(\boldsymbol{x}, y)$.

- **Random** training set: $\mathcal{T} := {(\boldsymbol{X}_1, Y_1), \dots, (\boldsymbol{X}_n, Y_n)}$ with $n$ examples ($\mathcal{T}$ = mnemonic for **t**raining).
- **Deterministic** outcome: $\tau := {(\boldsymbol{x}_1, y_1), \dots, (\boldsymbol{x}_n, y_n)}$.

**N.B.:** distinguish carefully between the random $\mathcal{T}$ and its outcome $\tau$.

---

## Supervised Learning

We try to learn the functional relationship $g^*$ between feature vector $\boldsymbol{x}$ and response $y$, in the presence of a **teacher** who provides the training set $\mathcal{T}$.

- $g_\mathcal{T}$ = the **learner**: best approximation for $g^*$ constructible from $\mathcal{T}$. It is a **random function**; a particular outcome is $g_\tau$.
- Main methodologies: **regression** and **classification**.
- Advanced techniques: reproducing kernel Hilbert spaces, tree methods, deep learning.

---

## Unsupervised Learning

No distinction between response and explanatory variables — the objective is to learn the **structure** of the unknown data distribution, i.e. learn $f(\boldsymbol{x})$.

The guess $g(\boldsymbol{x})$ approximates $f(\boldsymbol{x})$, with risk: $$ \ell(g) = \mathbb{E} \mathrm{Loss}(f(\boldsymbol{X}), g(\boldsymbol{X})). $$
Main methodologies: **clustering**, **principal component analysis (PCA)**, **kernel density estimation**.

---

## Training Loss

Typically the risk $\ell(g) = \mathbb{E},\mathrm{Loss}(Y, g(\boldsymbol{X}))$ cannot be computed. Using the training sample, we approximate it via the **empirical (sample-average) risk**, called the **training loss**:

$$ \ell_\mathcal{T}(g) = \frac{1}{n} \sum_{i=1}^{n} \mathrm{Loss}(Y_i, g(\boldsymbol{X}_i)). \tag{2} $$

This is an **unbiased estimator** of the risk for a fixed prediction function $g$, based on the training data.

---

## The Learner

To approximate $g^*$ (minimizer of the risk):

1. Select a suitable collection of approximating functions $\mathcal{G}$.
2. Take the learner to be the function in $\mathcal{G}$ minimizing the training loss: $$ g_\mathcal{T}^{\mathcal{G}} = \underset{g \in \mathcal{G}}{\arg\min}\ \ell_\mathcal{T}(g). \tag{3} $$

> [!example] Simplest useful class
> The set of **linear functions** of $\boldsymbol{x}$: all $g : \boldsymbol{x} \mapsto \boldsymbol{\beta}^\top \boldsymbol{x}$ for a real-valued vector $\boldsymbol{\beta}$.

---

## Overfitting

> [!danger] Why not minimize over _all_ functions? 
> Minimizing training loss over **all** possible functions is not meaningful: any function with $g(\boldsymbol{X}_i) = Y_i$ for all $i$ gives minimal training loss (for squared-error loss, exactly **0**). Such functions predict new, independent data **poorly** — this is **overfitting**.

By choosing $g$ as a function that predicts the training data exactly (and is, e.g., 0 otherwise), the squared-error training loss is zero. **Minimizing the training loss is not the ultimate goal!**

---

## Generalization Risk

Prediction accuracy on _new_ data is measured by the **generalization risk** of the learner. For a **fixed** training set $\tau$:

$$ \ell(g_\tau^{\mathcal{G}}) = \mathbb{E} \mathrm{Loss}(Y, g_\tau^{\mathcal{G}}(\boldsymbol{X})), \tag{4} $$

where $(\boldsymbol{X}, Y) \sim f(\boldsymbol{x}, y)$.
![[1 - Statistical Learning-1780679966854.webp]]
**Figure:** The generalization risk is the weighted-average loss over all possible pairs $(\boldsymbol{x}, y)$.
### Expected Generalization Risk
For a **random** $\mathcal{T}$, the generalization risk is itself a random variable. Averaging over all instances of $\mathcal{T}$:

$$ \mathbb{E}\ell(g_\mathcal{T}^{\mathcal{G}}) = \mathbb{E} \mathrm{Loss}(Y, g_\mathcal{T}^{\mathcal{G}}(\boldsymbol{X})), \tag{5} $$

where $(\boldsymbol{X}, Y)$ is **independent of** $\mathcal{T}$. 
![[1 - Statistical Learning-1780680104803.webp]]
The expected generalization risk is the weighted-average loss over all pairs $(\boldsymbol{x}, y)$ **and** over all training sets.

---

## Test Loss

For any training outcome $\tau$, we estimate the generalization risk **without bias** using a separate **test sample**:

$$ \ell_{\tau'}(g_\tau^{\mathcal{G}}) := \frac{1}{n'} \sum_{i=1}^{n'} \mathrm{Loss}(Y_i', g_\tau^{\mathcal{G}}(\boldsymbol{X}_i')). \tag{6} $$
Where:
-  $\mathcal{T}' = {(\boldsymbol{X}_1', Y_1'), \dots, (\boldsymbol{X}_{n'}', Y_{n'}')}$ is called **test sample**.
 The test sample is drawn the **same way** as $\mathcal{T}$ (independent draws from $f(\boldsymbol{x}, y)$), but is **completely separate**. It is **crucial** that $\mathcal{T}$ is independent of $\mathcal{T}'$.

### Test & Validation Sets
To compare the predictive performance of different learners we can use the **same** fixed $\tau$ and $\tau'$ to compare different learners:
- With abundant data, split into **training** + **test** sets.
- If the test set is also used for **model selection**, a **third (validation)** set is needed:
![[1 - Statistical Learning-1780685652932.webp]]

---

## Three Pillars for Finding Good Learners

1. **Function approximation** — represent the (unknown) relationship between variables; approximate well given enough computing power and data.
2. **Optimization** — efficiently search for the best function within a class.
3. **Probability & Statistics** — data is a realization of a random process whose probability law determines prediction accuracy.

---

## Notation Summary

| Symbol                                | Meaning                                                               |
| ------------------------------------- | --------------------------------------------------------------------- |
| $\boldsymbol{x}$ and $\boldsymbol{X}$ | Fixed and random explanatory (feature) vector                         |
| $y$ and $Y$                           | Fixed and random response                                             |
| $f(\boldsymbol{x}, y)$                | Joint pdf of $\boldsymbol{X}$ and $Y$                                 |
| $\tau$ and $\mathcal{T}$              | Fixed and random training data ${(\boldsymbol{X}_i, Y_i)}$            |
| $\mathbf{X}$                          | Model matrix of explanatory variables, rows ${\boldsymbol{x}_i^\top}$ |
| $g$                                   | Prediction (guess) function                                           |
| $\mathrm{Loss}(y, \widehat{y})$       | Loss when predicting $y$ with $\widehat{y}$                           |
| $\ell(g)$                             | Risk for $g$: $\mathbb{E},\mathrm{Loss}(Y, g(\boldsymbol{X}))$        |
| $g^*$                                 | Optimal prediction function: $\arg\min_g \ell(g)$                     |
| $\ell_\tau(g)$                        | Training loss: sample-average estimate of $\ell(g)$ based on $\tau$   |
| $g_\tau^{\mathcal{G}}$                | The learner: $\arg\min_{g \in \mathcal{G}} \ell_\tau(g)$              |

---

## Worked Example — Polynomial Regression

> [!example] Data-generating model 
> $(U_i, Y_i)$, $i = 1, \dots, 100$, where ${U_i} \sim_{\text{iid}} \mathcal{U}(0, 1)$ and, given $U_i = u_i$: $$ Y_i \sim \mathcal{N}(10 - 140u_i + 400u_i^2 - 250u_i^3,\ 25). $$
> ![[1 - Statistical Learning-1780685734999.webp|563x355]]

Under squared-error loss, the optimal prediction function $h^*(u) = \mathbb{E}[Y \mid U = u]$ is the **true cubic**: $$ h^*(u) = 10 - 140u + 400u^2 - 250u^3. $$
To estimate $h^*(u)$ from $\tau = \{(u_i, y_i), i= 1, ..., n\}$, minimize the squared-error training loss over a candidate set $\mathcal{H}$: $$ \ell_\tau(h) = \frac{1}{n} \sum_{i=1}^{n} (y_i - h(u_i))^2. $$
### Polynomial Function Class

Class $\mathcal{H}_p$ of polynomials of order $p - 1$: $$ h(u) := \beta_1 + \beta_2 u + \beta_3 u^2 + \cdots + \beta_p u^{p-1}, \tag{7} $$ with parameter vector $\boldsymbol{\beta} = [\beta_1, \dots, \beta_p]^\top$. This is a **parametric** optimization problem (find best $\boldsymbol{\beta}$).

> [!tip] Key idea
>  Expand the feature space to obtain a _linear_ prediction function Map each feature $u$ to $\boldsymbol{x} = [1, u, u^2, \dots, u^{p-1}]^\top$. Then $$ g(\boldsymbol{x}) = \boldsymbol{x}^\top \boldsymbol{\beta}, $$ which is **linear** in $\boldsymbol{x}$ (and in $\boldsymbol{\beta}$). So we work with the linear class $\mathcal{G}_p$ instead of $\mathcal{H}_p$.

### Linear Prediction Function
Instead of working with the set  of $\mathcal{H}_p$ polynomial functions we may prefer to work with the set $\mathcal{G}$ of linear functions.
Let us now reformulate the learning problem in terms of the new explanatory (feature) **variables** $\boldsymbol{x}_1 = [1, u_i, u^2_i, \dots, u_i^{p-1}]^\top,i= 1,...,n$,.  It will be convenient to arrange these feature vectors into  a matrix $X$ with rows $x_1^\top, ..., x_n^\top$ (the so-called **model matrix**):
$$ \mathbf{X} = \begin{bmatrix} 1 & u_1 & u_1^2 & \cdots & u_1^{p-1} \\ 1 & u_2 & u_2^2 & \cdots & u_2^{p-1} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & u_n & u_n^2 & \cdots & u_n^{p-1} \end{bmatrix}. $$

---

## Least-Squares Solution
Collecting responses $\{\boldsymbol{y_i}\}$ into $\boldsymbol{y}$ a column vector, the training loss is: $$ \frac{1}{n} |\boldsymbol{y} - \mathbf{X}\boldsymbol{\beta}|^2. \tag{8} $$

The **ordinary least-squares (OLS)** solution minimizes it: $$ \widehat{\boldsymbol{\beta}} = \underset{\boldsymbol{\beta}}{\arg\min}\ |\boldsymbol{y} - \mathbf{X}\boldsymbol{\beta}|^2. $$

### Projection Matrix
![[1 - Statistical Learning-1780686860901.webp]]
Geometrically, $\mathbf{X}\widehat{\boldsymbol{\beta}}$ is the **orthogonal projection** of $\boldsymbol{y}$ onto $\mathrm{Span}(\mathbf{X})$ (the space spanned by the columns of $\mathbf{X}$): $$ \mathbf{X}\widehat{\boldsymbol{\beta}} = \mathbf{P}\boldsymbol{y}, $$ where $\mathbf{P}$ is the **projection matrix**.

### Pseudo-Inverse
The projection matrix is given by
$$ \mathbf{P} = \mathbf{X}\mathbf{X}^+, $$ where the $p \times n$ matrix $\mathbf{X}^+$ is the **pseudo-inverse** of $\mathbf{X}$.

> [!note] Full column rank case 
> If $\mathbf{X}$ has full column rank (no column is a linear combination of the others): $$ \mathbf{X}^+ = (\mathbf{X}^\top \mathbf{X})^{-1} \mathbf{X}^\top. $$


---
## Model Selection
![[1 - Statistical Learning-1780686943592.webp|675]]
Fitting at different $p$:

|$p$|Fit quality|
|---|---|
|$p = 2$|**underfit** (straight line)|
|$p = 4$|**correct** (true cubic)|
|$p = 16$|**overfit** (close to data, far from true curve)|

At $p = 16$ the curve hugs the data points but strays from the true polynomial → overfitting. 
The true $p = 4$ is much better than either extreme.

Each function class $\mathcal{G}_p$ gives a different learner $g_\tau^{\mathcal{G}_p}$. To assess which is best, use the **test loss** on a test set. With test feature matrix $\mathbf{X}'$ and test responses $\boldsymbol{y}'$:

$$ \ell_{\tau'}(g_\tau^{\mathcal{G}_p}) = \frac{1}{n'} |\boldsymbol{y}' - \mathbf{X}'\widehat{\boldsymbol{\beta}}|^2, $$

where $\widehat{\boldsymbol{\beta}}$ is found from the **training** data.

> [!summary] The "bath-tub" curve 
> The test loss (estimate of generalization risk) plotted against the number of parameters $p$ has a characteristic **bath-tub shape** — lowest at $p = 4$ (the true model). It becomes numerically unreliable beyond $p = 16$.
> ![[1 - Statistical Learning-1780687092838.webp]]

---

## Key Takeaways

- [ ] Statistical learning = modeling for **interpretation + uncertainty quantification**.
- [ ] **Risk** = expected loss; the optimal $g^*$ minimizes it but needs the unknown joint distribution.
- [ ] For squared-error loss, $g^*(\boldsymbol{x}) = \mathbb{E}[Y \mid \boldsymbol{X} = \boldsymbol{x}]$.
- [ ] **Training loss** is an unbiased estimate of risk; minimizing it alone causes **overfitting**.
- [ ] Use a **separate test set** (and a validation set for model selection) to estimate generalization risk.
- [ ] Polynomial regression → **expand the feature space** to make the problem **linear** → solve via **OLS / projection**.
- [ ] Choose model complexity $p$ by the minimum of the **bath-tub** test-loss curve.
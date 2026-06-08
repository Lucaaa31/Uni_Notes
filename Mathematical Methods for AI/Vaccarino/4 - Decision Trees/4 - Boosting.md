> [!abstract] Purpose
> **Boosting** aims to improve the accuracy of _any_ learning algorithm, especially when it involves **weak learners** — simple prediction functions that perform only slightly better than random guessing. Shallow decision trees typically yield weak learners.

---
## Core Idea
Boosting works for both **classification** and **regression**. Like Bagging, it uses an **ensemble** of prediction functions — but with a key difference:

> [!important] Sequential vs. Parallel
> In bagging, learners are trained independently. In **boosting**, prediction functions are learned **sequentially**: each learner uses information from the previous ones.

Starting from a simple model (weak learner) $g_0$ on data $\tau = \{(\boldsymbol{x}_i, y_i)\}_{i=1}^{n}$, we "boost" it:

$$g_1 := g_0 + h_1$$

where $h_1$ minimizes the training loss for $g_0 + h_1$ over all functions $h$ in some class $\mathcal{H}$.

### The Boosted Function

For example, $\mathcal{H}$ could be all prediction functions obtainable from a decision tree of maximal depth 2. Given a loss function, $h_1$ solves:

$$h_1 = \underset{h \in \mathcal{H}}{\arg\min}\ \frac{1}{n}\sum_{i=1}^{n} \text{Loss}\big(y_i, g_0(\boldsymbol{x}_i) + h(\boldsymbol{x}_i)\big)$$

Repeating this ($g_2 = g_1 + h_2$, etc.) yields the **boosted prediction function**:
$$g_B(\boldsymbol{x}) = g_0(\boldsymbol{x}) + \sum_{b=1}^{B} h_b(\boldsymbol{x})$$

---

## Regression Boosting (Squared-Error Loss)

### The Shrinkage Step

Instead of $g_b = g_{b-1} + h_b$, we prefer a **smooth updating step**:

$$g_b = g_{b-1} + \gamma, h_b$$

for a suitably chosen **step-size parameter** $\gamma$. This helps reduce overfitting.

### Setup
Using squared-error loss $\text{Loss}(y, \hat{y}) = (y - \hat{y})^2$, it is common to start with:
$$g_0(\boldsymbol{x}) = \frac{1}{n}\sum_{i=1}^{n} y_i$$

Each $h_b$ ($b = 1, \dots, B$) is a learner fitted to the **residuals** of $g_{b-1}$. The residual data set $\tau_b := {(\boldsymbol{x}_i, e_i^{(b)})}_{i=1}^{n}$ uses:

$$e_i^{(b)} := y_i - g_{b-1}(\boldsymbol{x}_i) \tag{1}$$

> [!algorithm] Algorithm 1 — Regression Boosting with Squared-Error Loss
> **Input:** Training set $\tau = {(\boldsymbol{x}_i, y_i)}_{i=1}^{n}$, number of boosting rounds $B$, shrinkage step-size $\gamma$. **Output:** Boosted prediction function.
> 
> 1. Set $g_0(\boldsymbol{x}) \leftarrow n^{-1}\sum_{i=1}^{n} y_i$
> 2. **for** $b = 1$ **to** $B$ **do**
> 3.      Set $e_i^{(b)} \leftarrow y_i - g_{b-1}(\boldsymbol{x}_i)$ for $i = 1,\dots,n$, and let $$\tau_b \leftarrow {(\boldsymbol{x}_i, e_i^{(b)})}_{i=1}^{n}$$
> 4.      Fit a prediction function $h_b$ on the training data $\tau_b$
> 5.      Set $g_b(\boldsymbol{x}) \leftarrow g_{b-1}(\boldsymbol{x}) + \gamma h_b(\boldsymbol{x})$
> 6. **return** $g_B$

### Role of the Step-Size $\gamma$
The step-size controls the **speed** of the fitting process. For small $\gamma$, boosting takes smaller steps toward minimizing the training loss.

> [!tip] Why $\gamma$ matters $\gamma$ is of great practical importance because it helps the algorithm **avoid overfitting**.
> ![[4 - Boosting-1780862940826.webp]]
> Comparing $g_{1000}$ fitted with two step sizes:
> 
> - **$\gamma = 1.0$** → severe overfitting (the curve chases every data point)
> - **$\gamma = 0.005$** → smooth fit that captures the trend

---

## Gradient Boosting

### From Residuals to Gradients

The parameter $\gamma$ can be viewed as a step in the direction of the **negative gradient** of the squared-error loss. For squared-error loss:

$$-\left.\frac{\partial, \text{Loss}(y_i, z)}{\partial z}\right|_{z = g_{b-1}(\boldsymbol{x}_i)} = -\left.\frac{\partial (y_i - z)^2}{\partial z}\right|_{z = g_{b-1}(\boldsymbol{x}_i)} = 2\big(y_i - g_{b-1}(\boldsymbol{x}_i)\big)$$

This is **two times the residual** $e_i^{(b)}$ from equation (1) used in Algorithm 1.

> [!important] Key Generalization
>  The same gradient-descent idea works for **any differentiable loss function**. The resulting algorithm is **gradient boosting**.

### The Main Idea (mimicking gradient descent)

1. Calculate a **negative gradient** on the $n$ training points $\boldsymbol{x}_1, \dots, \boldsymbol{x}_n$
2. Fit a **simple model** (e.g., a shallow decision tree) to approximate the gradient
3. Make a **$\gamma$-sized step** in the direction of the negative gradient

> [!note] Algorithm 2 — Gradient Boosting 
> **Input:** Training set $\tau = {(\boldsymbol{x}_i, y_i)}_{i=1}^{n}$, number of rounds $B$, differentiable loss $\text{Loss}(y, \hat{y})$, step-size $\gamma$. 
> **Output:** Gradient boosted prediction function.
> 
> 1. Set $g_0(\boldsymbol{x}) \leftarrow 0$
> 2. **for** $b = 1$ **to** $B$ **do**
> 3.      **for** $i = 1$ **to** $n$ **do**
> 4.          Evaluate the negative gradient: $$\quad r_i^{(b)} \leftarrow -\left.\dfrac{\partial, \text{Loss}(y_i, z)}{\partial z}\right|_{z = g_{b-1}(\boldsymbol{x}_i)}$$
> 5.      Approximate the negative gradient: $$\quad h_b = \underset{h \in \mathcal{H}}{\arg\min}\ \dfrac{1}{n}\sum_{i=1}^{n}\big(r_i^{(b)} - [g_{b-1}(\boldsymbol{x}_i) + h(\boldsymbol{x}_i)]\big)^2$$
> 6.      Set $g_b(\boldsymbol{x}) \leftarrow g_{b-1}(\boldsymbol{x}) + \gamma, h_b(\boldsymbol{x})$
> 7. **return** $g_B$

### Example — Gradient Boosting Regression Tree

Using sklearn's gradient boosting estimator with:
- $\gamma = 0.1$ (learning rate)
- $B = 100$ boosting rounds
- Weak learners: regression trees of depth $\leq 3$
```python
import numpy as np
from sklearn.datasets import make_friedman1
from sklearn.tree import DecisionTreeRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score

n_points = 1000 # create regression problem
x, y = make_friedman1(n_samples=n_points, n_features=15,
                      noise=1.0, random_state=100)

# split to train/test set
x_train, x_test, y_train, y_test = \
    train_test_split(x, y, test_size=0.33, random_state=100)

# boosting sklearn
from sklearn.ensemble import GradientBoostingRegressor

breg = GradientBoostingRegressor(learning_rate=0.1,
                                 n_estimators=100, max_depth=3, random_state=100)
breg.fit(x_train, y_train)
yhat = breg.predict(x_test)
print("Gradient Boosting R^2 score = ", r2_score(y_test, yhat))
```
> [!success] Performance Comparison ($R^2$ score)
> 
> |Model|$R^2$ Score|
> |---|---|
> |Single decision tree|0.5754|
> |Bagged tree|0.7610|
> |Random forest|0.8106|
> |**Gradient boosting**|**0.8990**|
> 
> Even though individual depth-3 trees are _weak_, the boosted ensemble clearly outperforms the others.

---

## AdaBoost (Binary Classification, $\{-1, 1\}$)
AdaBoost builds a sequence $g_0,\ g_1 = g_0 + h_1,\ g_2 = g_0 + h_1 + h_2,\ \dots$ with final function:

$$g_B(\boldsymbol{x}) = g_0(\boldsymbol{x}) + \sum_{b=1}^{B} h_b(\boldsymbol{x})$$

where each $h_b(\boldsymbol{x}) = \alpha_b c_b(\boldsymbol{x})$, with $\alpha_b \in \mathbb{R}_{+}$ and classifier $c_b(\boldsymbol{x}) \in \{-1, 1\}$ from a class $\mathcal{C}$.

### The Optimization Problem
At each iteration we solve, using the **exponential loss** $\text{Loss}(y, \hat{y}) = e^{-y\hat{y}}$:
$$(\alpha_b, c_b) = \underset{\alpha \ge 0,\ c \in \mathcal{C}}{\arg\min}\ \frac{1}{n}\sum_{i=1}^{n} \text{Loss}\big(y_i, g_{b-1}(\boldsymbol{x}_i) + \alpha c(\boldsymbol{x}_i)\big)$$

### Derivation
Starting from $g_0 := 0$, and defining the **weights** $w_i^{(b)} := \exp\{-y_i g_{b-1}(\boldsymbol{x}_i)\}$ (which depend on neither $\alpha$ nor $c$):
$$(\alpha_b, c_b) = \underset{\alpha \ge 0,\ c \in \mathcal{C}}{\arg\min}\ \sum_{i=1}^{n} \underbrace{e^{-y_i g_{b-1}(\boldsymbol{x}_i)}}_{w_i^{(b)}} e^{-y_i \alpha c(\boldsymbol{x}_i)}$$
Splitting into correctly and incorrectly classified points:
$$(\alpha_b, c_b) = \underset{\alpha \ge 0,\ c \in \mathcal{C}}{\arg\min}\ (e^{\alpha} - e^{-\alpha}) \ell_\tau^{(b)}(c) + e^{-\alpha} \tag{2}$$

where the **weighted zero–one training loss** at iteration $b$ is:
$$\ell_\tau^{(b)}(c) := \frac{\sum_{i=1}^{n} w_i^{(b)} \mathbb{I}\{c(\boldsymbol{x}_i) \neq y_i\}}{\sum_{i=1}^{n} w_i^{(b)}}$$
### Solving for the Optimal Classifier and Step-Size
For any $\alpha \ge 0$, problem (2) is minimized by the classifier minimizing the weighted zero–one loss:
$$c_b(\boldsymbol{x}) = \underset{c \in \mathcal{C}}{\arg\min}\ \ell_\tau^{(b)} \tag{3}$$
Substituting (3) into (2) and solving for the optimal $\alpha$ gives:
$$\boxed{\ \alpha_b = \frac{1}{2}\ln\left(\frac{1 - \ell_\tau^{(b)}(c_b)}{\ell_\tau^{(b)}(c_b)}\right)\ }$$
The **final classification** is delivered via:
$$\text{sign}\left(\sum_{b=1}^{B} \alpha_b c_b(\boldsymbol{x})\right)$$

> [!note] Algorithm 3 — AdaBoost 
> **Input:** Training set $\tau = {(\boldsymbol{x}_i, y_i)}_{i=1}^{n}$, number of rounds $B$. 
> **Output:** AdaBoost prediction function.
> 
> 1. Set $g_0(\boldsymbol{x}) \leftarrow 0$
> 2. **for** $i = 1$ **to** $n$ **do**
> 3.      $w_i^{(1)} \leftarrow 1/n$    _(equal initial weights)_
> 4. **for** $b = 1$ **to** $B$ **do**
> 5.      Fit classifier $c_b$ by solving $$\ c_b = \underset{c \in \mathcal{C}}{\arg\min}\ \ell_\tau^{(b)}(c) = \underset{c \in \mathcal{C}}{\arg\min}\ \dfrac{\sum_{i=1}^{n} w_i^{(b)} \mathbb{I}\{c(\boldsymbol{x}_i) \neq y_i\}}{\sum_{i=1}^{n} w_i^{(b)}}$$
> 6.      Set $\alpha_b \leftarrow \frac{1}{2}\ln\left(\dfrac{1 - \ell_\tau^{(b)}(c_b)}{\ell_\tau^{(b)}(c_b)}\right)$  _(update weights)_
> 7.      **for** $i = 1$ **to** $n$ **do**   
> 8.          $w_i^{(b+1)} \leftarrow w_i^{(b)} \exp\{-y_i \alpha_b c_b(\boldsymbol{x}_i)\}$
> 9. **return** $g_B(\boldsymbol{x}) := \sum_{b=1}^{B} \alpha_b c_b(\boldsymbol{x})$

### Intuition Behind the Weights

> [!info] How AdaBoost reweights samples
> 
> - **Step 1 ($b = 1$):** every sample gets equal weight $w_i^{(1)} = 1/n$.
> - **Step $b > 1$:** weights of **incorrectly classified** observations are **increased**; weights of **correctly classified** ones are **decreased**.
>     - Misclassified samples get extra weight → better chance of being classified correctly next round.
> - The step-size $\alpha_b$ is an **optimal step-size** in the sense of training-loss minimization.

> [!tip] Slowing down AdaBoost 
> One can reduce overfitting by fixing $\alpha_b$ to a small constant $\alpha_b = \gamma$ instead of the optimal value.

![[4 - Boosting-1780864167525.webp|532]]
## Python Implementations

> [!example]- `RegressionBoosting.py` — Boosting from scratch (squared-error)
> 
> ```python
> import numpy as np
> from sklearn.tree import DecisionTreeRegressor
> from sklearn.model_selection import train_test_split
> from sklearn.datasets import make_regression
> import matplotlib.pyplot as plt
> 
> def TrainBoost(alpha, BoostingRounds, x, y):
>     g_0 = np.mean(y)
>     residuals = y - alpha * g_0
>     # list of basic regressors
>     g_boost = []
>     for i in range(BoostingRounds):
>         h_i = DecisionTreeRegressor(max_depth=1)
>         h_i.fit(x, residuals)
>         residuals = residuals - alpha * h_i.predict(x)
>         g_boost.append(h_i)
>     return g_0, g_boost
> 
> def Predict(g_0, g_boost, alpha, x):
>     yhat = alpha * g_0 * np.ones(len(x))
>     for j in range(len(g_boost)):
>         yhat = yhat + alpha * g_boost[j].predict(x)
>     return yhat
> 
> np.random.seed(1)
> sz = 30
> x, y = make_regression(n_samples=sz, n_features=1,
>                        n_informative=1, noise=10.0)  # create data set
> 
> BoostingRounds = 1000
> alphas = [1, 0.005]
> for alpha in alphas:  # boosting algorithm
>     g_0, g_boost = TrainBoost(alpha, BoostingRounds, x, y)
>     yhat = Predict(g_0, g_boost, alpha, x)
>     tmpX = np.reshape(np.linspace(-2.5, 2, 1000), (1000, 1))
>     yhatX = Predict(g_0, g_boost, alpha, tmpX)
>     f = plt.figure()
>     plt.plot(x, y, '*')
>     plt.plot(tmpX, yhatX)
>     plt.show()  # plot
> ```

> [!example]- `GradientBoostingRegression.py` — sklearn gradient boosting
> 
> ```python
> import numpy as np
> from sklearn.datasets import make_friedman1
> from sklearn.tree import DecisionTreeRegressor
> from sklearn.model_selection import train_test_split
> from sklearn.metrics import r2_score
> 
> n_points = 1000  # create regression problem
> x, y = make_friedman1(n_samples=n_points, n_features=15,
>                       noise=1.0, random_state=100)
> 
> # split to train/test set
> x_train, x_test, y_train, y_test = \
>     train_test_split(x, y, test_size=0.33, random_state=100)
> 
> # boosting sklearn
> from sklearn.ensemble import GradientBoostingRegressor
> breg = GradientBoostingRegressor(learning_rate=0.1,
>          n_estimators=100, max_depth=3, random_state=100)
> breg.fit(x_train, y_train)
> yhat = breg.predict(x_test)
> print("Gradient Boosting R^2 score = ", r2_score(y_test, yhat))
> 
> # Gradient Boosting R^2 score = 0.8993055635639531
> ```

> [!example]- `AdaBoost.py` — AdaBoost from scratch
> 
> ```python
> from sklearn.datasets import make_blobs
> from sklearn.tree import DecisionTreeClassifier
> from sklearn.model_selection import train_test_split
> from sklearn.metrics import zero_one_loss
> import numpy as np
> 
> def ExponentialLoss(y, yhat):
>     n = len(y)
>     loss = 0
>     for i in range(n):
>         loss = loss + np.exp(-y[i] * yhat[i])
>     loss = loss / n
>     return loss
> 
> # create binary classification problem
> np.random.seed(100)
> n_points = 100  # points
> x, y = make_blobs(n_samples=n_points, n_features=5, centers=2,
>                   cluster_std=20.0, random_state=100)
> y[y == 0] = -1
> 
> # AdaBoost implementation
> BoostingRounds = 1000
> n = len(x)
> W = 1/n * np.ones(n)
> Learner = []
> alpha_b_arr = []
> for i in range(BoostingRounds):
>     clf = DecisionTreeClassifier(max_depth=1)
>     clf.fit(x, y, sample_weight=W)
>     Learner.append(clf)
>     train_pred = clf.predict(x)
>     err_b = 0
>     for i in range(n):
>         if train_pred[i] != y[i]:
>             err_b = err_b + W[i]
>     err_b = err_b / np.sum(W)
>     alpha_b = 0.5 * np.log((1 - err_b) / err_b)
>     alpha_b_arr.append(alpha_b)
>     for i in range(n):
>         W[i] = W[i] * np.exp(-y[i] * alpha_b * train_pred[i])
> 
> yhat_boost = np.zeros(len(y))
> for j in range(BoostingRounds):
>     yhat_boost = yhat_boost + alpha_b_arr[j] * Learner[j].predict(x)
> 
> yhat = np.zeros(n)
> yhat[yhat_boost >= 0] = 1
> yhat[yhat_boost < 0] = -1
> print("AdaBoost Classifier exponential loss = ", ExponentialLoss(y, yhat_boost))
> print("AdaBoost Classifier zero--one loss = ", zero_one_loss(y, yhat))
> 
> # AdaBoost Classifier exponential loss = 0.004224013663777142
> # AdaBoost Classifier zero--one loss = 0.0
> ```

---

## Summary

|Method|Loss|Initial $g_0$|What each learner fits|Step rule|
|---|---|---|---|---|
|**Regression Boosting**|Squared-error|$\bar{y}$|Residuals $e_i^{(b)}$|$g_{b-1} + \gamma h_b$|
|**Gradient Boosting**|Any differentiable|$0$|Negative gradient $r_i^{(b)}$|$g_{b-1} + \gamma h_b$|
|**AdaBoost**|Exponential $e^{-y\hat{y}}$|$0$|Weighted classifier $c_b$|$g_{b-1} + \alpha_b c_b$|

> [!summary] Key Takeaways
> 
> - Boosting builds an ensemble **sequentially**, each learner correcting its predecessors.
> - **Weak learners** (e.g. shallow trees) become strong when combined.
> - The **step-size / learning rate** ($\gamma$) is crucial for controlling overfitting — smaller is safer.
> - **Gradient boosting** generalizes regression boosting to any differentiable loss via gradient descent.
> - **AdaBoost** reweights samples each round, up-weighting misclassified points, with an analytically optimal $\alpha_b$.

### Boosting Core Idea

> **Context:** Boosting aims to improve the accuracy of any learning algorithm, especially when it involves weak learners — simple prediction functions that perform only slightly better than random guessing (shallow decision trees typically yield weak learners). Like bagging it uses an ensemble, but the learners are trained sequentially rather than independently.

> [!definition] Sequential Boosting
> 
> In bagging learners are trained independently; in **boosting** prediction functions are learned **sequentially**, each using information from the previous ones.
> 
> Starting from a weak learner $g_0$ on data $\tau = \{(\boldsymbol{x}_i, y_i)\}_{i=1}^{n}$, we boost it as $g_1 := g_0 + h_1$, where $h_1$ minimizes the training loss for $g_0 + h_1$ over all functions $h$ in a class $\mathcal{H}$:
> 
> $$h_1 = \underset{h \in \mathcal{H}}{\arg\min}\ \frac{1}{n}\sum_{i=1}^{n} \text{Loss}\big(y_i, g_0(\boldsymbol{x}_i) + h(\boldsymbol{x}_i)\big)$$
> 
> Repeating yields the **boosted prediction function**:
> 
> $$g_B(\boldsymbol{x}) = g_0(\boldsymbol{x}) + \sum_{b=1}^{B} h_b(\boldsymbol{x})$$

### Regression Boosting (Squared-Error Loss)

> **Context:** With squared-error loss, each learner is fitted to the residuals of the current model. A smooth updating step controlled by a step-size parameter is preferred over a plain additive update, since it helps reduce overfitting.

> [!definition] Shrinkage Step and Residuals
> 
> Instead of $g_b = g_{b-1} + h_b$, use the smooth update with **step-size** $\gamma$:
> 
> $$g_b = g_{b-1} + \gamma\, h_b$$
> 
> Using squared-error loss, start with $g_0(\boldsymbol{x}) = \frac{1}{n}\sum_{i=1}^{n} y_i$. Each $h_b$ is fitted to the **residuals** of $g_{b-1}$:
> 
> $$e_i^{(b)} := y_i - g_{b-1}(\boldsymbol{x}_i)$$
> 
> The step-size $\gamma$ controls the **speed** of fitting: smaller $\gamma$ takes smaller steps and is of great practical importance for **avoiding overfitting**.

> [!algorithm] Algorithm 1 — Regression Boosting with Squared-Error Loss
> **Input:** Training set $\tau = {(\boldsymbol{x}_i, y_i)}_{i=1}^{n}$, number of boosting rounds $B$, shrinkage step-size $\gamma$. **Output:** Boosted prediction function.
> 
> 1. Set $g_0(\boldsymbol{x}) \leftarrow n^{-1}\sum_{i=1}^{n} y_i$
> 2. **for** $b = 1$ **to** $B$ **do**
> 3.      Set $e_i^{(b)} \leftarrow y_i - g_{b-1}(\boldsymbol{x}_i)$ for $i = 1,\dots,n$, and let $$\tau_b \leftarrow {(\boldsymbol{x}_i, e_i^{(b)})}_{i=1}^{n}$$
> 4.      Fit a prediction function $h_b$ on the training data $\tau_b$
> 5.      Set $g_b(\boldsymbol{x}) \leftarrow g_{b-1}(\boldsymbol{x}) + \gamma h_b(\boldsymbol{x})$
> 6. **return** $g_B$

### Gradient Boosting

> **Context:** The step in regression boosting can be viewed as a step in the direction of the negative gradient of the squared-error loss. Since for squared-error loss the negative gradient equals twice the residual, the same gradient-descent idea generalizes to any differentiable loss function.

> [!definition] From Residuals to Gradients
> 
> For squared-error loss:
> 
> $$-\left.\frac{\partial\, \text{Loss}(y_i, z)}{\partial z}\right|_{z = g_{b-1}(\boldsymbol{x}_i)} = 2\big(y_i - g_{b-1}(\boldsymbol{x}_i)\big)$$
> 
> i.e. twice the residual $e_i^{(b)}$. The same idea works for **any differentiable loss**, giving **gradient boosting**. The main steps:
> 
> 1. Calculate a **negative gradient** on the $n$ training points.
>     
> 2. Fit a **simple model** (e.g. a shallow tree) to approximate the gradient.
>     
> 3. Make a **$\gamma$-sized step** in the direction of the negative gradient.
>     

> [!note] Gradient Boosting 
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

### AdaBoost (Binary Classification)

> **Context:** AdaBoost handles binary classification with labels in $\{-1, 1\}$ using the exponential loss. It builds an additive sequence in which each new term is a weighted classifier, and at each round it reweights the training samples, up-weighting those misclassified so far.

> [!definition] AdaBoost Formulation
> 
> AdaBoost builds $g_B(\boldsymbol{x}) = g_0(\boldsymbol{x}) + \sum_{b=1}^{B} h_b(\boldsymbol{x})$, where each $h_b(\boldsymbol{x}) = \alpha_b c_b(\boldsymbol{x})$ with $\alpha_b \in \mathbb{R}_+$ and classifier $c_b(\boldsymbol{x}) \in \{-1, 1\}$ from a class $\mathcal{C}$.
> 
> Using the **exponential loss** $\text{Loss}(y, \hat{y}) = e^{-y\hat{y}}$, each iteration solves:
> 
> $$(\alpha_b, c_b) = \underset{\alpha \ge 0,\ c \in \mathcal{C}}{\arg\min}\ \frac{1}{n}\sum_{i=1}^{n} \text{Loss}\big(y_i, g_{b-1}(\boldsymbol{x}_i) + \alpha c(\boldsymbol{x}_i)\big)$$
> 
> Defining the **weights** $w_i^{(b)} := \exp\{-y_i g_{b-1}(\boldsymbol{x}_i)\}$ and the **weighted zero–one training loss**:
> 
> $$\ell_\tau^{(b)}(c) := \frac{\sum_{i=1}^{n} w_i^{(b)} \mathbb{I}\{c(\boldsymbol{x}_i) \neq y_i\}}{\sum_{i=1}^{n} w_i^{(b)}}$$
> 
> the optimal classifier minimizes $\ell_\tau^{(b)}$, and the optimal step-size is:
> 
> $$\alpha_b = \frac{1}{2}\ln\left(\frac{1 - \ell_\tau^{(b)}(c_b)}{\ell_\tau^{(b)}(c_b)}\right)$$
> 
> The final classification is $\text{sign}\left(\sum_{b=1}^{B} \alpha_b c_b(\boldsymbol{x})\right)$.

> [!note] AdaBoost 
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

> [!definition] Reweighting Intuition
> 
> - At $b = 1$ every sample gets equal weight $w_i^{(1)} = 1/n$.
>     
> - At $b > 1$ the weights of **incorrectly classified** observations are **increased** and those of **correctly classified** ones **decreased**, so misclassified samples get a better chance of being classified correctly next round.
>     
> - The step-size $\alpha_b$ is optimal in the sense of training-loss minimization. Overfitting can be reduced by fixing $\alpha_b = \gamma$ to a small constant instead of the optimal value.
>     

### Summary

> **Context:** The three boosting variants share the same sequential additive structure but differ in loss function, initialization, what each learner fits, and the update step.

> [!definition] Comparison of Boosting Methods
> 
> | Method | Loss | Initial $g_0$ | What each learner fits | Step rule |
> | --- | --- | --- | --- | --- |
> | **Regression Boosting** | Squared-error | $\bar{y}$ | Residuals $e_i^{(b)}$ | $g_{b-1} + \gamma h_b$ |
> | **Gradient Boosting** | Any differentiable | $0$ | Negative gradient $r_i^{(b)}$ | $g_{b-1} + \gamma h_b$ |
> | **AdaBoost** | Exponential $e^{-y\hat{y}}$ | $0$ | Weighted classifier $c_b$ | $g_{b-1} + \alpha_b c_b$ |
> 
> - Boosting builds an ensemble **sequentially**, each learner correcting its predecessors.
>     
> - **Weak learners** (e.g. shallow trees) become strong when combined.
>     
> - The **step-size / learning rate** $\gamma$ is crucial for controlling overfitting — smaller is safer.
>     
> - **Gradient boosting** generalizes regression boosting to any differentiable loss via gradient descent.
>     
> - **AdaBoost** reweights samples each round, up-weighting misclassified points, with an analytically optimal $\alpha_b$.
>     

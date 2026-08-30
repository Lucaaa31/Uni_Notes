# Logistic Regression
Logistic Regression comes from the idea of **using Linear Regression in a Classification problem**. We cannot use standard Linear Regression in order to do that, because it outputs real value and classification tasks typically require a binary or discrete prediction.

So, we always use the conditional probability of the Linear Regression:
$$
P(y|X,\theta)
$$
where $\theta$ are the weights of our model, but instead of a linear function in output we use a **sigmoid function** to classify the output:
$$
g(h)=\frac{1}{1 + e^{-h}}
$$
that it is always a value between $0$ and $1$.
![[07 - Logistic Regression & Calibration-1788008715212.webp|366]]

## Binary Classification
Using the sigmoid, for a binary classification problem, we model the conditional probability of the target variable $y$ belonging to class $1$ (or class $0$) given the input feature vector $X$ and parameters $\mathbf{w}$.:
- **Probability of the positive class ($y = 1$):**
$$
P(y=0 | X, \theta) = g(w^T X) = \frac{1}{1 + e^{w^TX}}
$$
- **Probability of the negative class ($y = 0$):**

$$P(y = 0 \mid X, \mathbf{w}) = 1 - g(\mathbf{w}^T X) = \frac{e^{-\mathbf{w}^T X}}{1 + e^{-\mathbf{w}^T X}} = \frac{1}{1 + e^{\mathbf{w}^T X}}$$
## Determining Parameters
Similar to the Regression problems we look for the **Maximum Likelihood Estimator** (always with the i.i.d. assumption):
$$
L( y | X,  \theta) = \prod_{i=1}^m (1 - g( X_i,  \theta))^{y_i} \cdot g( X_i,  \theta)^{(1 - y_i)}
$$
We prefer to maximize the **logarithmic loss function** instead of the simple loss:
$$LL(y \mid X, w) = \sum_{i=1}^N y_i \ln(1 - g(X_i, w)) + (1 - y_i) \ln g(X_i, w)$$
Some comments on the transformation:
- The $\prod$ becomes a $\sum$ because of the property $$\ln\left(\prod_{i=1}^N a_i\right) = \sum_{i=1}^N \ln(a_i)$$
- Because of the $ln$ the exponent goes down.

$LL$ is called **Logistic Loss**, maximizing this quantity means minimizing the Negative Log-Likelihood, that's what we want:
$$
... = \sum_{i = 1}^N y_i \ln \frac{1 - g(X_i, w)}{g(X_i, w)} + \ln g(X_i, w)
$$
And by substituting $g(X_i, \theta)$ with the full sigmoid function we get:
$$
= \sum_{i = 1}^N y_i \cdot w^T \cdot x
_i - \ln(1 - e^{w^T X_i})$$
This problem has not closed solution, so we have to derivate w.r.t. each component of $w$. The good thing is that this is a concave function, so a **gradient ascent** is feasible.
Let's consider the partial derivative for a generic component $w$:
$$
\frac{\partial \ln(L)}{\partial w_j} = \frac{\partial}{\partial w_j} \sum_{i = 1}^m \left(y_i \cdot w^T \cdot \vec x_i - \ln(1 + e^{w ^T \cdot \vec x_i}) \right)
$$
$$
= \sum_{i = 1}^m \frac{\partial}{\partial w_j} \left[ y_i (w^t X_i) - \ln (1 + e^{w^T X_i}) \right]
$$
The scalar product $w^T \cdot X_{i}$ can be written as: $$ \sum_{k = 1}^p w_k \cdot X_{ik} $$so its derivative w.r.t. $w_j$ is equal to $X_{ij}$, making the derivative of $y_i (w^T X_i) = y_i X_{ij}$. 

Then we use the chain rule for the derivative of $\ln(1 + e^{w^T X_i})$: $$ \frac{\partial}{\partial w_j}\ln(1 + e^{w^T X_i}) = \frac{1}{1 + e^{w^T X_i}} \cdot e^{w^T X_i}\cdot X_{ij} = (1 - g(X_i, w))\cdot X_{ij} $$ so overall we get: $$ \frac{\partial \ln(L)}{\partial w_j} = \sum_{i=1}^m X_{ij}\left(y_i - (1-g(X_i, w))\right) $$
So what we will do is iteratively adjust each component of $w$ by computing this quantity multiplied by a (small) constant, our learning rate. Here’s the pseudo code:
```pseudo
\begin{algorithm}
\caption{Logistic Loss Gradient Ascent}
\begin{algorithmic}
\State Choose $\epsilon$
\State Start with a random guess for $w$
\Repeat
    \For{all $j$}
        \State $w_j \gets w_j + \epsilon \cdot \sum_{i=1}^m X_{ij} (y_i - (1 - g(X_i, w)))$
    \EndFor
\Until{no improvement for $\ln(L(y \mid X ; w))$}
\end{algorithmic}
\end{algorithm}
```
---


## Linear Model

$$Y_{n \times 1} = X_{n \times p} , \beta_{p \times 1} + \varepsilon_{n \times 1}$$

$$\varepsilon_{n \times 1} \sim \mathcal{N}_n\left(\mathbf{0}_{n \times 1},; \sigma^2 I_{n \times n}\right)$$

Given observations $Y_1 = y_1, \ldots, Y_n = y_n$ _(observations = realizations, instances of $Y_1, \ldots, Y_n$)_, we compute the **MLE (maximum likelihood estimate)** of $\beta$ and $\sigma^2$ by maximizing the likelihood function, to get:
$$X'X\beta = X' \underset{n \times 1}{y} $$
To solve for $\beta$, assume $X'X$ is invertible (i.e. $(X'X)^{-1}$ exists), which happens when the $p$ columns of $X$ are linearly independent.
Then we solve for $\beta$:

$$(X'X)(X'X)\hat{\beta} = (X'X)^{-1}X'y$$

$$\boxed{\hat{\beta} = (X'X)^{-1}X'y} \quad \leftarrow \text{least square estimate of } \beta$$
Remember we also have $\sigma^2$.
Once we have maximized the likelihood in $\beta$, we are left with maximizing:
$$\max_{\sigma^2} \mathcal{L}(\hat{\beta}, \sigma^2;, y_1 \ldots y_n)$$
equivalent to maximizing the log likelihood:
$$\log \mathcal{L}(\hat{\beta}, \sigma^2;, y_1 \ldots y_n) = -\frac{n}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}(y - X\hat{\beta})'(y - X\hat{\beta})$$
We differentiate with respect to $\sigma^2$ and set the derivative to $0$:
$$-\frac{n}{2} \cdot \frac{1}{2\pi\sigma^2} + \frac{1}{2\sigma^4}(y - X\hat{\beta})'(y - X\hat{\beta}) = 0 \qquad \text{assume } \sigma^2 \neq 0$$
We get:
$$\hat{\sigma}^2 = \frac{(y - X\hat{\beta})'(y - X\hat{\beta})}{n} = \frac{\sum\left(y_i - \sum_{p=0}^{j-1} x_{i,j}\hat{\beta}_p\right)^2}{n} = \frac{1}{n} \cdot \text{(sum of squared differences between observed and fitted values)}$$

Where:

- $\hat{\beta}$ — estimates of $\beta$
- $\hat{y} = X\hat{\beta}$ — fitted $y$ values
- $y$ — observed $y$ values
- $e = (y - X\hat{\beta})$ — vector of residuals
- $e'e = (y - X\hat{\beta})'(y - X\hat{\beta})$ — **residual sum of squares**

---

## Example: The Null Model

$$Y = \begin{pmatrix}1 \\ \vdots \\ 1\end{pmatrix}_{n \times 1} \beta_0 + \varepsilon$$

$$\hat{\beta} = (X'X)^{-1}X'y = \left((1\cdots 1)\begin{pmatrix}1\\ \vdots \\ 1\end{pmatrix}\right)^{-1}(1 \cdots 1)\begin{pmatrix}y_1 \\ \vdots \\ y_n\end{pmatrix} = \frac{1}{n}\sum y_i = \bar{y}$$

This is the **sample mean of the $y$'s**, an MLE estimate of $\beta_0$.
Natural, since $\beta_0 = E(Y_i)$ for each $i$, because $Y_1, \ldots, Y_n \overset{\text{iid}}{\sim} \mathcal{N}(\beta_0, \sigma^2)$.

$$\hat{\sigma}^2 = \frac{\sum(y_i - \hat{y}_i)^2}{n} = \frac{\sum(y_i - \bar{y})^2}{n} \quad \text{MLE of } \sigma^2$$

since $\hat{y} = X\hat{\beta} = \begin{pmatrix}1 \\ \vdots \\ 1\end{pmatrix}\bar{y} = \begin{pmatrix}\bar{y} \\ \vdots \\ \bar{y}\end{pmatrix}$

Notice that in this one-sample problem we prefer to use:

$$s^2 = \frac{\sum(y_i - \bar{y})^2}{n - 1} = \frac{n}{n-1}\hat{\sigma}^2$$

since it is an **unbiased estimate** of $\sigma^2$. Something similar happens for the general linear model — see later.

---

## Example: Simple Linear Regression

$$\begin{pmatrix}Y_1 \\ \vdots \\ Y_n\end{pmatrix} = \underbrace{\begin{pmatrix}1 & x_1 \\ 1 & x_2 \\ \vdots & \vdots \\ 1 & x_n\end{pmatrix}}_{X}\begin{pmatrix}\beta_0 \\ \beta_1\end{pmatrix} + \begin{pmatrix}\varepsilon_1 \\ \varepsilon_2 \\ \vdots \\ \varepsilon_n\end{pmatrix}$$

$$\begin{pmatrix}\hat{\beta}_0 \ \hat{\beta}_1\end{pmatrix} = (X'X)^{-1}X'y$$

$$= \left(\begin{pmatrix}1 & \cdots & 1 \ x_1 & \cdots & x_n\end{pmatrix}\begin{pmatrix}1 & x_1 \\ \vdots & \vdots \\ 1 & x_n\end{pmatrix}\right)^{-1}\begin{pmatrix}1 & \cdots & 1 \ x_1 & \cdots & x_n\end{pmatrix}\begin{pmatrix}y_1 \\ \vdots \\ y_n\end{pmatrix}$$

$$= \begin{pmatrix}n & \sum x_i \\ \sum x_i & \sum x_i^2\end{pmatrix}^{-1}\begin{pmatrix}\sum y_i \\ \sum x_i y_i\end{pmatrix}$$

$$= \frac{1}{n\sum x_i^2 - (\sum x_i)^2}\begin{pmatrix}\sum x_i^2 & -\sum x_i \\ -\sum x_i & n\end{pmatrix}\begin{pmatrix}\sum y_i \ \sum x_i y_i\end{pmatrix} \quad \leftarrow \text{exercise}$$

$$= \cdots = \begin{cases} \hat{\beta}_0 = \bar{y} - \hat{\beta}_1 \bar{x} \\ \hat{\beta}_1 = \dfrac{\sum(x_i - \bar{x})y_i}{\sum(x_i - \bar{x})^2} = \dfrac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sum(x_i - \bar{x})^2} \end{cases}$$

> **Note:** $n\sum(x_i - \bar{x})^2$ $= \sum(x_i^2 - 2x_i\bar{x} + \bar{x}^2)$ $= \sum x_i^2 - 2\bar{x}\sum x_i + n\bar{x}^2$ $= \sum x_i^2 - 2\bar{x}(n\bar{x}) + n\bar{x}^2$ $= n\sum x_i^2 - (\sum x_i)^2$

As for $\hat{\sigma}^2$:
$$\hat{\sigma}^2 = \frac{\sum\left(y_i - (\hat{\beta}_0 + \hat{\beta}_1 x_i)\right)^2}{n}$$

or, better (see later):
$$\text{RMS} = \frac{n}{n-2}\hat{\sigma}^2 = \frac{\sum\left(y_i - (\hat{\beta}_0 + \hat{\beta}_1 x_i)\right)^2}{n - 2}$$

![[2 - Estimation in Linear Models-1780247639925.webp|592]]
The $y$ corresponding to $\bar{x}$ is:
$$y = \hat{\beta}_0 + \hat{\beta}_1 \bar{x} = \bar{y} - \hat{\beta}_1\bar{x} + \hat{\beta}_1\bar{x} = \bar{y}$$

---

## Estimate vs. Estimator

The quantity $\hat{\beta} = (X'X)^{-1}X'y$ corresponding to a specific observed vector $y$ is called an **ESTIMATE** of $\beta$ (a number resulting from an algorithm).

$\hat{\beta} = (X'X)^{-1}X'Y$ is the corresponding random variable, called the **ESTIMATOR** of $\beta$.

We can derive the **sampling distribution** of the $\hat{\beta}$ estimator.
Since $Y \sim \mathcal{N}(X\beta,, \sigma^2 I)$, $\hat{\beta}$ is a linear transformation of it, therefore:

$$\hat{\beta} \sim \mathcal{N}_p(?,, ?)$$
$$E(\hat{\beta}) = E\left((X'X)^{-1}X'Y\right) = (X'X)^{-1}X',E(Y)$$ $$= (X'X)^{-1}X'X\beta = \beta$$
which is the property of **unbiasedness** of $\hat{\beta}$.

$$\text{VarCov}(\hat{\beta}) = \text{VarCov}\left((X'X)^{-1}X'Y\right)$$

$$= (X'X)^{-1}X';\text{VarCov},Y;\left((X'X)^{-1}X'\right)' \quad \text{(by property of VarCov)}$$

$$= (X'X)^{-1}X';\sigma^2 I;X(X'X)^{-1} \quad \text{(by symmetry of } X'X\text{)}$$

$$= \sigma^2 (X'X)^{-1}X'X(X'X)^{-1}$$

$$= \sigma^2 (X'X)^{-1}$$

In particular:

$$\text{Var}(\hat{\beta}_{n-1}) = \sigma^2 (X'X)^{-1}_{ii}$$

e.g. $\text{Var}(\hat{\beta}_0) = \sigma^2 (X'X)^{-1}_{11}$

---

## Sampling Distribution of $\hat{\sigma}^2$

It can be proved that if we take the random vector of residuals:

$$E = Y - \hat{Y} = Y - X\hat{\beta}$$ $$= Y - (X'X)^{-1}X'Y$$ $$= \left(I - (X'X)^{-1}X'\right)Y$$

(where $e$ is the particular realization of $E$), then the sampling distribution of:

$$\frac{E'E}{\sigma^2} = \frac{\sum E_i^2}{\sigma^2} \sim \chi^2(n - p)$$

_"has a chi-square distribution with $n - p$ degrees of freedom"_

This is a distribution studied in probability such that:

$$E\left[\chi^2(n-p)\right] = n - p$$

and its cut-off points can be computed _(see also slides on chi² testing)_.

Therefore:

$$E\left(\frac{E'E}{\sigma^2}\right) = n - p$$

$$\hat{\sigma}^2 = \frac{E'E}{n} \quad \text{our MLE estimator of } \sigma^2$$

$$E\left(\frac{n\hat{\sigma}^2}{\sigma^2}\right) = n - p$$

$$E(\hat{\sigma}^2) = \frac{n-p}{n},\sigma^2 \quad \Rightarrow \text{ so } \hat{\sigma}^2 \text{ is biased; it is easy to correct it}$$

And to estimate $\sigma^2$ **unbiasedly** we will use:

$$\boxed{\text{RMS} = \frac{E'E}{n - p}} \quad \text{so that } E(\text{RMS}) = \sigma^2$$
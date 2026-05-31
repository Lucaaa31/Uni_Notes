1. First, simplest model for supervised learning
2. Important: use the multivariate normal distribution

$$Y = f(x_1, \ldots, x_{p-1}) + \varepsilon$$

- $Y$ = response variable
- $f(\cdot)$ = the simplest function is **linear**
- $\varepsilon$ = error (multivariate normal)

---

## More Particularly

$$Y = \begin{pmatrix} Y_1 \\ Y_2 \\ \vdots \\ Y_n \end{pmatrix}$$

Vector of responses, each of them viewed as random output values.

$$X_{n \times p} = \begin{pmatrix} x_{11} & \cdots & x_{1p} \\ x_{21} & \cdots & x_{2p} \\ \vdots & & \vdots \\ x_{i1} & \cdots & x_{ip} \\ \vdots & & \vdots \\ x_{n1} & \cdots & x_{np} \end{pmatrix}$$

- Row 1 → for first response
- Row 2 → for second response
- Row $i$ → the vector of $p$ predictors corresponding to $Y_i$, input values

Almost always, the first column $\begin{pmatrix} x_{11} \\ \vdots \\ x_{n1} \end{pmatrix} = \begin{pmatrix} 1 \\ \vdots \\ 1 \end{pmatrix}$

---

## Compact Matrix Notation

$$\underset{n \times 1}{Y} = \underset{n \times p}{X} \quad  \underset{p \times 1}{\beta} + \underset{n \times 1}{\varepsilon}$$

- $\beta_{p \times 1}$ = p-vector of **unknown coefficients** containing the effects of the predictors
- $Y_{n \times 1}$ = vector of responses, random variables
- $X_{n \times p}$ = known matrix of input values of predictors, often under the control of the experimenter
- $\varepsilon_{n \times 1}$ = vector of random variables representing **errors**

---

## Example — Two-Sample Problem

$$\begin{pmatrix} Y_1 \\ \vdots \\ Y_{n_1} \\ Y_{n_1+1} \\ \vdots \\ Y_n \end{pmatrix} = \underset{n \times 2}{X} \begin{pmatrix} \beta_1^* \\ \beta_2^* \end{pmatrix} + \begin{pmatrix} \varepsilon_1 \\ \varepsilon_2 \\ \vdots \\ \varepsilon_n \end{pmatrix}$$

$$Y_i = \begin{cases} \beta_1^* + \varepsilon_i & i = 1, \ldots, n_1 \\ \beta_2^* + \varepsilon_i & i = n_1+1, \ldots, n \end{cases}$$

This is a **two-sample problem**. We have two samples $Y_1, \ldots, Y_{n_1}$ and $Y_{n_1+1}, \ldots, Y_n$ and we want to compare their means $\beta_1^*$ and $\beta_2^*$.

If you take:

- $\beta_1^* = \mu$ = mean of 1st group
- $\beta_2^* = \nu$ = mean of 2nd group
- $\varepsilon_1, \ldots, \varepsilon_n$ i.i.d. $\mathcal{N}(0, \sigma^2)$

then:$$Y_1, \ldots, Y_{n_1} \text{ i.i.d. } \mathcal{N}(\mu, \sigma^2)$$$$Y_{n_1+1}, \ldots, Y_n \text{ i.i.d. } \mathcal{N}(\nu, \sigma^2)$$
We prefer to have $\begin{pmatrix} 1 \\ \vdots \\ 1 \end{pmatrix}$ as the first column of $X$, therefore we rewrite:

$$\begin{pmatrix} Y_1 \\ Y_2 \\ \vdots \\ Y_n \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 1 & 0 \\ \vdots & \vdots \\ 1 & 0 \\ 1 & 1 \\ \vdots & \vdots \\ 1 & 1 \end{pmatrix} \begin{pmatrix} \beta_0 \\ \beta_1 \end{pmatrix} + \begin{pmatrix} \varepsilon_1 \\ \varepsilon_2 \\ \vdots \\ \varepsilon_n \end{pmatrix}$$

---

## Example 1 — Biomedical

$Y$ = difference of blood pressure before–after a certain pill is taken.

- First group: standard pill
- Second group: experimental pill

$Y_1, \ldots, Y_{n_1}$: responses obtained with standard pill in $n_1$ patients  
$Y_{n_1+1}, \ldots, Y_n$: responses obtained with experimental pill in $n - n_1$ patients

Suppose $\varepsilon_1, \ldots, \varepsilon_n$ are random errors, so we take them i.i.d. $\mathcal{N}(0, \sigma^2)$:

$$Y_1, \ldots, Y_{n_1} \text{ i.i.d. } \mathcal{N}(\beta_0, \sigma^2)$$ $$Y_{n_1+1}, \ldots, Y_n \text{ i.i.d. } \mathcal{N}(\beta_0 + \beta_1, \sigma^2)$$

$\beta_1$ = mean difference in blood pressure in experimental group **minus** mean difference in blood pressure in standard group = **effect of experimental therapy compared to standard therapy**.

---

## Example 2 — Investment

$Y$ = ROI (return on investment). First and second groups are returns observed under two different investment strategies.

- $Y_1, \ldots, Y_{n_1}$: ROI's observed in the past with strategy A
- $Y_{n_1+1}, \ldots, Y_n$: ROI's observed in the past with strategy B

$$\varepsilon_1, \ldots, \varepsilon_n \text{ i.i.d. } \mathcal{N}(0, \sigma^2)$$ $$Y_1, \ldots, Y_{n_1} \text{ i.i.d. } \mathcal{N}(\beta_0, \sigma^2)$$ $$Y_{n_1+1}, \ldots, Y_n \text{ i.i.d. } \mathcal{N}(\beta_0 + \beta_1, \sigma^2)$$

$\beta_1$ = mean difference in ROI between B and A.

---

## Even Simpler Example — Null Model

$$\begin{pmatrix} Y_1 \\ \vdots \\ Y_n \end{pmatrix}_{n \times 1} = \begin{pmatrix} 1 \\ \vdots \\ 1 \end{pmatrix}_{n \times 1} \beta_0 + \begin{pmatrix} \varepsilon_1 \\ \vdots \\ \varepsilon_n \end{pmatrix}_{n \times 1}$$

If $\varepsilon_1, \ldots, \varepsilon_n$ are i.i.d. $\mathcal{N}(0, \sigma^2)$, then $Y_1, \ldots, Y_n$ i.i.d. $\mathcal{N}(\beta_0, \sigma^2)$.  
This is a **one-sample normal problem**.

---

## Example — Simple Linear Regression
$$\begin{pmatrix} Y_1 \\ Y_2 \\ \vdots \\ Y_n \end{pmatrix} = \begin{pmatrix} 1 & x_1 \\ 1 & x_2 \\ \vdots & \vdots \\ 1 & x_n \end{pmatrix} \begin{pmatrix} \beta_0 \\ \beta_1 \end{pmatrix} + \begin{pmatrix} \varepsilon_1 \\ \varepsilon_2 \\ \vdots \\ \varepsilon_n \end{pmatrix}$$

where $x_1, \ldots, x_n$ is a **quantitative predictor**.

**Example Sales**: $Y_i$ = sales when $x_i$ is spent on advertising.

**Example Stress**:

- $Y_i$ = lifetime of a certain $i$-th mechanical component
- $x_i$ = stress cycles applied to $i$-th component

### Graphical Representation

Observe $Y_1 = y_1, \ldots, Y_n = y_n$ (realizations, or instances of the random variables $Y_1, \ldots, Y_n$). Then we can plot $(x_1, y_1), \ldots, (x_n, y_n)$ in a **scatterplot**.
![[1 - Linear Models-1780243907071.webp|642]]

- **After sampling**: observed data points with fitted line $\hat{\beta}_0 + \hat{\beta}_1 x$
- **Before sampling**: underlying model — $Y_1, \ldots, Y_n$ i.i.d. $\mathcal{N}(\beta_0 + \beta_1 x_i, \sigma^2)$ — shown as normal distributions centered on the regression line at each $x_i$

---

## General Form

$$Y = X\beta + \varepsilon \implies Y_i = \sum_{j=0}^{p-1} x_{ij} \beta_j + \varepsilon_i \quad \text{(in general, for } p \geq 1\text{)}$$

In the linear regression example (case $p = 2$):

$$Y_i = \beta_0 + \beta_1 x_i + \varepsilon_i$$

Since $\beta_0, \beta_1$ are unknown (and $\sigma^2$), we will use the observations $y_1, \ldots, y_n$ to **estimate** them:

- $\hat{\beta}_0$ = estimate of $\beta_0$
- $\hat{\beta}_1$ = estimate of $\beta_1$
- $\hat{\sigma}^2$ = estimate of $\sigma^2$

In general, we need a vector of estimates:

$$\hat{\beta} = \begin{pmatrix} \hat{\beta}_0 \\ \hat{\beta}_1 \\ \vdots \\ \hat{\beta}_{p-1} \end{pmatrix}$$

and $\hat{\sigma}^2$.

---

## Recap — Linear Model with Normal Errors

$$\underset{n \times 1}{Y} = \underset{n \times p}{X} \quad \underset{p \times 1}{\beta} + \underset{n \times 1}{\varepsilon}$$

$\varepsilon_1, \ldots, \varepsilon_n$ i.i.d. $\mathcal{N}(0, \sigma^2)$, so that:

$$\varepsilon \sim \mathcal{N}_n!\left(\begin{pmatrix} 0 \\ \vdots \\ 0 \end{pmatrix}, \begin{pmatrix} \sigma^2 & 0 & \cdots & 0 \\ 0 & \sigma^2 & & 0 \\ \vdots & & \ddots & \vdots \\ 0 & \cdots & 0 & \sigma^2 \end{pmatrix}\right) = \mathcal{N}_n\left(\mathbf{0}_{n \times 1}, \sigma^2 I_{n \times n}\right)$$

where $I_{n \times n}$ is the identity matrix.

Therefore, since linear transformations of normals are normal:

$$Y \sim \mathcal{N}_n(X\beta,; \sigma^2 I)$$

$$E(Y) = E(X\beta + \varepsilon) = X\beta + E(\varepsilon) = X\beta$$

$$\mathrm{VarCov}(Y) = \mathrm{VarCov}(X\beta + \varepsilon) = \mathrm{VarCov}(\varepsilon) = \sigma^2 I$$

---

## Estimation — Maximum Likelihood

This is a **parametric model**, i.e. a probabilistic structure for observable responses $Y_1, \ldots, Y_n$ with unknown parameters $\beta$ and $\sigma^2$ ($X$'s are known).

**How do we estimate $\beta$ and $\sigma^2$?**

In traditional parametric probabilistic models like this, **Maximum Likelihood** is the preferred method. (Fisher ~1925, but also Gauss ~1800)

The likelihood is the density of the observations $Y_1, \ldots, Y_n$ viewed as a function of the unknown parameters:

$$\mathcal{L}(\beta, \sigma^2;, y_1, \ldots, y_n) = f(y_1, \ldots, y_n;, \beta, \sigma^2)$$

Since $Y_1, \ldots, Y_n$ are independent normal random variables:

$$= \prod_{i=1}^{n} \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left \{-\frac{1}{2\sigma^2}\left(y_i - \sum_{j=0}^{p-1} x_{ij}\beta_j\right)^{2}\right\}$$

$$= (2\pi\sigma^2)^{-n/2} \exp\left \{-\frac{1}{2\sigma^2} \sum_{i=1}^{n}\left(y_i - \sum_{j=0}^{p-1} x_{ij}\beta_j\right)^{2}\right \}$$

In matrix notation:

$$= (2\pi\sigma^2)^{-n/2} \exp\left\{-\frac{1}{2\sigma^2}(y - X\beta)'(y - X\beta)\right\}$$

Now we maximize $\mathcal{L}$, which is equivalent to maximizing $\log \mathcal{L}$:

$$\max_{\beta,\sigma^2} \log \mathcal{L}(\cdot) = \max_{\beta,\sigma^2}\left(-\frac{n}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}(y-X\beta)'(y-X\beta)\right)$$

which is equivalent to minimizing $-\log \mathcal{L}$:
$$\min_{\beta,\sigma^2}\left(\frac{n}{2}\log(2\pi\sigma^2) + \frac{1}{2\sigma^2}(y-X\beta)'(y-X\beta)\right)$$

We do it in two steps: first for fixed $\sigma^2$, then for variable $\sigma^2$.

**For fixed $\sigma^2$:**

$$\min_{\beta}(y - X\beta)'(y - X\beta) = \min_{\beta},\sum_{i=1}^{n} \left(y_i - \sum_{j=0}^{p-1} x_{ij}\beta_j\right)^{!2}$$

> This is the famous **least squares problem**.

---

## Solving the Least Squares Problem

$$\min_{\beta},(y - X\beta)'(y - X\beta)$$

Expanding:

$$= \min_{\beta}\left(y'y - \beta'X'y - y'X\beta + \beta'X'X\beta\right)$$

Note: $\beta'X'y$ is a scalar equal to its transpose $y'X\beta$, so:

$$= \min_{\beta}\left(y'y - 2,y'X\beta + \beta'X'X\beta\right)$$

This is a smooth function, so we differentiate to find critical points.

**Matrix derivatives used:** $$\frac{\partial}{\partial x}(a'x) = a' \qquad \frac{\partial}{\partial x}(x'Ax) = 2Ax$$
Differentiating with respect to $\beta$:

$$\frac{\partial}{\partial \beta} \left(y'y - 2,y'X\beta + \beta'X'X\beta\right) = -2X'y + 2X'X\beta = 0$$

We get the **normal equations**:

$$\boxed{X'X,\beta = X'y}$$
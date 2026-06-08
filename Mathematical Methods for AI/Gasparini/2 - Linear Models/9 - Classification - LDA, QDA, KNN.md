> [!summary] Three Approaches to Supervised Classification
> 1. **Logistic regression**, as seen yesterday (GLM) \[classify as population 1 if $p >$ threshold, often threshold $= \frac{1}{2}$\]
> 2. **Model-based (statistical):** linear and quadratic Fisher discrimination
> 3. **Model-free (computational/algorithmic):** KNN
>
> We start with two populations, as in logistic regression, then we generalize to more than 2.

---
## 2) Fisher's Discriminant Analysis

![[9 - Classification - LDA, QDA, KNN-1780518385566.webp]]
Fisher's discriminant analysis **reverses the logic of logistic regression** and looks at the distribution of the features $X = (X_1, \ldots, X_p)'$ and assumes they are **quantitative**, and in particular **multivariate normal**.

> [!definition] Fisher's Discriminant Analysis Setup
> Two normal distributions, one for the $+$ population and one for the $-$ population:
>
> $$X_+ \sim \mathcal{N}(\mu_+, \Sigma_+) \quad \text{features from the + population}$$
>
> $$X_- \sim \mathcal{N}(\mu_-, \Sigma_-) \quad \text{features from the - population}$$
>
> **Fisher's idea:** look at the likelihood ratio $\dfrac{f_+(x)}{f_-(x)}$ and assign to $+$ population if the likelihood ratio is greater than a threshold (e.g. $\frac{1}{2}$).

---

## Derivation of the Classification Rule

$$f_+(x) = (2\pi)^{p/2} (\det \Sigma_+)^{-\frac{1}{2}} \exp\left \{-\frac{1}{2}(x-\mu_+)'\Sigma_+^{-1}(x-\mu_+)\right \}$$

$$f_-(x) = (2\pi)^{p/2} (\det \Sigma_-)^{-\frac{1}{2}} \exp\left\{-\frac{1}{2}(x-\mu_-)'\Sigma_-^{-1}(x-\mu_-)\right\}$$

So the likelihood ratio is:

$$\frac{f_+(x)}{f_-(x)} = \frac{(\det \Sigma_+)^{-1/2} \exp\left\{-\frac{1}{2}(x-\mu_+)'\Sigma_+^{-1}(x-\mu_+)\right\}}{(\det \Sigma_-)^{-1/2} \exp\left\{-\frac{1}{2}(x-\mu_-)'\Sigma_-^{-1}(x-\mu_-)\right\}} > \text{threshold}$$

Equivalently:

$$\exp\left\{-\frac{1}{2}(x-\mu_+)'\Sigma_+^{-1}(x-\mu_+) + \frac{1}{2}(x-\mu_-)'\Sigma_-^{-1}(x-\mu_-)\right\} > \frac{(\det \Sigma_-)^{-1/2}}{(\det \Sigma_+)^{-1/2}} \quad \text{[threshold*]}$$

Similarly, we can take logs and assign to $+$ population if:

$$-\frac{1}{2}(x-\mu_+)'\Sigma_+^{-1}(x-\mu_+) + \frac{1}{2}(x-\mu_-)'\Sigma_-^{-1}(x-\mu_-) > \log(\text{threshold}^{**})$$

Or equivalently, multiplying by $-\frac{1}{2}$:

> [!theorem] General Classification Rule
> $$\boxed{(x-\mu_+)'\Sigma_+^{-1}(x-\mu_+) - (x-\mu_-)'\Sigma_-^{-1}(x-\mu_-) < \text{threshold}^{***}} \tag{†}$$

which is a simpler expression.

---

## Example: $x$ is one-dimensional ($p = 1$)

$$\Sigma_+ = \Sigma_- = \sigma^2 \quad \text{(equal and one-dimensional)}$$
![[9 - Classification - LDA, QDA, KNN-1780518596372.webp]]
_(Diagram of two overlapping normal curves with means $\mu_-$ and $\mu_+$)_

**Classification rule:** assign to the $+$ population if $\frac{f_+(x)}{f_-(x)} >$ threshold.
I.e., in this simple case, by (†) above:

$$\frac{(x - \mu_+)^2}{\cancel{\sigma^2}} - \frac{(x - \mu_-)^2}{\cancel{\sigma^2}} < \text{threshold}^{****}$$

$$\cancel{x^2} - 2\mu_+ x + \mu_+^2 - \cancel{x^2} + 2\mu_- x - \mu_-^2 < \text{threshold}^{***}$$

$$x(-2\mu_+ + 2\mu_-) < \text{threshold}^{****} - \mu_+^2 + \mu_-^2$$

Or, since $\mu_+ > \mu_-$ in the picture:

$$x > \underbrace{\frac{\text{threshold}^{****} - \mu_+^2 + \mu_-^2}{2(\mu_+ - \mu_-)}}_{\text{another threshold}^5}$$

**So, assign to the $+$ population if $x >$ threshold**
_e.g.:
- $+$ population: people at risk for heart attack; 
- $-$population: people not at risk
- $x =$ cholesterol level — a diagnostic problem

---
## General Multivariate Rule

$$(x - \mu_+)'\Sigma_+^{-1}(x-\mu_+) - (x-\mu_-)\Sigma_-^{-1}(x-\mu_-) < t \quad \text{(constant threshold)}$$

Expanding:

> [!theorem] Fisher's Quadratic Discrimination Rule (QDA)
> $$x'\Sigma_+^{-1}x - 2\mu_+'\Sigma_+^{-1}x + \mu_+'\Sigma_+^{-1}\mu_+ - x'\Sigma_-^{-1}x + 2\mu_-'\Sigma_-^{-1}x - \mu_-'\Sigma_-^{-1}\mu_- < t$$

If $\Sigma_+ = \Sigma_- = \Sigma$ (**homoscedasticity**, i.e. same covariance matrices), the rule simplifies to:

> [!theorem] Fisher's Linear Discrimination Rule (LDA)
> $$\boxed{-2(\mu_+' - \mu_-')\Sigma^{-1}x < t}$$

---

## Example: $p = 1$, $\sigma_+^2 > \sigma_-^2$
![[9 - Classification - LDA, QDA, KNN-1780518944616.webp]]

(Diagram: quadratic univariate Fisher's discriminant rule — two normal curves with different variances)

In this case, you assign to $+$ population if $x$ is **too large OR too small**.

> [!tip] In practice, you do not know $\mu_+, \mu_-, \Sigma_+, \Sigma_-$, so you have to **estimate** them based on your labeled data. If $\hat{\Sigma}_+ \approx \hat{\Sigma}_-$ approximately, you go for **Fisher's linear** (LDA).

For more than 2 classes, the decision is still based on **linear or quadratic boundaries**.
![[9 - Classification - LDA, QDA, KNN-1780519058865.webp]]
_(Diagrams from ISLR2: 3 classes, $p = 2$ features $X_1$ and $X_2$)_

---

## 3) KNN — K-Nearest Neighbors

> [!definition] K-Nearest Neighbors
> KNN is a totally different approach that does **not** rely on distributional assumptions.
>
> - $K$ = a fixed number
> - $N$ = nearest
> - $N$ = neighbors
![[9 - Classification - LDA, QDA, KNN-1780519086527.webp]]
_(Diagram from ISLR2, page 40: 2 populations, 2 features, 6×2 labeled data)_

Suppose you want to classify $x$ based on $K = 3$ and threshold $= \frac{1}{2}$.
Since among the 3 nearest neighbors of $x$ you have 2 blues, you say $x$ is **blue** since $\frac{2}{3} > \frac{1}{2}$.
Easy to generalize to more than two populations.

---

## Generalizing Logistic Regression to More Than Two Classes

Regarding (1), how do you generalize logistic regression to more than two classes?

**Via multinomial regression!**

Fix one class as a reference (the first or the last class, for example), then estimate:

$$P(k\text{-class}),\ k = 1, 2, \ldots, K-1 \quad = \quad \frac{e^{\beta_{k0} + \beta_{k1}x_1 + \cdots + \beta_{kp}x_p}}{1 + \displaystyle\sum_{\ell=1}^{K-1} e^{\beta_{\ell 0} + \beta_{\ell 1}x_1 + \cdots + \beta_{\ell p}x_p}}$$

$$P(\text{last class } K,\ \text{taken as reference}) \quad = \quad \frac{1}{1 + \displaystyle\sum_{\ell=1}^{K-1} e^{\beta_{\ell 0} + \beta_{\ell 1}x_1 + \cdots + \beta_{\ell p}x_p}}$$

→ ISLR2 page 140

→ SVM done by prof. Vaccarino
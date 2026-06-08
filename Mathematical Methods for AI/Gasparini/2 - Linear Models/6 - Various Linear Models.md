## Graphical Recap of Linear Models

- **Response** $Y$: a random variable
- **Predictors:**
    - $x_1, x_2, z, z^2, \ldots$ → quantitative
    - $b_1, b_2, \ldots$ → qualitative binary
    - $a_1, a_2, \ldots$ → qualitative predictors with more than binary levels (**factors**)
---

### Model Taxonomy (Graphical)

| Graph                     | Model                                                        |
| ------------------------- | ------------------------------------------------------------ |
| $\mathbf{1} \to Y$        | **Null model**                                               |
| $x \to Y$                 | **Simple linear regression**                                 |
| $x \to Y,\quad x^2 \to Y$ | **Polynomial linear regression**                             |
| $b \to Y$                 | **Two-sample normal problem**                                |
| $x_1, x_2, z \to Y$       | **Multiple linear regression** (all predictors quantitative) |

---

### EXAMPLE: INSULATE

#### Additive Model (a linear model)
![[8 - Various Linear Models-1780319012743.webp]]
#### Linear Model with Interaction
![[8 - Various Linear Models-1780319038193.webp]]

> [!important] The interaction $bx$ is a **nonlinear function of $x$**; nonetheless the model is **linear IN THE COEFFICIENTS**.

Matrix form:

$$\begin{pmatrix} Y_1 \\ \vdots \\ Y_n \end{pmatrix} = \begin{pmatrix} 1 & 0 & x_1 & 0 \\ 1 & 0 & x_2 & 0 \\ \vdots & 1 & \vdots & 0 \\ 1 & 1 & x_n & x_n \end{pmatrix} \begin{pmatrix} \beta_0 \\ \beta_1 \\ \beta_2 \\ \beta_3 \end{pmatrix} + \begin{pmatrix} \varepsilon_1 \\ \vdots \\ \varepsilon_n \end{pmatrix}$$

---

### Polynomial Regression & Transformations

Even in polynomial regression, the predictor $x$ is transformed before entering the model:

![[8 - Various Linear Models-1780319127595.webp]]

Other simple transformations of the predictors can be contemplated, like:

$$\log(x), \quad \sqrt{x}, \quad \ldots$$

> [!info] **Neural networks** are a development of these ideas.

---
## Advantages of the Linear Model

> [!summary] Advantages of the Linear Model
> In a simple linear model, you pay the price of introducing probability distributions for $Y$, but you gain:
>
> - Interpretability
> - Uncertainty quantification
> - Confidence intervals
> - Tests
> - Prediction intervals
> - All other **Bayesian tools** (covered in Part 2 of this course)

---
## Qualitative Predictors (Factors)

With a number of levels $\geq 2$
- If 2 levels → **binary predictors** (also called _categories_ or _classes_)

When used as predictors, qualitative variables are often called **factors**.
![[8 - Various Linear Models-1780319224306.webp]]

A **factor** $a$ can be, e.g.: nationality, brand, eye color, A/C/T/G in genomics, etc.

A factor can have an alphanumeric representation (Italian, French, Russian…), but in order to represent it in a linear model we use **ONE-HOT ENCODING** (a binary vector with length = number of levels).

### One-Hot Encoding Example
Observations: It, It, Fr, It, Fr, Ru, Ru, …

$$X = \begin{pmatrix} \text{IT} & \text{FR} & \text{RU} & \cdots & \text{SS} \\ 1 & 0 & 0 & \cdots & 0 \\ 1 & 0 & 0 & \cdots & 0 \\ 0 & 1 & 0 & \cdots & 0 \\ \vdots & & & \ddots & \\ 0 & 0 & 1 & \cdots & 0 \\ \vdots & \vdots & \vdots & \vdots & \vdots \end{pmatrix}$$

> [!tip] Use one-hot encoding to build the $X$ matrix → $Y = X\beta + \varepsilon$

### Special Case: 2 Levels

If number of levels $= 2$, one-hot encoding was already implied. E.g.:

- Italian / Not Italian → two levels of variable "Italian?"

$$\begin{pmatrix} Y_1 \\ \vdots \\ Y_n \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 1 & 0 \\ \vdots & \vdots \\ 1 & 1 \end{pmatrix} \begin{pmatrix} \beta_0 \\ \beta_1 \end{pmatrix} + \varepsilon$$

_(top rows = "Not Italian", bottom rows = "Italian")_

### General Rule

> [!definition] One-Hot Encoding Rule for Factors
> For **one factor** with $I$ levels, we need:
>
> - the **intercept column** (1s)
> - **$I - 1$ binary columns** (one-hot encoding)

$$a \to Y \qquad \Longleftrightarrow \qquad a \to \text{[one-hot encoding]} \to Y$$

---

## Two-Factor Models

![[8 - Various Linear Models-1780319451068.webp]]

Two factors can combine to give:
- A **simple additive model** (_small model_) — no interaction
- A **model with interactions** (_large model_)

### Additive Model (no interaction)

![[8 - Various Linear Models-1780319508614.webp]]

### Interaction Model

(encoded A and encoded B feed through an interaction term into $Y$)

If we knew the means $\mu_{ij}$, mean $Y$ response corresponding to:
- $i-th$ level of $a$
- $j-th$ level of b

---

### Visualizing the Two-Factor Mean Structure

Let $\mu_{ij}$ = mean $Y$ when
- $a = i$ 
- $b = j$

#### Additive model → parallel lines

![[8 - Various Linear Models-1780319893284.webp]]

The two lines are **parallel** since the model is additive — the same vertical distance represents the difference between DRIVER and IRON across all brands.

#### Interaction model → non-parallel lines
If there are interactions, **the effect of Club may change depending on the level of Brand** — lines are no longer parallel.
![[8 - Various Linear Models-1780319931248.webp]]

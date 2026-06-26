## Motivation

Some fundamental **computational learning questions**:
- What can be learned efficiently?
- What is inherently hard to learn?
- Is there a general model of learning?

Three notions of **complexity** matter:
- **Computational complexity** — time and space.
- **Sample complexity** — amount of training data needed to learn successfully.
- **Mistake bounds** — number of mistakes before learning successfully.

---

## Definitions and Notation

| Symbol            | Meaning                                                                                                  |
| ----------------- | -------------------------------------------------------------------------------------------------------- |
| $X$               | Set of all possible instances/examples (e.g. all people characterised by height & weight).               |
| $c : X \to {0,1}$ | The **target concept** to learn; identified with its support ${x \in X : c(x) = 1}$.                     |
| $C$               | **Concept class** — a set of target concepts $c$.                                                        |
| $D$               | **Target distribution** — fixed probability distribution over $X$. Train & test examples drawn $\sim D$. |
| $S$               | The **training sample**.                                                                                 |
| $H$               | The **hypothesis set** (e.g. all linear classifiers).                                                    |
| $h$               | A hypothesis selected from $H$.                                                                          |

The learning algorithm receives sample $S$ and selects a hypothesis $h \in H$ approximating $c$.

---

## Errors

> [!note] True error (generalization error)
>  Error of $h$ w.r.t. target concept $c$ and distribution $D$: $$R(h) = \Pr_{x \sim D}[h(x) \neq c(x)] = \mathbb{E}_{x \sim D}\big[\mathbb{1}_{h(x) \neq c(x)}\big].$$

> [!note] Empirical error 
> Average error of $h$ on the training sample $S$: $$\widehat{R}_S(h) = \Pr_{x \sim \widehat{D}}[h(x) \neq c(x)] = \mathbb{E}_{x \sim \widehat{D}}\big[\mathbb{1}_{h(x) \neq c(x)}\big] = \frac{1}{m}\sum_{i=1}^{m}\mathbb{1}_{h(x_i) \neq c(x_i)}.$$

**Key relation** — the empirical error is an unbiased estimate of the true error: $$R(h) = \mathbb{E}_{S \sim D^m}\big[\widehat{R}_S(h)\big].$$

---

## The PAC Model

**PAC = Probably Approximately Correct** learning (Valiant, 1984).

> [!important] PAC-learnable
>  A concept class $C$ is **PAC-learnable** if there exists a learning algorithm $L$ such that:
> 
> - for all $c \in C$, all $\varepsilon > 0$, $\delta > 0$, and all distributions $D$, $$\Pr_{S \sim D^m}[R(h_S) \leq \varepsilon] \geq 1 - \delta.$$
> - for samples $S$ of size $m = \text{poly}(\frac{1}{\varepsilon}, \frac{1}{\delta})$ (fixed polynomial)
>
> Let's suppose we have to train our model to identify dogs:
> - $c$ is all the rule of the concept class (e.g. C =Dog, c1="has 4 legs", c2...)
> - $D$ distribution in the real world (i.e. how frequent you found them)
> - $S$ the sample (the photo of the dog)
> - $h_S$ hypothesis made by the model (it did it by watching the photos)
> - $\varepsilon$ max error tollerated
> - $\delta$ probability to catch a bad sample and fail the training



### Remarks
- The concept class $C$ is **known** to the algorithm.
- **Distribution-free** model: no assumption on $D$.
- Both training and test examples are drawn from the same $D$.
- **Probably** → confidence $1 - \delta$.
- **Approximately correct** → accuracy $1 - \varepsilon$.
- **Efficient PAC-learning** → runs in time $\text{poly}(1/\varepsilon, 1/\delta)$.

### Computational refinement
Accounting for representation cost:
- Cost for $x \in X$ is $O(n)$.
- Cost for $c \in C$ is $O(\text{size}(c))$.
- Running time extends to: $$O\big(\text{poly}(\frac{1}{\varepsilon}, \frac{1}{\delta})\big) \to O\big(\text{poly}(\frac{1}{\varepsilon}, \frac{1}{\delta}, n, \text{size}(c))\big).$$
---

## Example — Axis-Aligned Rectangle Learning

> [!example]
>  **Problem:** Learn an unknown **axis-aligned rectangle** $R$ using as small a labeled sample as possible. 
>  ![[2 - Learning with Finite Hypothesis Sets-1780865968193.webp]]
>  **Hypothesis:** is a rectangle $R'$; in general there may be false positives and false negatives.
**Simple method:** choose the _tightest consistent rectangle_ $R'$ for a large enough sample. ![[2 - Learning with Finite Hypothesis Sets-1780866039532.webp]]

Two questions: how large must the sample be, and is the class PAC-learnable?
### Proof setup
Fix $\varepsilon > 0$ and assume $\Pr_D[R] > \varepsilon$ (otherwise the result is trivial).
Let $r_1, r_2, r_3, r_4$ be the four smallest rectangles along the sides of $R$ such that $\Pr_D[r_i] \geq \tfrac{\varepsilon}{4}$. 
![[2 - Learning with Finite Hypothesis Sets-1780866121266.webp]]
With $R = [l, r] \times [b, t]$, one side is built as: $$r_4 = [l, s_4] \times [b, t], \qquad s_4 = \inf\Big\{s : \Pr\big[[l,s]\times[b,t]\big] \geq \tfrac{\varepsilon}{4}\Big\},$$ so that $\Pr_D\big[[l, s_4[ \times [b,t]\big] < \tfrac{\varepsilon}{4}$.

### Bounding the error
Errors only occur in the region $R \setminus R'$. By geometry: $$R(R') > \varepsilon \Rightarrow R' \text{ misses at least one region } r_i.$$
Therefore: $$ \begin{aligned} \Pr[R(R') > \varepsilon] &\leq \Pr\Big[\textstyle\bigcup_{i=1}^{4}\{R' \text{ misses } r_i\}\Big] \\ &\leq \sum_{i=1}^{4} \Pr[\{R' \text{ misses } r_i\}] \\ &\leq 4\Big(1 - \tfrac{\varepsilon}{4}\Big)^{m} \leq 4 e^{-\frac{m\varepsilon}{4}}. \end{aligned} $$![[2 - Learning with Finite Hypothesis Sets-1780866233303.webp]]
### Sample complexity
Set $\delta > 0$ to match the upper bound: $$4 e^{-\frac{m\varepsilon}{4}} \leq \delta \Longleftrightarrow m \geq \frac{4}{\varepsilon}\log\frac{4}{\delta}.$$

> [!success] Result
>  For $m \geq \dfrac{4}{\varepsilon}\log\dfrac{4}{\delta}$, with probability at least $1 - \delta$: $$R(R') \leq \varepsilon.$$
![[2 - Learning with Finite Hypothesis Sets-1780866273267.webp]]
### Notes on the proof
- This is an **infinite hypothesis set**, yet the proof is simple.
- The proof relies on **geometric properties**, which are key but **non-trivial to extend** to other classes (e.g. non-concentric circles).
- → Motivates the need for a **more general** proof and results.

---

## Learning Bound for Finite $H$ — Consistent Case
> [!important] Theorem
>  Let $H$ be a finite set of functions from $X$ to $\{0,1\}$, and $L$ an algorithm that for any target concept $c \in H$ and sample $S$ returns a **consistent** hypothesis $h_S$ (i.e. $\widehat{R}_S(h_S) = 0$). Then for any $\delta > 0$, with probability at least $1 - \delta$: $$R(h_S) \leq \frac{1}{m}\Big(\log|H| + \log\frac{1}{\delta}\Big).$$

### Proof
For any $\varepsilon > 0$, define the set of "bad" hypotheses $H_\varepsilon = {h \in H : R(h) > \varepsilon}$. Then: $$ \begin{aligned} \Pr\big[\exists h \in H_\varepsilon : \widehat{R}_S(h) = 0\big] &= \Pr\big[\widehat{R}_S(h_1)=0 \vee \cdots \vee \widehat{R}_S(h_{|H_\varepsilon|})=0\big] \\ &\leq \sum_{h \in H_\varepsilon} \Pr\big[\widehat{R}_S(h)=0\big] && \text{(union bound)} \\ &\leq \sum_{h \in H_\varepsilon} (1-\varepsilon)^m \leq |H|(1-\varepsilon)^m \leq |H| e^{-m\varepsilon}. \end{aligned} $$ Setting the RHS $\leq \delta$ and solving for $\varepsilon$ gives the bound.

### Remarks

- The algorithm can be **ERM** (Empirical Risk Minimization) if the problem is **realizable**.
- Error bound is **linear in $\tfrac{1}{m}$** and only **logarithmic in $\tfrac{1}{\delta}$**.
- $\log_2 |H|$ = number of bits needed to represent $H$.
- Bound is **loose for large $|H|$**, and **uninformative for infinite $|H|$**.

---

## Application — Conjunctions of Boolean Literals

> [!example] Problem
> Learn the class $C_n$ of conjunctions of boolean literals over at most $n$ variables (e.g. for $n=3$: $x_1 \wedge \overline{x_2} \wedge x_3$).

**Algorithm:** start with the maximally specific $x_1 \wedge \overline{x_1} \wedge \cdots \wedge x_n \wedge \overline{x_n}$, then **rule out literals incompatible with positive examples**.

### Worked example ($n = 6$)

|$x_1$|$x_2$|$x_3$|$x_4$|$x_5$|$x_6$|label|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|0|1|1|0|1|1|+|
|0|1|1|1|1|1|+|
|0|0|1|1|0|1|−|
|0|1|1|1|1|1|+|
|1|0|0|1|1|0|−|
|0|1|0|0|1|1|+|

Reading off the positives (columns that stay constant), the inferred hypothesis is: $$\overline{x_1} \wedge x_2 \wedge x_5 \wedge x_6.$$

### Complexity analysis

- Since $|H| = |C_n| = 3^n$, the **sample complexity** is: $$m \geq \frac{1}{\varepsilon}\Big((\log 3), n + \log\frac{1}{\delta}\Big).$$

Numerical example with $\delta = 0.02$, $\varepsilon = 0.1$, $n = 10$: $m \geq 149$.

- **Computational complexity**: polynomial — the algorithmic cost per training example is $O(n)$.

---

## Inconsistent Case

> [!warning] Motivation 
>Often **no consistent hypothesis** $h \in H$ exists — the typical case in practice (difficult problems, complex concept classes). But inconsistent hypotheses with a **small number of training errors** can still be useful.
> 
> We need a more powerful tool: **Hoeffding's inequality**.

### Hoeffding's Inequality

> [!note] Corollary
>  For any $\varepsilon > 0$ and any hypothesis $h : X \to {0,1}$: $$\Pr[R(h) - \widehat{R}(h) \geq \varepsilon] \leq e^{-2m\varepsilon^2},$$ $$\Pr[\widehat{R}(h) - R(h) \geq \varepsilon] \leq e^{-2m\varepsilon^2}.$$ Combining the two one-sided inequalities: $$\Pr[|R(h) - \widehat{R}(h)| \geq \varepsilon] \leq 2 e^{-2m\varepsilon^2}.$$

### Why we can't apply Hoeffding directly

> [!danger] Pitfall
>  Can we apply this bound to the hypothesis $h_S$ returned by our algorithm? **No** — because $h_S$ is **not a fixed hypothesis**; it depends on the training sample $S$. Also note $\mathbb{E}[\widehat{R}(h_S)]$ is **not** a simple quantity like $R(h_S)$.
> 
> Instead we need a bound that holds **simultaneously for all** $h \in H$ — a **uniform convergence bound**.

---

## Generalization Bound — Finite $H$ (Inconsistent Case)

> [!important] Theorem 
> Let $H$ be a finite hypothesis set. Then for any $\delta > 0$, with probability at least $1 - \delta$: $$\forall h \in H, \quad R(h) \leq \widehat{R}_S(h) + \sqrt{\frac{\log|H| + \log\frac{2}{\delta}}{2m}}.$$

### Proof

By the union bound: $$ \begin{aligned} \Pr\Big[\max_{h \in H} |R(h) - \widehat{R}_S(h)| > \varepsilon\Big] &= \Pr\Big[|R(h_1) - \widehat{R}_S(h_1)| > \varepsilon \vee \cdots \vee |R(h_{|H|}) - \widehat{R}_S(h_{|H|})| > \varepsilon\Big] \\ &\leq \sum_{h \in H} \Pr\big[|R(h) - \widehat{R}_S(h)| > \varepsilon\big] \\ &\leq 2|H|\exp(-2m\varepsilon^2). \end{aligned} $$ Setting $2|H|\exp(-2m\varepsilon^2) = \delta$ and solving for $\varepsilon$ gives the bound. 

### Remarks

- Thus, for a finite hypothesis set, with high probability: $$\forall h \in H, \quad R(h) \leq \widehat{R}_S(h) + O \left(\sqrt{\frac{\log|H|}{m}}\right).$$
- Error bound is now in $O \left(\tfrac{1}{\sqrt{m}}\right)$ — **quadratically worse** than the consistent case.
- $\log_2 |H|$ = number of bits needed to encode $H$.

> [!quote] Occam's Razor (William of Occam) _"Plurality should not be posited without necessity"_ — rephrased as _"the simplest explanation is best."_
> 
> - Invoked across many contexts (e.g. syntax).
> - **Kolmogorov complexity** is the corresponding information-theory framework.
> - Here: to minimize true error, choose the most **parsimonious** explanation (smallest $|H|$).

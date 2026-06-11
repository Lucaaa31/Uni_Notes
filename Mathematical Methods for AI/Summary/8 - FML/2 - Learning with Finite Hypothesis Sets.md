### Motivation

> **Context:** Computational learning theory asks what can be learned efficiently, what is inherently hard, and whether a general model of learning exists. Answering these requires distinguishing several notions of complexity.

> [!definition] Notions of Complexity
> 
> - **Computational complexity** — time and space.
>     
> - **Sample complexity** — amount of training data needed to learn successfully.
>     
> - **Mistake bounds** — number of mistakes before learning successfully.
>     

### Definitions and Notation

> **Context:** The PAC framework is built on a fixed instance space, a target concept to be learned, a concept class, and a distribution from which both training and test examples are drawn. The algorithm selects a hypothesis approximating the target.

> [!definition] Core Symbols
> 
> | Symbol | Meaning |
> | --- | --- |
> | $X$ | Set of all possible instances/examples. |
> | $c : X \to \{0,1\}$ | **Target concept** to learn; identified with its support $\{x : c(x)=1\}$. |
> | $C$ | **Concept class** — a set of target concepts $c$. |
> | $D$ | **Target distribution** over $X$; train & test examples drawn $\sim D$. |
> | $S$ | The **training sample**. |
> | $H$ | The **hypothesis set**. |
> | $h$ | A hypothesis selected from $H$. |

### Errors

> **Context:** The true error measures disagreement with the target concept over the full distribution, while the empirical error measures it on the sample. The empirical error is an unbiased estimate of the true error.

> [!definition] True and Empirical Error
> 
> - **True (generalization) error:** $R(h) = \Pr_{x \sim D}[h(x) \neq c(x)] = \mathbb{E}_{x \sim D}\big[\mathbb{1}_{h(x) \neq c(x)}\big]$.
>     
> - **Empirical error:** $\widehat{R}_S(h) = \frac{1}{m}\sum_{i=1}^{m}\mathbb{1}_{h(x_i) \neq c(x_i)}$.
>     
> - **Key relation:** $R(h) = \mathbb{E}_{S \sim D^m}\big[\widehat{R}_S(h)\big]$ (unbiased estimate).
>     

### The PAC Model

> **Context:** PAC (Probably Approximately Correct) learning, introduced by Valiant in 1984, is a distribution-free framework that defines what it means for a concept class to be learnable using a polynomially bounded sample.

> [!definition] PAC-Learnability
> 
> A concept class $C$ is **PAC-learnable** if there exists an algorithm $L$ such that for all $c \in C$, all $\varepsilon > 0$, $\delta > 0$, and all distributions $D$:
> 
> $$\Pr_{S \sim D^m}[R(h_S) \leq \varepsilon] \geq 1 - \delta$$
> 
> for samples of size $m = \text{poly}(1/\varepsilon, 1/\delta)$.
> 
> - The concept class $C$ is **known** to the algorithm; the model is **distribution-free**.
>     
> - **Probably** → confidence $1 - \delta$; **approximately correct** → accuracy $1 - \varepsilon$.
>     
> - **Efficient PAC-learning** runs in time $\text{poly}(1/\varepsilon, 1/\delta)$.
>     
> - Accounting for representation cost (cost $O(n)$ per instance, $O(\text{size}(c))$ per concept), the running time extends to $O\big(\text{poly}(1/\varepsilon, 1/\delta, n, \text{size}(c))\big)$.
>     

### Learning Bound for Finite H — Consistent Case

> **Context:** When the algorithm always returns a hypothesis consistent with the sample (zero empirical error) and the target lies in $H$, a simple union-bound argument gives a generalization guarantee that scales with $\log|H|$.

> [!theorem] Consistent Case Bound
> 
> Let $H$ be a finite set of functions $X \to \{0,1\}$ and $L$ an algorithm that returns a **consistent** hypothesis $h_S$ (i.e. $\widehat{R}_S(h_S) = 0$) for any target $c \in H$. Then for any $\delta > 0$, with probability at least $1 - \delta$:
> 
> $$R(h_S) \leq \frac{1}{m}\Big(\log|H| + \log\frac{1}{\delta}\Big)$$
> 
> - The algorithm can be **ERM** if the problem is **realizable**.
>     
> - The bound is **linear in $1/m$** and only **logarithmic in $1/\delta$**; $\log_2|H|$ is the number of bits to represent $H$.
>     
> - It is **loose for large $|H|$** and **uninformative for infinite $|H|$**.
>     

### Application — Conjunctions of Boolean Literals

> **Context:** The consistent-case bound applies directly to learning conjunctions of boolean literals, a finite concept class where a simple specific-to-general algorithm rules out literals contradicted by positive examples.

> [!definition] Learning Conjunctions
> 
> For the class $C_n$ of conjunctions of boolean literals over at most $n$ variables, start with the maximally specific conjunction $x_1 \wedge \overline{x_1} \wedge \cdots \wedge x_n \wedge \overline{x_n}$, then **rule out literals incompatible with positive examples**.
> 
> - Since $|H| = |C_n| = 3^n$, the **sample complexity** is $m \geq \frac{1}{\varepsilon}\big((\log 3)\, n + \log\frac{1}{\delta}\big)$.
>     
> - **Computational complexity** is polynomial: cost per training example is $O(n)$.
>     

### Inconsistent Case

> **Context:** In practice no consistent hypothesis often exists, so we must work with hypotheses that make some training errors. This requires Hoeffding's inequality, but it cannot be applied directly to the algorithm's chosen hypothesis because that hypothesis depends on the sample.

> [!theorem] Hoeffding's Inequality (Corollary)
> 
> For any $\varepsilon > 0$ and any fixed hypothesis $h : X \to \{0,1\}$:
> 
> $$\Pr[|R(h) - \widehat{R}(h)| \geq \varepsilon] \leq 2 e^{-2m\varepsilon^2}$$

> [!definition] Why Hoeffding Cannot Be Applied Directly
> 
> The bound cannot be applied to the hypothesis $h_S$ returned by the algorithm because $h_S$ is **not fixed** — it depends on the training sample $S$, and $\mathbb{E}[\widehat{R}(h_S)]$ is not a simple quantity. Instead we need a bound that holds **simultaneously for all** $h \in H$: a **uniform convergence bound**.

### Generalization Bound — Finite H (Inconsistent Case)

> **Context:** Applying the union bound over all hypotheses in $H$ yields a uniform convergence bound valid even when no hypothesis is consistent. The cost is a slower convergence rate than in the consistent case.

> [!theorem] Inconsistent Case Bound
> 
> Let $H$ be a finite hypothesis set. Then for any $\delta > 0$, with probability at least $1 - \delta$:
> 
> $$\forall h \in H, \quad R(h) \leq \widehat{R}_S(h) + \sqrt{\frac{\log|H| + \log\frac{2}{\delta}}{2m}}$$
> 
> - Equivalently $R(h) \leq \widehat{R}_S(h) + O\big(\sqrt{\log|H| / m}\big)$.
>     
> - The error bound is in $O(1/\sqrt{m})$ — **quadratically worse** than the consistent case.
>     

### Occam's Razor

> **Context:** The dependence of the generalization bound on $\log|H|$ formalizes a preference for simpler hypothesis sets, echoing the classical principle that the simplest explanation should be preferred.

> [!definition] Occam's Razor
> 
> "Plurality should not be posited without necessity" — the simplest explanation is best. To minimize true error, choose the most **parsimonious** explanation (smallest $|H|$). **Kolmogorov complexity** is the corresponding information-theory framework.

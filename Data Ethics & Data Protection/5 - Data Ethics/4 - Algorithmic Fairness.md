## 1. Classification Criteria
- Accuracy of a classifier is defined as $P(Y = R)$
- When classification is binary, it is possible to use the notation of conditional probability: $$P(\text{classification } | \text{ condition}) = \frac{P(\text{classification } ∩ \text{ condition})}{P(\text{condition})}$$

| Classification | Condition | Notion            |
| -------------- | --------- | ----------------- |
| $R = 1$        | $Y = 1$   | TP                |
| $R = 0$        | $Y = 1$   | FN (Error type 2) |
| $R = 1$        | $Y = 0$   | FP (Error type 1) |
| $R = 0$        | $Y = 0$   | TN                |

---
## 2.  Fairness Criteria
### Independence
The probability of belonging to $R=1$ has to be the same if the subjects belongs to group $a$ or $b$.
$$𝑅 ⊥ 𝐴$$
$$ℙ \{𝑅 = 1 | 𝐴 = 𝑎\} = ℙ \{𝑅 = 1 | 𝐴 = 𝑏\}$$
Relaxed:
$$\frac{ℙ \{𝑅 = 1 | 𝐴 = 𝑎\}}{ℙ \{𝑅 = 1 | 𝐴 = 𝑏\}} \ge 1 - \epsilon$$
![[4 - Algorithmic Fairness-1781904720170.webp|281]]

Example 1 - Independence NOT respected:
![[4 - Algorithmic Fairness-1781904844544.webp|525]]

Example 2 - Independence respected:
![[4 - Algorithmic Fairness-1781904992205.webp|531]]

**Achieving Independence:**
- **Pre-processing:** Adjust the feature space to be uncorrelated with the sensitive attribute
- **At training time:** Work the constraint into the optimization process that constructs a classifier from training data.
- **Post-processing:** Adjust a learned classifier so as to be uncorrelated with the sensitive attribute.

**Pros:**
- It can be applied at every stage of the process
**Cons:**
- It ignores the possible correlation between $Y$ and $A$
- It allows to have good classifications in one group and random classifications in another

### Separation
We have separation when the True Positive Rate and the False Positive Rate is equal for the two groups $a$ and $b$.
In other words, the algorithm has to success and fails at the same probability for each class.
$$R \perp A \mid Y
$$
$$
\mathbb{P}(R = 1 \mid Y = 1, A = a) = \mathbb{P}(R = 1 \mid Y = 1, A = b)
$$
$$
\mathbb{P}(R = 1 \mid Y = 0, A = a) = \mathbb{P}(R = 1 \mid Y = 0, A = b)$$
Summary of the possible cases:
![[4 - Algorithmic Fairness-1781905955935.webp]]
- Ex 1: Separation NOT respected
- Ex 2: Separation PARTLY respected
- Ex 3: Separation respected

How to obtain separation:
- **Training:** via ad-hoc optimization
- **Post-elaboration:** it is verified whether the intersection of the ROC curves of each group occurs
![[4 - Algorithmic Fairness-1781906074085.webp|482]]**Pros:**
- It is compatible with $R=Y$
- It incentives to reduce errors uniformly in all groups
**Cons:**
- more difficult to apply
- Does not take into account false negative rate

### Sufficiency
Unlike the previous two, it watches the results after the prediction. The percentage of people really True ($Y = 1$) and the percentage really False ($Y = 0$) has to be the same for the 2 classes.
$$Y \perp A \mid R$$

$$\mathbb{P}\{Y = 1 \mid R = r, A = a\} = \mathbb{P}\{Y = 1 \mid R = r, A = b\}$$

| Event | Condition | Notion                          |
| ----- | --------- | ------------------------------- |
| $Y=1$ | $R = 1$   | Positive predictive value (PPV) |
| $Y=1$ | $R = 0$   | False omission rate (FOR)       |
Binary case:
$$\mathbb{P}\{Y = 1 \mid R = 1, A = a\} = \mathbb{P}\{Y = 1 \mid R = 1, A = b\}$$
$$\mathbb{P}\{Y = 1 \mid R = 0, A = a\} = \mathbb{P}\{Y = 1 \mid R = 0, A = b\}$$
- First row $\rightarrow$ PPV
- Second row $\rightarrow$ FOR

### Calibration and Sufficiency
A score R is calibrated if $ℙ \{ 𝑌 = 1 | 𝑅 = 𝑟\} = 𝑟$.

For all instances with an $R$ score, there is a fraction $r$ of positive instances. Formally, for each set S: $ℙ \{ 𝑌 = 1 | 𝑅 = 𝑟, 𝑋 ∈ 𝑆 \} = 𝑟$.

Calibration is obtained from the latter equation:
$$\mathbb{P}\{Y = 1 \mid R = r, A = a\} = r, \quad \forall r, \forall a$$
![[4 - Algorithmic Fairness-1781907452561.webp]]
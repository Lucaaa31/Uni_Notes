> Computer systems that systematically and unfairly discriminate against certain individuals or groups of individuals in favor of others by denying an opportunity for a good or assigning an undesirable outcome to an individual or groups of individuals on grounds that are unreasonable or inappropriate.

## 1. Bias in ML Cycle
![[3 - Source of Bias-1781890079055.webp]]
- **Potential Space:** the global theoretical reality, all the possible aspects
- **Construct Space:** the way humans categorize the reality
- **Observed Space:** the effective data that have been measured, categorized and registered
- **Decision Space:** all the decisione taken by the model

### Construct Space
- $Y$: target variable
- $X$: feature variables
- The phenomenon of interest modeled by the relationship:
$$Y = f(X) + \epsilon$$
### Space of Observations
- $\tilde X = g(X)$
- $\tilde Y = h(Y)$
Data from observations: $$(\tilde x_1, \tilde y_1), (\tilde x_2, \tilde y_2),..., (\tilde x_n, \tilde y_n)$$
where:
- $\tilde x_i$ is an instance
- $\tilde y_i$ is the label
A feature that is a **protected attribute** is named 𝐴, they are the subject cited in Article 21.

### Predictions and Decisions
- $\hat f \sim f$ estimate of $f$
- $\tilde{R} = \tilde{f}(\tilde{X}, \tilde{Y})$: prediction $\tilde{R}$ made by the classifier trained on $\tilde{X}, \tilde{Y}$
- $D = d(\tilde{R})$: decision $D$ made with a decision rule $d$ applied on $\tilde{R}$

In certain applications, it might be decided that:
- $D = R$
- Or that a decision relies upon:
    - Additional context information $Q$
    - Considerations about a protected attribute $A$ (or more)
- In that case $\rightarrow D = d(R, A, Q)$
### Mapping to spaces
![[3 - Source of Bias-1781897725670.webp|240]]
- Construct Space
	- $Y = f(X, Y) + \epsilon$
- Observed Space
	- $\tilde X = g(X)$, $\tilde Y = h(Y)$
- A prediction $R$ made by the model trained on $\tilde X$
	- $R = \hat f(\tilde X)$
- A decision $D$ made with a decision rule $d$ applied on $R$, protected attribute $A$ and enviromental information $Q$













## 2. Graphical notations

### Variables

• 𝑅: classification
• 𝐴: sensitive characteristics/protected attribute
• 𝑌 : target
• 𝐶: capacity of an individual
	• Example: economic resources, properties, personal talents, skills, etc.
• $𝑃_c$ : proxy variable we have access instead of C (or any other variable)
	• E.g., university final grade is a proxy for skills
• 𝑄: additional context variables
	• they may or may not be relevant for the problem (i.e. impacting 𝑌 )
	• they may or may not be impacted either by 𝑅 or 𝐴,
	• e.g. the neighborhood where one lives in.
### Elements
- **Variables**: circles
	- **Grey circle:** variable employed in the model $\hat f$
	- **White circle:** otherwise
- **Dependence/correlation:** a connecting arrow

--- 
## 3. Bias from individuals/society to data
![[3 - Source of Bias-1781898897875.webp]]
### Historical Biases
Inequalities and disproportions in the world:
- 95% of 500 CEO are men
- different average income between men and women

It occurs when:
- A relevant capacity variable is dependent on a protected attribute
- the target is dependent on a protect attribute
![[3 - Source of Bias-1781899038532.webp]]

---
## 4. Bias from data to algorithm
![[3 - Source of Bias-1781899087256.webp]]
### Measurement bias
Similar to historical, but it does not involve the phenomenon itself.
Possible causes:
- Feature selection influenced by implicit mental models/subjective choise
- Although protected attributes are not used, their proxies are relevant

It occurs when:
- a proxy of some capacity relevant to the target is employed, and that proxy is dependent on some sensitive characteristics
- a proxy for the target is used, that is dependent on a protected attribute
- a proxy of a protected attribute is used, that is related to the target and/or the capacity
![[3 - Source of Bias-1781899240588.webp]]

Some examples:
- Using reporting of street crimes as proxy for criminality; street crimes are highly related to poverty and race
- Using zip code o predict socio-economic position; zip code correlated with ethnic group
- Using IQ (intelligence quotient) as proxy of intelligence; IQ is correlated to socio-economic status.

### Representation bias
Occurs when data are not representative of the actual population:
- Sampling method does not reach all $N$ equally
- Changes in the population not detected
Representation bias is sufficient to create discrimination.
![[3 - Source of Bias-1781899503984.webp]]

Some examples:
- Training with data only from few geographic locations
- Training with data from social networks (not all people are there)

### Omitted relevant variables bias
- It may occur when a variable relevant to the target/goal is omitted and not present in the collected data
- If the other variables present in the dataset have some dependence on protected attributes, the trained model will learn those dependencies and outcomes will be affected by spurious dependence on sensitive attributes
- Omission of a relevant variable alone cannot be a source of disparities, but it can amplify existing biases
![[3 - Source of Bias-1781899823612.webp|630]]
---
## 5. Bias from algorithm to individuals/society
![[3 - Source of Bias-1781899870338.webp|652]]
### Deployment bias
It arises when the model's predictions have harmful downstream consequences.
It can be seen as a bias going from algorithm to the society through decision makers.
![[3 - Source of Bias-1781899998729.webp]]

### Algorithmic bias
When algorithmic outcomes affect the behavior of people, exacerbating performance disparities on underrepresented groups:
- Aggregation and learning bias
- Evaluation bias
![[3 - Source of Bias-1781900098688.webp|280]]
### Aggregation bias
It arises when subgroups are so different that different ML models should be used instead of only one for everyone. It results in inconsistency in mapping inputs to labels, i.e., a different probability of receiving a label given some features.

Examples:
- Clinical aid tools might need different models for different groups of people
- Slangs in social network used by specific groups of people, but only one language model used
### Learning bias
It arises when algorithmic design choices are not equally suited for all subgroups.

Example:
- On web platforms, when later review rates are strongly influenced by previous ones, they should be treated separately

### Evaluation bias
It arises when
- evaluation data do not well represent the target population, or training and evaluation/operation data are very different
- model performance metrics used are poorly relevant for the relevant target population and the application context

Examples:
- facial recognition mostly trained on white face, then used/evaluated in contexts where other skin colors exist
- a model trained with purchases record on American supermarkets then evaluated with data from African supermarkets.
## 1. Introduction and Definitions
### Decision
- **Decision:** commitment to a course of action
- Choice: type of decision that involves to choose an option
- Judgment: a broader concept that involves reflective consideration of situational factors
### Automated Decision Making Systems
A System that automates decision through choice or predictions. They are always a combination of the following social and technological parts:
- Decision-making model
- Algorithm that translate the model in software code
- Datasets used for train and evaluate the model
- The whole of the political and economic ecosystems that ADM systems are embedded in

Some characteristics of AI based ADM systems:
1. They uses machine learning
2. predicts future outcomes or classifies people
3. makes decision based on these outcomes
![[1 - Automatic Decision Making-1781877290357.webp|443]]
### Expectations on ADM w.r.t. human decisions
- High accuracy in predicting outcomes
- Fairness across individuals
- Efficiency gains by reducing time spent by human decision-makers

### Data and ADM systems
ADM systems learn from historical series or example and make prediction based on them so, in order to learn correctly, they need:
- a sufficiently **large number** of examples
- a sufficiently **heterogeneous** set of examples
- examples annotated with the **"right answers"**

---
## 2. Types of Inferences
### Deductive
In deductive inferences, what is inferred is necessarily true if the premises from which it is inferred are true.
In other words, the truth of the premises guarantees the truth of the conclusion

_Example:_
 - All As are Bs
 - a is an As
 $$\rightarrow \text{a is a B}$$

### Inductive
Inductive inferences are based purely on statistical data, such as observed frequencies of occurrences of a particular feature in a given population:

_Example:_
- 96% of the Flemish college students speak both Dutch and French
- Louise is a Flemish college student
$$\rightarrow \text{Louise speaks both Dutch and French}$$
### Abductive
In abduction there is an implicit or explicit appeal to explanatory considerations there may also be an appeal to frequencies or statistics.

_Example:_
- I observed many gray elephants and no non-gray ones
- The best explanation for why I have observed so many gray elephants and no non-gray ones is that all elephants are gray
$$\rightarrow \text{all elephants are gray}$$
### Details on Inductive and Abductive
They are:
- **Non necessary**
	- Unlike deductions, they are not 100% true, they are probable
	- In the examples before we do not know if Louise speaks that languages and we know for sure that there are elephants that are not gray
- **Ampliative**
	- The conclusion goes beyond what is contained in the premises
	- For example, we have watched 100 elephants and says that all of them are gray, all the elephants >>> 100 elephants
- **Non-monotone**
	- New informations can override previous predictions
	- For example, if we find an elephant that is white we have to change our abductance
### AI as an inductive process
![[1 - Automatic Decision Making-1781880404194.webp|480]]
When modelling human characteristics and behaviors, there are some potential problems in the use of historical series:
- Reality is a super-set of what is measurable
	- Some aspects of our life are measurable only indirectly
- Spurious correlations and confounding factors
- Societies have historical and structural inequalities, reflected by the data

ADM systems inherit these problems.
### Example of these problems
Amazon analyzes historical data of online purchases on its platform and demographic data to determine in which neighborhoods to activate the fast delivery service:
![[1 - Automatic Decision Making-1781880694795.webp]]
In many cities, white residents were twice as likely as black residents to live in a neighborhood where service was offered.

**Consequences:** some neighborhoods even if they were in the center part of the city were excluded, and the ones with the whites, that were close to the excluded ones resulted positives.

After the publication of the study, the company extended the service to many of the districts that did not have it, in the cities mentioned by the study.

## 3. EU Charter of Fundamental Rights

### Article 21 - Non-discrimination
1. Any discrimination based on any ground such as sex, race, colour, ethnic or social origin, genetic features, language, religion or belief, political or any other opinion, membership of a national minority, property, birth, disability, age or sexual orientation shall be prohibited.
2. Within the scope of application of the Treaties and without prejudice to any of their specific provisions, any discrimination on grounds of nationality shall be prohibited.
![[1 - Automatic Decision Making-1781882494713.webp|473]]
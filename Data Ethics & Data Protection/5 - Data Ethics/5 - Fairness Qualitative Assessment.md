## 1. Introduction
- **What:** identify issues of fairness
- **When:** design phase, but also during and after deployment
- **Who by:** creator of the algorithmic system, researchers, students etc.
![[5 - Fairness Qualitative Assessment-1781944311441.webp|408]]

### Questions that we have to do
1. What problems can arise from wrong operations of the system?
2. Which stakeholders are adversely affected by these problems? How?
3. What values/social interests are at stake for those stakeholders?
4. How could the value/interest conflict be (fairly) resolved?

---
## 2. Example - Using the police crime data to improve safety of Edinburgh
### 1. What problems can arise from wrong operations of the system?

We use the Taylor-Russel diagram, also known as Confusion matrix:

| **Event**                              | **Prediction: Low risk of assault**               | **Prediction: High risk of assault**              |
| -------------------------------------- | ------------------------------------------------- | ------------------------------------------------- |
| **Criminal intention or circumstance** | Dangerous area/circumstance misclassified as safe | Danger avoided                                    |
| **No assault**                         | Safe area/circumstance correctly labelled         | Safe area/circumstance misclassified as dangerous |

### 2. Which stakeholders are adversely affected by these problems? How?
#### Stakeholders' identifications:
- **Direct stakeholder (users)**:
	- people who interact directly with the tool
- **Indirect stakeholders**:
	- persons who do not use the technology under consideration but they are impacted by it
#### Stakeholders - safer route
- **Direct stakeholder (users)**:
	- people new in the city
	- women
	- The owners of the app
- **Indirect stakeholders**:
	- Residents living in the areas covered by the app
	- Business in the area
#### In the Confusion Matrix
- **False positives**
	- **Business owners:** loss of customers, reputation damage
	- **Residents:** loss economical values for properties, reputation damage
- **False negatives**
	- **The users of the app** suffer assaults in an area labeled as safe

---
### 3. What values/social interests are at stake for those stakeholders?
#### Values
The principles or standards of a person (or society), the personal (or societal) judgement of what is valuable and important in life.

Properties:
- often related to ethics and morality
- hierarchy of values constitute what we are/want to be
- impossible to avoid conflicting values between individuals and social groups

Two aspects:
- **Genericity:** values are generic and can be instantiated in a wide range of concrete situations (e.g. eating well and exercising all contribute to the abstract goal of "health")
- **Comparison:** values allow comparison of different situations with respect to that value (e.g. according to the value "health" you should eat salad over pizza)

In Value Sensitive Design, most concern is on:
- Human well-being
- Human dignity
- (Social) Justice
- Welfare
- Human rights

### Values & software
Values play a role even in the design of a application:
- Explicitly supported
	- can be in the form of design constraints or even formal requirements
- Designer values
	- personal or professional values that a designer/developer/software architect brings into the design of a tool;
	- not necessarily aligned with/supported by an explicitly supported value
- Stakeholder values
	- What matters for specific stakeholders / stakeholders’ groups
	- They might by explicitly asked or elicited indirectly (e.g., values scenarios)

### Matrix Confusion
Now we map the value:
- Economic and reputational security
	- Residents/business owners of areas labelled as dangerous
- Personal safety
	- Women, especially young
	- Tourists or new residents

---
## 4. How could the value/interest conflicts be (fairly) resolved?
### How to solve value tensions?
Some examples can be:
- Solicit public feedback since design phase
- Enable software errors reports and reconfigure software during operation
- Value-based prioritization
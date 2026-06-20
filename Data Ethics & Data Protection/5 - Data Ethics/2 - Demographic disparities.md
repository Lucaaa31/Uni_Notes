## 1. The Machine Learning Loop
![[2 - Demographic disparities-1781882904078.webp|562]]
- **Measurement:** made by the State of World in order to obtain information and transform them in Data
- **Learning:** with these Data we train our Model
- **Action:** we use out model to make decision on individuals
- **Feedback:** individuals reacts to the decision and they can change the state of the world
### State of the world -> Demographic disparities
Disproportions and inequalities are common in our societies, e.g.:
- Certain groups of individuals may concentrate in specific neighborhoods of a city, or of a geographical region
- Services to citizens and industries are unevenly distributed
- Etc.
### Matrix of Domination/Oppression
![[2 - Demographic disparities-1781885097184.webp|386]]
- Dominant $\rightarrow$ inside circle
- Oppressed $\rightarrow$ outside circle
---
## 2. The measurement process
Data are often interpreted as **objective**. However, data about society and individuals is the result of a measurement process, in which many **subjective** choices have to be made.
The measurement process is the empirical process of assigning numerical values to an entity, with the purpose of characterizing a specific attribute.

### Example
The management of a company decides to adopt an automatic system for identifying the 10 most productive developers in the last year, and reward them. 
The following choices are made:
- The company code repository is taken into consideration
- Daily presence of staff in the company
- Productivity were measured both in terms of source code committed and in terms of fixed defects
- The final choice is made on a unique indicator of productivity
![[2 - Demographic disparities-1781885727187.webp|430]]![[2 - Demographic disparities-1781885748932.webp|427]]
Some considerations has to be done:
- Some programming languages are more "verbose" than others
- The indicator does not take into account the time spent on: documentations, debugging etc.
- Moving from a rational to an ordinal scale eliminates distances between positions
- Developers will be pushed to write a lot more lines of code that can distort the social process that we are monitoring
### Follow up of the example - Simpson's paradox
The following year the management of the company decides to reward groups of developers rather than individually. The average productivity is used, and two managers are commissioned to perform the calculation.

The two managers use two different measurement methods for fixed defects:
1. defects fixed/hour
2. hours worked to fix a defect (reciprocal)
![[2 - Demographic disparities-1781886179483.webp|418]]
- Manager 1 will reward group 1
- Manager 2 will reward group 2
Both used the same data.

> According to Simpson’s paradox, a trend, association, or characteristic observed in underlying subgroups may be quite different from association or characteristic observed when these subgroups are aggregated.

---
## 3. Learning Process
Models can propagate demographic disparities in the data.

_Example 1:_
“If the admission models to American universities had been trained on the basis of data from the 1960s, we would probably now have very few women enrolled, because the models would have been trained to recognize successful white males.

_Example 2:_
If you search "Nurse" on Google Images most of the images will be of women.
If you search "CEO" on Google Images most of the images will be of men.

---
## 4.  Predictive policing (Action and Feedback)
### Preliminary Considerations
- The characteristics and behavior of individuals change over time
- Only certain crimes can be easily mapped
- The output of an algorithm can have an effect on the individual, which tends to confirm or contrast the action

### Predictive policing
a. Number of drug arrests made by Oakland Police Department, 2010:
1. West Oakland
2. International Boulevard
![[2 - Demographic disparities-1781887166772.webp|337]]

b) Estimated number of drug users, on the basis of the 2011 national consumption survey of drugs and the state of health:
![[2 - Demographic disparities-1781887248807.webp|399]]

a) Number of days of targeted patrols for drug offences in the areas reported by the analysis of Oakland P.D. data.


![[2 - Demographic disparities-1781887372539.webp|261]]
a) Number of days of targeted patrols for drug offences in the areas reported by the analysis of Oakland P.D. data.

b) Targeted drug offense patrols, by skin color.

c) Estimation of drug use by skin color, from the 2011 survey














### Algorithm
Prediction of the probability of drug offences in designated areas of the city by the PredPol algorithm.

The ’baseline’ indicates the original Oakland P.D. data. The ’add 20%’ line simulates the effect of additional crimes observed in places indicated by the algorithm.

This is the estimated effect of the feedback loop, which **reinforces the distortion in the original data**.
![[2 - Demographic disparities-1781887673428.webp|441]]
**Problem:** the algorithm says to a cop to go to inspects a place full of black people and he arrests some people and boosts the data reinforcing the distortion.


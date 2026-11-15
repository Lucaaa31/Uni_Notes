The scope of a simulation is to evaluate the system performance.
The key activity of our simulation is to **collect samples** needed for the evaluation of our indexes.

# Averages
---
We need to measure different types of averages:
- **Point averages:** Average of individual variables
	- e.g. average delay in the queue per customer or average service time
- **Time averages:** Average of continuous-time variables
	- average number of customers
- **Statistical frequencies:** How often a given condition is observed
	- e.g. variance, standard deviation, quantiles

## Measuring Averages
Measuring averages requires a lot of attention, we have two possibilities:
1. collect all the samples we are interested in a file, then leave statistical analysis to an external dedicated program
2. perform inside the program the needed mathematical operations to produce the desired outputs

## Point Averages
They are used for quantities that are sampled at a discrete time instants; i.e., for which we have a finite number of samples
$$
\hat x = \frac{1}{N} \sum^N_{i = 1} x_i
$$
![[04 - Measuring Performance-1794506192278.webp|498]]

In code:
```python
# We accumulate the samples x_i and the no. of samples
while(...):
	total += sample
	nr_samples++

# At the end of the simulation we compute
mean = total/nr_samples
```

In a similar way we can compute the variance using the estimator:
$$
\hat{\sigma}^2 = \frac{1}{N-1}\left(\sum_{i=1}^{N} x_i^2 - N \cdot \hat{x}^2\right)
$$
## Time Averages
They are used for quantities that take a value in any (continuous) time instant.
Both the value and the duration of time for which that value holds have to be taken into account.
$$
\hat{\mu}_x = \lim_{T \to \infty} \frac{1}{T} \int_{0}^{T} x(t)\,dt
$$
![[04 - Measuring Performance-1794506457397.webp|564]]

In code:
```python
while(...):
	# time between the events
	delta_time = current_time – last_time
	
	# update the integral
	total_area += old_value*delta_time
	
	#  needed at next event
	old_value = current_value
	
	last_time = current_time
	
# At the end of the simulation we compute
mean = total_area / final_time

```

### Poisson Point Processes
Time averages can be obtained also exploiting properties of Poisson Point Processes:
	Sequence $\{T_n\}_n$ forms a PPP if $X_n =T_{n+1}-T_n$ are i.i.d and exponentially distributed

It can be proved that:
$$
\hat{\mu}_{\mathrm{x}} = \lim_{T \to \infty} \frac{1}{T} \int_{0}^{T} x(t)\,dt = \lim_{n \to \infty} \frac{\sum_{1}^{n} x(Ti)}{n}
$$
provided that $\{T_n\}_n$ is independent from system dynamics.

## Statistical Frequency
We use them to evaluate probabilities of some specific situation to occur.

We distinguish between:
- **Discrete-time variables**
- **Continuous-time variables** 

### Discrete-time variables
We relate the number of occurrences of the situation of interest with the total number of potential occurrences.
$$
\hat p_i = \frac{N_i}{N}
$$
where:
- $N_i$ number of times that a situation occurred
- $N$ number of times that the situation could have occurred

In code:
```python
# During the simulation
while(...):
	if (condition):
		nr_good += 1
	
	nr_samples += 1

# At the end of the simulation
prob = nr_good / nr_samples 

```

### Continuous-time variables
We relate the duration of time for which a specific situation occurs over the total time the situation could have occurred.
![[04 - Measuring Performance-1794735502397.webp|584]]

In code:
```python
while(...):
	if (condition):
		delta_time = current_time – last_time # time between events

		total_time += delta_time # update the total time

		last_time = current_time

# At the end of the simulation
mean = total_time / final_time

```


## Histograms and Quantiles
They are derived similarly to probabilities discussed above by defining:
> How many ranges to split the possible values into, and how wide each range is.

![[04 - Measuring Performance-1794737842024.webp]]

## Caveat
Our computations are, at the moment, only an unreliable estimates of the quantities we want to measure.
We are not considering:
- the effect of the initial conditions
- the effect of the transient
- the fact we are measuring a single specific realization of the stochastic process representing the simulated system

## Initialization
Before entering in the Main Loop we must:
- initialize all the variable used to store our measures
- initialize the data structures needed for the simulation
- assign a value to all the simulation parameters, possibly through user inputs
- bootstrap the simulation, scheduling the first few events

## Termination
At the end of the Event Loop, before exiting the program, we must inform the users of the simulation results.

If the simulation ended regularly, we print (on the screen and/or on a file) all the measures collected and any information it is important to report.

If the simulation ended anomalously, we will explicitly report this fact, to avoid the possible usage of meaningless data.


## Simulation Campaign
A run of the program corresponds to a single realization of the stochastic process representing the
simulated system, but this is not enough to study the system.
> So we plan an **execution campaign**.

### Executing a Campaign
To execute a simulation campaign we should:
1. distinguish the input parameters between:
	- **fixed parameters**, whose influence is not a subject of interest in the specific campaign
	- **varying parameters**, whose effect on the system performance indexes is the subject of simulation
2. identify the output parameters whose variation we are interested in
3. execute simulation runs for each meaningful combination of the varying input parameters
4. aggregate and represent the output data in a way that explicates their dependence on the varying input parameters

## Wrap-up
- Results of a simulation are statistical measures of the performance of the system, obtained through the stochastic processes represented by the simulation model

- Measuring performance requires to collect statistics about the values taken by state variables

- The way we collect statistics depends on the kind of variables
	- Discrete-time variables: sampled at discrete time instants
	- Continuous-time variables: that take a values in any instant
	- Frequencies of particular conditions/situations


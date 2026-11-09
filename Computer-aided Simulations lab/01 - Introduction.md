>Simulating a **dynamic stochastic system** involves replicating its behavior over time.

Imitating the system behaviour mean **artificially reconstruct its history and evolution**, this allows us to extract valuable insights of the system.

### Properties
We may be interested in discovering or verifying system:
- **Performance:** how well does it works?
- **Correctness:** is it working as desired?
- **Capabilities:** can it reach some objectives?
- **Reliability:** under which conditions is it working as desired?
- ...

## Simulation Model
In order to simulate we need to develop a simulation model, that is a **formal description of how the system works**.

It is made by a set of assumptions on the system and its behavior and it describes a dynamic system as a set of components interacting according to specific rules.

### State Variables
> They describes the system and its components with their value.

To describe the system evolution we need to describe the rules according to which the system changes its own state.
To imitate the system and recreate its history means to rebuild the sequence of states visited by the system.

### Deriving the model
A simulation model is a formal representation of a system derived from the **theoretical knowledge** of the system or from empirical observations.

We need to identify the boundary between the system and its environment.

### Definitions
- **State:** describes the system behavior in any time instant
- **Entities:** objects of interest in the system
- **Attributes:** properties of an entity
Actions:
- **Activity:** a time period dedicated to a specific operation
- **Event:** an instantaneous occurrence that changes the system state
Actions can be:
- **Endogenous:** events and activities internal to the system
- **Exogenous:** events and activities in the environment that affect the system

### Types of models
They can be classified as:
- **Static:** the system behavior does not change in time
- **Dynamic:** the system behavior changes in time

Dynamic models can also be classified as:
- **Discrete:** the system changes only in a discrete set of points in time
- **Continuous:** he system state changes continuously in time
![[Introduction-1793728866947.webp|567]]Models can also be:
- **Deterministic:** the model does not contain random variables, therefore a set of input values produces a unique set of outputs
- **Stochastic:** inputs are represented by random variables, so the outputs are random processes
	- The simulation output analysis requires the application of statistic techniques on results collected in several simulation runs

## Analytical model
> It is an alternative to simulation for simple systems or under special hypothesis. We can obtain indications on the system performance through mathematical expressions.
> **Examples:** Physics laws, differential equations, Markov chains etc.

They can have:
- **Exact solution:** the solution is represented by a closed form expression, $x = f(y, z, n)$
- **Numerical solution:** due to its complexity, the solution can be obtained only though numerical approximation: $x=f(y,z,n) \qquad y=g(z,n) \qquad z=(x,y,n)$


## Analytical vs. simulation models
Analytical models
- Emphasize the cause-effect relations between inputs and outputs
- Limited solution cost (in time and computational power)
- Help the system comprehension
- Fewer ‘technical’ errors (bugs)

Simulation models
- Better adherence the actual system
- Easier to introduce incremental improvements in the model
- Easier to modify some to the underlying model hypothesis
- Simpler model construction




The basic elements of a discrete-event simulator are:
- **Event Loop**, the main cycle for event analysis
- **Future-Event Set**
- **Functions** to be executed when each event happens

To these we can add:
- the procedures for data collection and measurement analysis
- the procedures for the initialization of the simulator
- the termination criteria for the simulation

# Fundamental Terms
---
## Simulation Time
It is the representation of time in the real system.
In the model time can be:
- Continuous
- Discrete

### Continuous time
Represented in a **floating-point** format and it is the nearest one to the real nature of time.
**The probability for two events to happen exactly at the same time is zero.**
We do not need to model simultaneous events.

### Discrete time
Represented in **integer** format, it is the most suited for those system where time evolves naturally in steps.
**The probability for more than one event to happen at the same discrete time may be not negligible.**
We need to define an order criterion between contemporaneous events.

### System clock (current time)
Independently from the representation format, we need a global indicator of the time evolution.
It must be accessible from any point of the simulator, representing the current value of the simulation time.



## Events
Each event must be represented in our program using a composite data type, whose nature
depends on the implementation choice for the Future-Event Set.

### Data structure
Each event contains at least:
- the time at which the event is scheduled
- an identifier for the type of event

The **event scheduling time is the key field**, since it is needed to decide which is the next event to be executed, i.e., the one with the smallest scheduling time in the FES.

The **event type** is needed to identify which kind of event must happen, and, hence, which **specific function should be executed**.

```python
# the simulation time
time = 0

# the list of events in the form: (time, type)
FES = PriorityQueue()

# schedule the first arrival at t=0
FES.put((0, "arrival"))
```

It is useful to **associate attributes to each event**: this allows us to aggregate events according to their general type and discriminate them at execution time.



## Event Loop
It is a loop in which:
- the next event is extracted from the FES
- the current time is advanced to the scheduling time of the event
- depending on the event type, the associated procedure is executed

```python
# the simulation time
time = 0

# the list of events in the form: (time, type)
FES = PriorityQueue()

# schedule the first arrival at t=0
FES.put((0, "arrival"))

while time < SIM_TIME:
	(time, event_type) = FES.get()

	if event_type == "arrival":
		arrival(time, FES, queue)
	elif event_type == "departure":
		departure(time, FES, queue)
```

The Event Loop is executed until the termination condition is reached:
- The FES is empty
- The maximum simulation time has been reached
- The maximum number of events has been reached
- The precision conditions on the measurements have been satisfied
- An anomalous condition has been reached

## Future-Event Set
The Future-Event Set is the key structure of a discrete-event simulator.
The efficiency of the simulator depends on its implementation.
We need to consider the trade-off between complexity and execution efficiency.

> The Future-Event Set is the set of all the events scheduled for future execution

From this set we extract each time the one with the minimum scheduling time.
The selected data structure must support in the most efficient way the common operations for the FES:
- Insertion of a new event
- Extraction of the next event
- Lookup and deletion of an event

The management efficiency depends a lot on the specific conditions of the simulator:
- The sequence of the insertion and deletion operations
- The average number of events in the FES

### Example
```python
# state variable: no. of clients in the queue
users = 0

# *********************************************
# Client
# *********************************************

class Client:

def __init__(self,type,arrival_time):
self.type = type
self.arrival_time = arrival_time


def arrival(time, FES, queue):
	global users
	
	# sample the time until the next event
	inter_arrival = random.expovariate(1.0/ARRIVAL)
	
	# schedule the next arrival
	FES.put((time + inter_arrival, "arrival"))
	
	# Now manage the event; update state variable
	users += 1
	
	# create a record for the client
	client = Client(TYPE1,time)
	
	# insert the record in the queue
	queue.append(client)
	
	# if the server is idle start the service
	if users==1:
		# sample the service time
		service_time = random.expovariate(1.0/SERVICE)
		# schedule when the client will finish the server
		FES.put((time + service_time, "departure"))
		
		
		
def departure(time, FES, queue):
	global users
	
	# get the first element from the queue
	client = queue.pop()
	
	# Update state variable
	users-= 1
	
	# see whether there are more clients to in the line
	if users >0:
	
		# sample the service time
		service_time = random.expovariate(1.0/SERVICE)
		
		# schedule when the client will finish the server
		FES.put((time + service_time, "departure"))

```

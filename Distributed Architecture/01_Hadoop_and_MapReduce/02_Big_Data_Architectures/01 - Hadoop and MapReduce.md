# Apache Hadoop

Apache Hadoop is a scalable fault-tolerant distributed system for Big Data, here's some properties:
-   Distributed Data Storage
-   Distributed Data Processing
-   Borrowed concepts from the systems designed at Google
## Main Components

Hadoop has two core components:
-   **Distributed Big Data Processing Infrastructure based on the MapReduce programming paradigm**
    -   Provides high-level abstraction view, so that programmers do not need to care about scheduling and synchronization
    -   Fault-tolerant, it automatic manages node and task failures
-   **Hadoop Distributed File System (HDFS)**
    -   High availability distributed storage
    -   Fault tolerant

![[02 - Hadoop and MapReduce-1778183392503.webp ]](Example of the Hadoop components with two of replicas per chunk)

In this image the $C$ represents the **chucks**, they are units that the HDFS uses to divide large files.

---
## Distributed Big Data Processing Infrastructure
Hadoop programs, as already said, are based on the MapReduce programming paradigm. The MapReduce framework automatic handles the distributed problems like scheduling and synchronization. But it is important to know how it works in order to develop efficient applications.
## Scalability
We can define scalability along two dimensions:
-   **In terms of data:** Given twice the amount of data, the word count algorithm takes approximately no more than twice as long to run
-   **In terms of resources:** Given twice the number of servers, the word count algorithm takes approximately no more than half as long to run

For this example we have excluded the time needed to send local result to the node in charge of computing the final result, but in real time applications it has to be considered.
## Properties
These are the MapReduce-approach key ideas:
-   **Scale out, not up:** Increase the number of servers, instead of upgrading the resources
-   **Data locality:** Move the process (the code, the algorithm) to the data, because the network bandwidth is lesser then the physical memory
-   **Process data sequentially**, because the seek operations are expensive and usually the big data applications require to analyze all the records, so random access is useless

So, Hadoop/MapReduce is designed for:
-   Batch processing involving (mostly) full scans of the input data
-   Data-intensive applications:
    -   Read and process the whole Web
    -   Read and process the whole Social Graph
    -   Log analysis
In the other hand, Hadoop/MapReduce does not feet well with:
-   Iterative problems
-   Recursive problems
-   Stream data processing
-   Real-time processing
# The MapReduce Programming Paradigm
The MapReduce programming paradigm is based on the basic concepts of
Functional programming. Everything is based on two function with
predefined signatures, Map and Reduce:

-   **Map function:** it is applied over each element of the input and produces a set of **(key, value) pairs**. It can be viewed as a **transformation** and it is defined by the developer

-   **Reduce function:** it is applied to each pairs, produced by the Map, with the same key and produces a set of (key, value) pairs that are our final result. It can be seen as an **aggregate operation**, defined by the developer

**N.B.:** the map reduce is applied **isolated** for each elements, it does not have memories of the elements processed before
## Word count running example
Here's an example applied to the Word count example seen before, the
input is a list of words:
![[02 - Hadoop and MapReduce-1778183955116.webp]]

We can distinguish **three phases**:
-   **Map phase:** we apply a function on **each element**, in this case we produce set $<k, v>$ where $k$ is a word and $v$ is $1$, because it is applied isolated for each element

-   **Shuffle and Sort phase:** we group by key

-   **Reduce phase:** we apply a function to each group, in this case we sum the values

**N.B.:** the shuffle and sort phase is always the same, it is provided by the Hadoop system.
## Formal definition of Map and Reduces functions
The map and reduce functions are formally defined as follows:
-   map: $(k_1, v_1) \rightarrow [(k_2, v_2)]$
-   reduce: $(k_2, [v_2]) \rightarrow [(k_3, v_3)]$
So:
-   The map function **returns a list**
-   The reduce function is **invoked once for each distinct key**,
    receives the **list of values** of the key and **returns a list of
    keys-values**
## Pseudocode
```java
map(key, value):
  // key: offset of the word in the file
  // value: a word of the input document
  emit(value, 1)

reduce(key, values):
  // key: a word; values: a list of integers
  occurrences = 0
  for each c in values:
    occurrences = occurrences + c
  emit(key, occurrences)
```







## Sharing parameters among Driver, Mappers and Reducers
The **configuration object** is used to share the configuration of the Hadoop environment across the driver, the mappers and the reducers of the application/job:

-   It stores a list of **(property-name, property-value)** pairs
-   These pairs are **specified in the Driver**
-   They are used for sharing some parameters with Mappers and Reducers

They are very useful for sharing **constant properties** that are only available during the execution of the program:

-   The Driver set them
-   The Mappers and Reducers can read but not modify them

### In The Program

In the Driver we have to do these things:
1.  Retrieve the configuration object: `Configuration conf = this.getConf();`
2.  Set personalized properties: `conf.set("property-name", "value");`
3.  Then in the Mapper and the Reducer: `context.getConfiguration().get("property-name")`

# Counters

Hadoop provides a set of basic, built-in, counters to store some **statistics** about jobs, mappers, reducers. Also, we can define personalized counters to compute the statistics that we want.

## User-defined Counters
They are defined using Java **Enum**, they are incremented in the Mappers and in the Reducers. The value is printed by the Driver at the end of the job.

For increment the counter we do: `context.getCounter(countername).increment(value);`. In the Driver we use the methods `getCounters()` and `findCounter()` methods for retrieve the final values of the Counters.

User-defined counters can be defined runtime by using the method `incrCounter(<group name>, <counter name>, <value>)`.

## Example of User-defined Counters
Here's an example of how implement them:
-   In the Driver we define it
```java
public static enum COUNTERS {
    ERROR_COUNT,
    MISSING_FIELDS_RECORD_COUNT
}
```

-   For increment it in the Mapper or in the Reducer

```java
context.getCounter(COUNTERS.ERROR_COUNT).increment(1);
```

-   For retrieving the final value at the end of the execution of the
    application

```java
Counter errorCounter = job.getCounters().findCounter(COUNTERS.ERROR_COUNT);
```

# Map-only Job

In some applications all the work can be performed by the **Mappers**. Hadoop allows the execution of only the Map jobs:

-   The Reduce and the shuffle and sort phases are not performed

-   The output of the Mappers is directly stored in HDFS

In order to implement a Map-only job we will have to set the number of
reducers to zero:

```java
job.setNumReduceTasks(0);
```

# Setup and Cleanup

Mappers classes are characterized by a setup and a cleanup methods. They are empty if not overridden.
-   **Setup:**
    -   Called once for each mapper **before** the map() method
    -   Used for setup the attributes of the Mapper instance in order to preserve his state
    -   **N.B.:** the map() method is stateless
-   **Cleanup:**
    -   Called once for each mapper **after** the map() method
    -   It can be used to emit (key, value) pairs based on the values of the attributes of the instance
Also the Reducer class is characterized by a setup and a cleanup method.
## In-Mappers Combiners
The in-mapper variables are used to perform the work of the combiner in the mapper, it can allow improving the overall performance of the application but it uses a lot of main memory.
![[02 - Hadoop and MapReduce-1778185002904.webp|556]]

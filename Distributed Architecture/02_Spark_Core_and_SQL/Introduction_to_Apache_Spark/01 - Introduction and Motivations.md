Spark is a fast and general-purpose engine for large-scale data processing. 
His goals are:
- **Generality,** diverse workloads, operators, job sizes
- **Low latency
- **Fault tolerance,** faults are the norm, not the exception
- **Simplicity** 
## Motivations
### MapReduce and Iterative Jobs
When we performs iterative jobs we have to involve a lot of **disk I/O** for each iteration and stage, and this is **very slow** even if we are local.
![[Introduction and Motivations-1778323326538.webp]]
The solution for avoid this problem is to **keep more data in main memory**, that is the basic idea of Spark.
### Iterative Jobs
#### MapReduce
![[Introduction and Motivations-1778323480644.webp]]
#### Spark
![[Introduction and Motivations-1778323506931.webp]]

As we can see the data shared between the iterations are kept in the main memory, or at least a part of them. 
This is 10 to 100 times faster than disk.

###  Multiple Analyses of the Same Data
### MapReduce
![[Introduction and Motivations-1778336177274.webp]]
#### Spark
![[Introduction and Motivations-1778336199324.webp]]
As we can see, in Spark we read the data only once from the HDFS and than store them in the main memory of each server.

### MapReduce vs Spark

|                          | **MapReduce**  | **Spark**                      |
| ------------------------ | -------------- | ------------------------------ |
| **Storage**                  | Disk only      | In-memory or on disk           |
| **Operations**               | Map and Reduce | Map, Reduce, Join, Sample,etc… |
| **Execution model**          | Batch          | Batch,interactive, streaming   |
| **Programming environments** | Java           | Scala, Java, Python, and R     |
### Two iterative Machine Learning algorithms
![[Introduction and Motivations-1778336907105.webp]]

# TEMP 


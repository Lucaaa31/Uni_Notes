## Basic Idea
The data are represented as RDDs, they are partitioned in a collection of objects and distributed across the nodes of a cluster:
- They are split in partitions
- Each node of the cluster that is running an application contains at least one partition of the RDDs
They allows executing in parallel the code invoked on them: each executor of a worker node runs the specified code on its partition of the RDD.
Here's an example of RDD split in 3 partitions
![[03 - Resilient Distributed Data sets-1778338441297.webp]]
They are immutable once constructed, so their content cannot be modified.
Spark tracks lineage information to efficiently recompute lost data:
- Spark knows, of each RDD, how it has been constructed and can rebuilt it if a failure occurs
- This information is represented by means of a Direct Acyclic Graph connecting input data and RDDs
RDDs are automatically rebuilt on machine failure.
## Creation of a RDDs
RDDs can be created:
- by parallelizing existing collections of the hosting programming language (Scala, Python, etc.)
	- \# of partitions = specified by the user
- from large files stored in HDFS
	- \# of partitions = 1 per HDFS block
- from files stored in many traditional file systems or databases
- by transforming an existing RDDs
	- \# of partitions = depends on the type of transformation

## Spark Framework
Spark programs are written in terms of **operations** on RDDs, they are of two types:
- Transformations
	- map, filter, join,...
- Actions
	- count, collect, save,...
What it does:
- Manages scheduling and synchronization of the jobs
- Manages the split of RDDs in partitions and allocates RDDs’ partitions in the nodes of the cluster
- Hides complexities of fault-tolerance and slow machines (RDDs are automatically rebuilt in case of machine failures)
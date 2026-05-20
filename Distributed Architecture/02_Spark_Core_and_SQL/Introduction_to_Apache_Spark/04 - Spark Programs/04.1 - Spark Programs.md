## Supported Languages
Spark supports many programming languages:
- Scala, used to develop the Spark framework and all its components
- Java
- Python
- R
We will use Python.
## Structure of Spark Programs
It is a Python program.
### Driver
It is the start of the application, it contains the **main()** method and it is responsible of defining:
- The local variables 
- The RDDs stored in the nodes of the cluster
It access Spark through the **SparkContext** object, that represents a connection to the cluster. It allows the Driver to:
- Create RDDs
- Submitting executors (processes) that execute in parallel specific operations on RDDs (Transformations and Action)
### Worker Nodes
The worker nodes of the cluster are used to run your application by means of executors. Each executor runs on its partition of the RDDs the operations that are specified in the driver.
![[04 - Spark Programs-1778340739275.webp]]
## Local Execution of Spark
Spark programs can also be executed locally:
- Local threads are used to parallelize the execution of the application on RDDs on a single PC
- It is useful to develop and test the applications before deploying them on the cluster
- A local scheduler is launched to run Spark programs locally
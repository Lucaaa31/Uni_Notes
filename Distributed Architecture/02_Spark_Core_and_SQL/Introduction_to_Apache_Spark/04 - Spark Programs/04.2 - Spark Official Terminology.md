## Application
User program built on Spark, it consists of a driver program and executors on the cluster.
## Driver program
The process running the main() function of the application and creating the SparkContext.
## Cluster manager
An external service for acquiring resources on the cluster (e.g. standalone manager, Mesos, YARN).
## Deploy mode
Distinguishes where the driver process runs:
- In **cluster mode**, the framework launches the driver inside of the cluster
- In **client mode**, the submitter launches the driver outside of the cluster
## Worker node
Any node of the cluster that can run application code in the cluster.
## Executor
A process launched for an application on a worker node, that runs tasks and keeps data in memory or disk storage across them, each application has its own one.
## Task
A **unit of work** that will be sent to one executor.
## Job
A parallel computation consisting of **multiple tasks** that gets spawned in response to a Spark action (e.g. save, collect).
## Stage
Each **job** gets divided into **smaller sets of tasks** called **stages**.
The output of one stage is the input of the next stage:
- The outputs of those stages is stored in HDFS or a database
The **shuffle operation** is always executed between two stages:
- Data must be grouped/repartitioned based on a grouping criteria that is different with respect to the one used in the previous stage
- It is a **heavy operation**
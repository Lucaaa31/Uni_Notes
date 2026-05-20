In the following image we can see the architecture of Spark:
- In blue there are the data analytics components
- In light-blue the core
- 
![[Spark Components-1778336996684.webp]]
## Spark Core
Spark is based on a **basic component** that offers to all the high-level data analytics components his functionalities, for example it exploits:
- Task scheduling
- Memory management
- Fault recovery
- etc.
It provides the APIs that are used to create RDDs and applies transformations and actions on them.
When the efficiency of the core component is increased also the efficiency of the other high-level components increases.
## Data analytics components
### Spark SQL structured data
This component is used to interact with structured datasets by means of the SQL language or specific querying APIs.
Other properties:
- it supports Hive Query Language
- it uses manu data sources (JSON, Parquet, etc.)
- it exploits a query optimizer engine

### Spark Streaming real-time
It is used to process live streams of data in real-time, his API are that operates on RDDs and are similar to the ones used to process standard RDDs associated with static data sources.


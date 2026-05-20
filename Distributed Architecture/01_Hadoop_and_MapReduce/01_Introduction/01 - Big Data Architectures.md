A big data architectures is designed to handle:
-   ingestion
-   processing
-   analysis
of data that are too large or complex for the traditional database
systems.

These solutions typically uses one or more of the following types of
workload:
-   **Batch preprocessing**
-   **Real-time processing**
-   **Interactive exploration**
-   **Predictive analytics and machine learning**

To use them we need to consider:
-   Store and process **data that are too big** for the traditional databases
-   Transform **unstructured data** for doing a report
-   Capture, process and analyze **unbounded streams of data** in real
    time
The most used architecture is the **Lambda Architecture**.
## Lambda Architectures
![[Big Data Architectures-1778182941667.webp]]

The Lambda Architecture is a **data-processing framework** designed to handle massive quantities of data by balancing **latency, throughput, and fault tolerance**.
It emerged as a solution to the inherent trade-offs of early big data tools:
-   **Batch Systems (e.g., Hadoop):** Capable of processing vast datasets through parallelization but characterized by **high latency**.
-   **NoSQL Databases (e.g., Cassandra):** Provide scalability and low latency but often impose **limited data models** and **lack human-fault tolerance** due to their mutable nature.
### Key Components
-   **Batch Layer:** Manages the master dataset (**an immutable, append-only system of record**) and precomputes comprehensive views. While accurate, it typically suffers from high latency.
-   **Speed Layer (Stream):** Processes real-time data to provide low-latency updates, compensating for the delay of the batch layer.
-   **Serving Layer:** Indexes and merges the outputs from both layers to respond to ad-hoc queries with the most up-to-date information.
### Requirements
A Lambda architecture has to be:
-   **Fault-tolerant** against hardware failures and human errors
-   **Support variety of use cases** such as low latency querying and updates
-   Linear **scale-out capabilities**
-   **Extensible**, in order to be manageable and easy to introduce new features
### Queries
Here's an example of query: $$\text{query = function(all data)}$$
Queries have some properties:
-   **Latency:** time it takes to run the query
-   **Timeliness:** how up-to-date the query is (**freshness** and **consistency**)
-   **Accuracy:** tradeoff between performance and scalability (**approximations**)
### Paths
Lambda architecture is based on **two paths**:
-   **Cold path (batch layer):**
    -   It stores all data in raw form and performs batch processing on the data
    -   The result of this process is stored as batch views
-   **Hot path (speed layer):**
    -   It analyzes data in real time
    -   Designed for having low latency, but it has low accuracy
### A more detailed view
![[Big Data Architectures-1778182923053.webp|545]]

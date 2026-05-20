Used to implement applications that produces summarized view of the data, they are of three types: 
- Numerical summarizations 
- Inverted index 
- Counting with counters 
## Numerical Summarizations 
**Goal:** group records by a key field(s)and calculate numerical aggregations per group. It provides a top-level view of large input dataset. 
**Motivation:** few high level statistics can be analyzed by domain expert to identify trends, anomaly etc.
### Structure
**Mappers**
- Output **(key, value) pairs** where
	- **key** is associated with the fields used to define groups
	- **value** is associated with the fields used to compute the aggregate statistics
**Reducers**
- Receive a set of numerical values for each **group-by key** and compute the final statistics for each **group**
**Combiners**
- If the computed statistic has **specific properties** (for example commutative or associative), combiners can be used to **speed up performances**

![[IMG-20260507213327251.webp]]

Known uses are:
- Word count
- Record count (per group)
- Min/Max/Count (per group)
- Avg/Med/Std deviation (per group)
----------------------------------
## Inverted Index Summarizations
**Goal:** build an index from the input data to support faster searches or data enrichment. It maps terms to a list of identifiers.
**Motivation:** improve search efficiency.
### Structure
**Mappers**
- Output **(key, value) pairs** where
	- **key** is the set of fields to index (a keyword)
	- **value** is a unique identifier of the objects to associate with each keyword
**Reducers**
- Receive a set of identifiers for each keyword and simply concatenate them
**Combiners**
- Usually are not useful when using this pattern
- Usually there are no values to aggregate

![[IMG-20260507213327275.webp]]

Known uses are:
- Web search engine
	- Word – List of URLs 
---------------------
## Counting with Counters
**Goal:** Compute count summarizations of datasets. Provides a top-level view of the datasets.
**Motivation:** few high level statistics can be analyzed by domain expert to identify trends, anomaly etc.
### Structure
Mappers
- Process each input record and increment a set of counters
Map-only job
- No reducers
- No combiners
The results are stored/printed by the Driver of the application.
![[IMG-20260507213327409.webp]]
Known uses are:
- Count number of records
- Count a small number of unique instances
- Summarizations
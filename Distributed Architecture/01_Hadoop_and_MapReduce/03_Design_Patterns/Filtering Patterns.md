They are used to select the subset of input records of interest:
- Filtering
- Top K
- Distinct
## Filtering
**Goal:** Filter out input records that are not of interest and keep only the ones that we want. It focuses the analysis of the records of interest.
**Motivation:** Depending on the goals of your application, frequently only a small subset of the input data is of interest for further analyses.
### Structure
The **input** of the mapper is a set of records:
- **Key** $\rightarrow$  primary key
- **Value** $\rightarrow$ record
**Mappers:**
- Output: one **(key, value) pair** for each record that satisfies the enforced filtering rule
**Reducers: $\rightarrow$ Map-only job:**
- The reducer is useless in this pattern
- The number of reducers is set to 0
![[Filtering Patterns-1778183056286.webp|556]]
Known uses are:
- Record filtering
- Tracking events
- Distributed grep
- Data cleaning
-------
## Top K
**Goal:** Select a small set of top K records according to a ranking function. Focus on the most important records of the input data set.
**Motivation:** Frequently the interesting records are those ranking first according to a ranking function (most profitable items, outliers etc.).
### Structure
**Mappers**
- Each mapper initializes an in-mapper top $k$ list
	- $k$ is usually small (e.g. 10)
	- The current top $k$-records of each mapper can be stored in main memory
	- Initialization performed in the setup method of the mapper
- The map function updates the current in-mapper top $k$ list
- The cleanup method emits the **$k$ (key, value) pairs** associated with the in-mapper local top $k$ records:
	- **Key** is the “null key”
	- **Value** is a in-mapper top $k$ record
**Reducer**
- A single reducer must be instantiated:
	- One single global view over the intermediate results emitted by the mappers to compute the final top $k$ records
- It computes the final top $k$ list by merging the local lists emitted by the mappers
	- All input (key, value) pairs have the same key
	- Hence, the reduce method is called only once
![[Filtering Patterns-1778183091663.webp|589]]
Known uses are:
- Outlier analysis
- Select interesting data
All based on a ranking function.
---
## Distinct
**Goal:** Find a unique set of records.
**Motivation:** Duplicates records are frequently useless.
### Structure
**Mappers** 
- Emit one (key, value) pair for each input record
	- **Key** = input record 
	- **Value** = null value 
 **Reducers** 
 -  Emit one (key, value) pair for each input (key, list of values) pair 
	 - **Key** = input key, i.e., input record 
	 - **Value** = null value
![[Filtering Patterns-1778183121538.webp|516]]
Known uses are:
- Duplicate data removal
- Distinct value selection

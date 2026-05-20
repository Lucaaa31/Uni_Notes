Are used to split or reorganize in subsets the input data:
- Binning
- Shuffling
The output of an application based on an organization pattern is usually the input of another application.
## Binning
**Goal:** Organize/move the input records into categories.
**Motivation:** The input data set contains heterogonous data, but each data analysis usually is focused only on a specific subsets of your data.
### Structure
Based on a Map-only job.
**Driver**
- Sets the list of “bins/output files” by means of Multiple Outputs
**Mappers**
- For each input (key, value) pair, select the output bin/file associated with it and emit a (key, value) in that file
	- **Key** of the emitted pair = key of the input pair
	- **Value** of the emitted pair = value of the input pair
No combiner or reducer.
![[Data Organization Patterns-1778321262552.webp]]
---
## Shuffling
**Goal:** Randomize the order of the records.
**Motivation:** For anonymization reasons or for selecting a subset of random data.
### Structure
Mappers
- Emit one (key, value) for each input record
	- **Key** is a random key
	- **Value** is the input record
Reducers
- Emit one (key, value) pair for each value in \[list-of-values\] of the input (key, \[list-of-values\]) pair


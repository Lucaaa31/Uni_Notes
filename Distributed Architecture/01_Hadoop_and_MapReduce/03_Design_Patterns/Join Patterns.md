Are use to implement the join operators of the relational algebra.
We will focus on the natural join, however the pattern is analogous for the other types of joins.
## Reduce side natural Join
**Goal:** Join the content of two relational tables.
The two tables are large.
**Motivation:** The join operation is useful in many applications.
### Structure
**Mappers**
- There are two mapper classes, one for each table
- Emit one (key, value) pair for each input record
	- **Key** is the value of the common attributes
	- **Value** is the concatenation of the name of the table of the current record and the content of the current record
	- Suppose you want to join the following tables:
		- Users = {userid, name, surname}
		- Likes = {userid, movieGenre}
	- The record
		- userid=u1, name=Paolo, surname=Garza for Users
		- Will generate the pair:
			- (userid=u1, “Users: name=Paolo, surname=Garza”)
**Reducers**
- Iterate over the values associated with each key and compute the local natural join for the current key
- For example, the (key, \[list of values\]) pair
	- (userid=u1,\[“User: name=Paolo, surname=Garza”, “Likes: movieGenre=horror”, “Likes: movieGenre=adventure”\]
	- Will generate the following output (key, value) pairs
		  - (userid=u1,“name=Paolo,surname=Garza,genre=horror”)
		  - (userid=u1,“name=Paolo,surname=Garza, genre=adventure”)
![[IMG-20260507213327233.webp]]
---
## Map side natural Join
**Goal:** Join the content of two relational tables.
One table is large, the other one is small enough to be completely loaded in main memory.
**Motivation:** The join operation is useful in many applications and frequently one of the two tables is small.
### Structure
Map-only job.
**Mappers**
- Processes the content of the large table
	- Receives one input (key, value) pair for each record of the large table and **joins it with the small table**
- The distributed cache approach is used to provide a copy of the small table to all mappers
- **Each mapper**
	- Performs the **local natural join** between the current record of the large table it is processing and the records of the small table
		- The content of the small table is loaded in the main memory of each mapper during the execution of its setup method
![[IMG-20260507213327267.webp]]
### Theta, Semi and Outer Joins
The SQL language is characterized by many types of joins. The **same patterns** used for implementing the natural join can be used also for the other SQL joins:
- The local join in the reducer of the reduce side natural join is substituted with the type of join of interest
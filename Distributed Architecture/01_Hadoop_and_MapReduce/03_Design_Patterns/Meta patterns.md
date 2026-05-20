Are used to organize the workflow of a complex application executing many jobs.
## Job Chaining
**Goal:** Execute a sequence of jobs synchronizing them.
**Intent:** Manage the workflow of complex applications based on many phases (iterations):
- Each phase is associated with a different MapReduce Job
- The output of a phase is the input of the next one
### Structure
**Single Driver**
- Contains the workflow of the application
- Executes the jobs in the proper order
**Mappers, reducers, and combiners**
- Each phase of the complex application is implement by a MapReduce Job
![[IMG-20260507213327242.webp]]
### Complex workflow
More complex workflows, which execute jobs in parallel, can also be implemented, however the synchronization of the jobs become more complex.
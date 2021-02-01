# Designing Data-Intensive Applications
## Chapter 1 - Foundations
databases - store data
caches - remember data for speed ups

| | |
|:-:|:-:\
| _reliability_ | continue to work correctly, even in face adversity |
| _scalability_ | reasonable ways to deal with growth |
| _maintainability_ | future developers will be able to work productively |
### Reliability
tolerate anticipated faults
fault-tolerant when things go wrong
fault: one component deviating from spec
failure: system as a whole stops providing required service to user
Want to prevent faults becoming failures. Better to tolerate faults than prevent faults. So may want to deliberately induce faults to ensure fault tolerance. 
Can sacrifice reliability to cut development costs, but important to understand potential overall business costs.
#### Hardware faults
Random and independent between machines. More likely to cause single node failures.
Mean time to failure (MTTF)
Hard disk MTTF = 10 - 50 years. Storage cluster with 10000 disks -> expected one failure a day.
Introduce hardware redundancy.
#### Software faults
Correlated across nodes. Mode likely to cause system-wide failure.
Typically caused by an assumption about the computational environment that stops being true.
Solve by:
- reasoning about assumptions and interactions in the system
- allow processes to crash and restart
- process isolation
- measure, monitor and analyze system behaviour/ assumptions in produciton
#### Human Errors
Configuration errors leading cause of server otage
Solve by:
- design systems that minimize opportunities for error: easy to do the right thing.
- Decouple locations where people can make most mistakes from where they can cause most failures: sandbox experimental environments.
- Test thoroughly at all levels
- Quick easy recovery from human errors: easy to rollback configurations
- Detailed and clear monitoring (telemetry) show early warning signals of problems, and help to diagnose when things go wrong:
    - performance metrics
    - error rates
### Scalability
Ability to cope with increased load.
#### Load parameters
Load is described by _load parameters_:
- requests per second
- ratio of reads to writes to a database
- number of simultaneous users
- cache hit rate
Do we care about average case, or bottleneck of small extreme cases?
_fan out_ number of requests to other services needed to serve one incomoing request
**Twitter example page 11**
#### Describing performance
1. When increasing a load parameter and hold syatem resources constant, how is system performance affected
2. When increasing load parameter, how much of an increase in system resources is needed to keep system performance unchanged
Latency vs. response time:
- response time: time client sees
- latenchy: duration that request is waiting to be handled
Usually care about _distribution_ of response times. Important to look at _tail latencies_ as that drives customer experience
#### Coping with load
Need to rethink architecture every order of magnitude of load increase
- *scaling up*: more powerful machine
- *scaling out*: add more machines
Distributing stateless services across multiple machinesis easy. But distributing stateful data systems is hard.
In a start-up it is usually more important to iterate quickly than to scale to some hyopthetical future load.
### Maintainability
Majority of cost of SE occurs in ongoing maintenence.
Nobody wants to work on legacy systems, so important not to create them.
Three design principles:
 - **Operability**: easy for Ops to keep system running
 - **Simplicity**: Easy for new engineers to understand the system
 - **Evolvability**: Easy to make changes in the future
#### Operability
 - Monitor health of system and restore service
 - Track causes of problems
 - Keep SW up to date
 - Keep tabs on component dependencies
 - Establish goog processes, practices and tools
 - Maintain security
 - provide self-healing where appropriate
#### Simplicity
Symptoms of complexity:
- explosion of state space
- tight couping
- tangled dependencies
- inconsistent naming and terminology
- hacks for performance issues
- special-case work arounds
Abstraction to the rescue!
- removes accidental complexity (complexity not inherent in the problem)
- can be used in a wide range of different problems
- easier to evolove
#### Evolvability
Be Agile.
## Chapter 2 - Data models and query languages
### Foundations
Important question in data modelling: how is data represented in terms of the next level?
#### NoSQL
Not only SQL
Driven by:
 - need for greater scalabiliy
 - preference for open source software
 - need for specialised query operationrs
 - desire for more dynamic and expressive data model
Document databases are stored in one-to-many model: one JSON key can have multiple children
#### Object relational mismatch
Awkard transaltion layer needed between DB and applicaiton. Can use Object-relational mapping (ORM)
#### Many-to-one and Many-to-many relationships
ID vs free string: ID removes duplication. This removal of duplicaiton is _normalization_.
Normalization introduces many-to-one relationships (many people all share a single ID), doesn't match well in document model. May need to emulate joins in application code. Users have many positions, and many poisitions are shared by users. But can use _document reference_ to referenece a related item.
#### Relational vs Document databases
pros:
- relational:
    - good for joins and many-to-many and many-to-one relationships
- document:
    - good when applicaiton has a tree like document structure
    - good for collections of hererogenous objects
    - good when structure of data is detemrined by external systems over which you have no control
    - storage locality: if commonly access entire document (e.g. to display a webpage) benefit from data locality
cons:
- relational:
    - can lead to clumsy schemas and queries when using object orientated code
- document:
    - poor support for joins, have to do it in application code with multiple queries.
    - storage locality: if you only need to update a small portion of document, still need to load all of it (so keep documents small)
Document model: schema-on-read, schema is implicit and only interpreted when data is read (similar to dynamic typing)
- if change schema, just start writing new documents differently, and then handle old format seprately
Relational model: shema-on-write, schema is explidit and database ensures all writen data conforms to it (similar to static typing)
- to change database schema you need to perform a database migration

Overall, two data models complement each other, and each is becoming more like the other (JSON in SQL, client-side joins in MongoDB).
Best to take a hybrid database approach.

# Designing Data-Intensive Applications
## Chapter 1
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

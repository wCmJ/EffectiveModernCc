# Key characteristics of a distributed system
## Scalabity

## Reliability

## Availability

## Efficiency

## LB
- a critical component of any distributed system
- spread the traffic across a cluster of servers to improve responsiveness and availability of applications, websites or databases
- reduce individual server load and prevents any one application server from becoming a single point of failure
- To utilize full scalability and redundancy, we can try to balance the load at each layer of the system, we can add LBs at three places
  1. between the user and the web server
  2. between web servers and an internal platform layer, like application servers or cache servers
- algorithm
  1. the server is actually responding appropriately to requests
  2. algorithms
    - least connection method
    - least response time method
    - least bandwidth method
    - round robin method
    - weighted round robin method
    - IP Hash
- LB can be a single point of failure, to overcome this, a second LB can be connected to the first to form a cluster

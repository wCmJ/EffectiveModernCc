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

## Caching
- Application server cache
- Content Distribution Network(CDN)
  1. 
- Cache invalidation
  - Solve cache invalidation
    1. write-through cache
    2. write-around cache
    3. write-back cache
- Cache eviction policies
  1. FIFO
  2. LIFO
  3. LRU
  4. MRU
  5. LFU
  6. RR



## Basics
### consider a few things
- the different architectural pieces that can be used
- how do this pieces work with each other
- how can we best utilize these pieces: what are the right tradeoffs?
Start with the key characteristics of Distributed Systems


## Key characteristics of Distributed Systems
- Scalability
- Reliability
- Availability
- Efficiency
- Manageability
### Scalability
- capability of a system, process, or a network to grow and manage increased demand
- to support the growing amount of work
- without performance loss

#### Horizontal vs. Vertical Scaling
- Horizontal scaling means that you scale by adding more servers into your pool of resources whereas. add more machines into the existing pool
- Vertical scaling means that you scale by adding more power(CPU, RAM, Storage, etc.). comes with an upper limit
- 横向扩展意味着增加更多机器，纵向扩展意味着使用性能更强劲的机器
- Examples:
  - Horizontal scaling
    1. MongoDB: by adding more machines to meet growing needs
  - Vertical scaling
    1. MySQL: by switching from smaller to bigger machines

### Reliability
- 分布式系统中，多台机器提供服务，确保信息不丢失
- 关键词：冗余防止单点故障

### Availability
- if a system is reliable, it is available
- if it is available, it is not necessarily reliable

### Efficiency
- 衡量标准：response time(latency) and throughput(or bandwidth)

### Serviceability or Manageability
- how easy it is to operate and maintain
- the time to fix a failed system increases
- the ease of diagnosing and understanding problems when they occur
- ease of making updates or modifications

## Load Balancer
- another critical component of any distributed system
- reduce individual server load and prevents any one application server from becoming a single point of failure
- we can add LBs at three places
  - between the user and the web server
  - between web servers and an internal platform layer, like application servers or cache servers
  - between internal platform layer and database

- 负载均衡算法，依赖于健康度检查，健康度检查与后端servers连接，获取它们的状态
  - 最少连接
  - 最短延时
  - 最低带宽（最少业务流量）
  - 轮询
  - 加权轮询
  - IP Hash

- LB单点故障
  - 引入另一个LB连接第一个Lb形成一个集群
  - 每一个LB监控另一个LB的健康度
  - 如果main LB故障，第二个LB成为main LB


## Caching
- LB支撑横向扩展
- 缓存提高单台机器的效率
- 缓存可以存在于任意层：硬件、操作系统、浏览器、应用程序等
### Application server cache
- cache on a request layer node

### Content Distribution Network (CDN)
- static media
- a request will first ask the CDN for a piece of static media
- CDN will serve that content if it has it locally available
- if it isn't available, the CDN will query the back-end servers for the file, cache it locally


### Cache Invalidation
- cache should be valid, to maintain this
  - write-through cache
    1. 数据同时写入缓存和数据库中，保证不会丢失任何数据，无论发生crash，断电或者其他系统错误
    2. 增加latency，在返回数据之前需要写两次
  - write-around cache
    1. 写入数据库，跳过缓存
    2. 新数据请求会出现缓存失效，需要重新从后端读取，增加latency
  - write-back cache
    1. 写入缓存后返回给用户，在规定时间或者满足一定条件后，再写入数据库
    2. 存在数据丢失风险

### 缓存淘汰策略
- FIFO
- LIFO
- LRU
- MRU
- LFU
- RR


## Data Partitioning
- break up a big db into many smaller parts

### Partitioning Methods
- Horizontal partitioning
  1. 将不同的rows放入不同tables中
  2. 问题：如果分片关键字不均匀，会导致服务器不均衡

- Vertical partitioning
  1. 将数据按照属性分别存储在不同的server，比如用户、照片、关注等，分别放在不同的数据库中
  2. 问题：查询时需要遍历所有servers
- Directory Based Partitioning
  1. 创建一个查询服务，该服务知道当前的分片策略，并且负责维护tuple key和db的映射关系

### Partitioning criteria
- Key or hash-based partitioning
  1. 哈希形式，固定总量n，通过%n定位服务器
  2. 问题：增加n时，需要更新所有数据。可通过一致性哈希解决

- List partitioning
  1. 

- Round-robin partitioning

- Composite partitioning
  1. 结合上述几种方式


### 问题
- 数据被打散后，会给查询或者更新带来问题



## Indexs

## Proxies
- filter requests, log requests, thansform requests
- serve as back-end server use cache


### Proxy Server Types
- Open Proxy
- Reverse Proxy



## Redundancy and Replication
- Redundancy: remove single point of failure
- Replication: sharing information to ensure consistency between redundant resources
  - master-slave


## SQL Vs. NoSQL
- SQL: rows and columns
- NoSQL
  - Key-Value Stores: Redis, Voldemort, Dynamo
  - Document Databases: CouchDB, MongoDB
  - Wide-Column DB: Cassandra, HBase
  - Graph DB: Neo4J, InfiniteGraph


### High level differences
- Storage
  1. SQL: tables
  2. NoSQL: key-value, document, graph
- Schema
  1. SQL: each record conforms to a fixed schema
  2. NoSQL: schemas are dynamic
- Querying
- Scalability
  1. SQL: vertical
  2. NoSQL: horizontal

- Reliability
  1. most of the NoSQL solutions sacrifice ACID compliance for performance and scalability


### Which one to use
- Reasons to use SQL
  1. ACID
  2. data is structured and unchanging

- Reasons to use NoSQL
  1. large volumes of data that often have little to no structure
  2. Making the most of cloud computing and storage
  3. Rapid development










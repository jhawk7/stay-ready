# Data Intensive Design Overview

## Data Models
### DocumentDB vs RelationDB
- when data becomes more relational as in many-to-many relationships, it become more awkward to use a document db than a relational db (all though graph data models are best for this use case)
- document data models provide a flexible schema and are good for one-to-many relationships and when the entire document is frequently accessed (such as rendering a user profile page)
- in DocumentDBs such as MongoDB, data is stored as a single continual binary (BSON) which provides better data locality when you often need to read the entire document of the database as opposed to a relational database where splitting the data across multiple joined tables requires more disk seeks for foreign key references. This makes MongoDB faster for this use case. 
- This advantage can also be a disadvantage for MongoDB, as writes apply to the entire document and it is recommended to keep documents fairly small and avoid writes that increase the size of the document

### Graph Data Models
- as **many-to-many relationships** become more and more complex the need for a graph data model prevails over a relational db model (better for simpler many-to-many relationships)
- creating complex many-to-many relationships using a typical relational model requires the use of conjunction tables

## Retrieving/Storing Data
**covered in system design topics**
- **Btree Indexes, Hash Indexes, LSM Trees + SS Tables**
- with LSM Trees its possible for us to lose data if a server crashes before flushing the data in the tree to the tables on disk. To mitigate this issue we can append writes to the LSM tree automatically to a log on disk for the sole purpose of restoring the tree if it goes down. Once the data is flushed to disk, we can clear the logfile
- `Bloom filters` are a useful datastructure for LSM Tree + SS Tables to quickly tell us if a key does not exist so that we can avoid reading SS tables on disk
- through **compatction** ss tables produce smaller file sizes on disk than btrees

## Distributed Data
- there are two ways data is distributed across multiple nodes:
	- `Replication` - keeping copies of data on several different nodes, potentially in different locations; provides redundancy
	- `Partitioning` - splitting database into smaller subsets across multiple partitions or shards (sharding)

## Replication
- Replication can provide better data locality for users, reducing latency. This also increases availability of data due to the redundant copies. Replicated data also increases read throughput (more places to read)
- data is kept consistent across replicas via leader-based replication (singe-leader, multi-leader, leaderless) and some form of replication (asynchronous, synchronous, semi-synchronous)
- its usually impratical to use a fully synchronous replication approach as a replica that is down can block all writes to the system
- when adding a new follower to the system, the follower can be added without downtime by taking a snapshot of the current data and requesting new data since its snapshow when it becomes available (via some sort of log sequence number)
- using **monotonic reads** ensures that each user sees the same data in a scenario where some asynchronous replicas may have not received the updated data and others have; a user will read from the same replica consistently

## Partitioning
- for very large datasets or high query throughput, it becomes inefficient to only replicate data across nodes; we have to partition our data for better scalability
- partitioning is usually combined with replication so that replications of partitions are stored across nodes
- each node may contain partions that are leaders of their replications and partitions that are followers of other replications
- there are different ways to partition data
	- **key range** - allows for range queries, but introduces the problem of hotspots
	- **randomly** - creates more evenly distributed data across partitions, but we have to query all partions in parallel to find our data
	- **hash** - this method is preferred; it uses the hash of a key to determine its partition, reducing potential hotspots, evenly distributing data across shards, and allowing faster lookup times; we lose the ability to use range queries (or range queries require secondary ordering of data - `global secondary index`)
- keeping a seperate set of sorted data across nodes based on a global secondary index would also require something like `two-phase commit` to atomically update data across shards
- overtime we may need to account for **rebalancing** or moving load between shards, which is where we'd need `consistent hashing`
- the complexity of routing client requests to the appropriate shard is usually handled by a service coordinator

## Transactions
- group serveral related reads and writes into one logical unit; transaction is all or nothing (`atomicity` in `ACID`)
- `isolation` in terms of ACID is synonomous with serializability, meaning concurrent transactions do not step on each others toes and act in isolation from one another
- there are differenct isolation levels that dbs use:
	- `Read Commited Isolation` - the weakest and least expensive form of isolation; prevents dirty reads and writes (only r/w data that is committed)
	- `Snapshot Isonlation` - prevents read skew by allowing each transaction to read from a consistent snapshot of the data; this also involves keeping several version of an object side by side (multi version concurrency control MVCC); hurts long running transactions
	- `Locking` - providing row level locks to prevent race conditions, but can also cause deadlocks to occur (solution is to grab more locks/ **predicate locking**)
	- `Serial Execution` - literally run transactions in serial order; single core (no concurrency); another way to achieve serializability is to use `Two-Phase Locking (2PL)` (present in MySQL) - a form of `pessimistic concurrency control`, and a third is to implement `Serializable Snapshot Isolation (SSI)` (present in PostgresQL) - a form of `optimistic concurrency control` that adds an algorithm for detecting serialization conflicts in order to abort transactions on top of snapshot isolation

- Two-phase-locking can be overkill for concurrent transactions that don't necessarily conflict with each other (still need to wait for locks). On the other hand, SSI can be bad for many transactions accessing the same objects

## Consistency, Consensus, Fault Tolerance, & Linearizability
- Distributed systems usually deal with `eventual consistency` data, but need `linearizability` to mitigate issues such as replication lag, and give the appearance of linear data with atomic operations
	- in a linearizable system, all clients should be able to see a value that was just written (most recent write); linearizability is a recency guarantee

- Serializability is different from linearizability in that serializability refers to the concept of concurrent transactions behaving the same as if they were executed in some serial order, where as linearizability is all about the recency guarantee on r/w operations
	- actual serial execution and two-phase locking are typically linearizable
	- SSI is not linearizable as reads are made from a consistent snapshot which doesn't include recent updates to avoid lock contention
 
- `Coordination Services` are often used for `distributed locking` and `leader election` and do so with `consensus algorithms`
- `CAP theorem` explains the trade-offs between availability and consistency/linearizability of a system in the face of a network partition
- Distributed transactions and consensus are important parts of a distributed system
- `Total Order Broadcast` is a protocol for exchanging messages between nodes requiring total order and reliable delivery (used by etcd and zookeeper); it can be thought of as a way of creating a log
- `two-phase commit` is an algorithm used to atomically commit transactions across nodes
- consensus algorithms in general help keep replications consistent
- zookeeper is based on Google's chubby lock service and implements consensus as well as a other features that are useful in a distibuted system
	- linearizable atomic operations which allow for things like distributed locks
	- total ordering of operations
	- failure detection
	- change notifications
- services like zookeeper and etcd (k8s) also provide `service discovery` - to find which IP is needed to reach a certain service. This is important for services running on vms as they constantly come online/offline. With service discovery, services register their IP/endpoint with a public service registry 






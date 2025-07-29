> Alot of this is leveraged from Hello Interview and are just my notes on the content inside it

# Approach Framework:

- Functional Requirement
- Non Functional Requirement:

  - Availability vs Consistency:

    - Available Systems: Data may not be accurate - but will be available in terms of providing response to request
    - Consistent Systems: Data will always be accurate - but the system or row in DB may not be available - All instance of the DB if there is replication will not be available for that time
    - Generally Aim for high availability - unless system like banking, booking or Inventory management; for limited resources are involved:
      - NoSQL vs SQL:
        - Both can be highly available vs Scalable
        - Default SQL (Even for Available Systems), NoSQL for high Read/Write (Live comments, Logging etc.) or Non Relational type data:
          - SQL - 5k upto 10k rps on spikes
          - NoSQL - 10k upto 15k rps on spikes
    - How to make System Available vs making System Consistent -> How can Availability move towards consistency and consistency move towards Availability:
      - Availability:
        - System can be made Available:
          - Sharding/Partioning - each DB instance holds specific subsets of data (DB1 -> Username A-G, DB2 -> Username H-K ..):
            - Can lead to FanOut: Need to collect data from various nodes leading to scatter and gather causing requests to fail if one fails - single point of failure but offers improves scalability and latency
            - Race Condition - Same partition accessing same data order can lead to different results
          - Replication - Multiple Instances of the DB
          - Failover/Retry Mechanism
          - Horizontal Scaling:
        - Available Systems can be made Consistent:
          - Sharding only locks the row in specific DB row
      - Consistency:
        - System can be made Consistent:
          - Select SQL DB
        - Consistent Systems made Available:
          - Sharding
          - Optimistic Updates
          - Failover/Retry Mechanism

  - Throughput/Scalability/Bursty Traffic
    - Queues / Event Sourcing / Pub/Sub
    - Partitioning
    - Federation
  - Latency
    - Caching
    - Indexing
    - Eliminate Joins/FanOut
  - Fault Tolerance
    - Replication
    - Retry Mechanism
  - Compliance
  - Security
    - Transit - Encrypt HTTPs
    - Rest/DB - Encrypt Data
    - Access Level - Authorization
  - Environment

- Core Entities: Main components that will be within the system
- API Design
- High Level Design:
  - Tackle FR - Basic CRUD
- Deep Dive
  - NFR - Add complexities like Scaling/Queue/Caching/Type of DB - Replication/Sharding etc.

# Core Concepts & Technologies:

- Locking:
  - When multiple resources try to access the same source can lead to data loss or corruption - hence we lock the DB:
    - This makes a specific row unavailable and then we can employ optimistic updates
- Indexing:
  - Make request faster with faster lookup (Hashmap, B-Tree)
  - Fewer Subset of data to go through - Can employ binary search on the keys in BTree with sorted data
- Communication Protocol:

  - HTTPs - Client to Server / Webhooks (Server to Server)
  - gRPC - Client to Server
  - Websockets - BiDirectional
  - SSE - Server to Client

- Core DB:

  - SQL - Relational DB, SQL Joins Slow, Transactions multiple request if one fails all fail, Indexing fast
  - NoSQL - Redis/KV, Document/Dynamo - High Reads/Write, Column/Cassandra - Good for VERY High Writes, Elastic Search (Searching), Blob Storage (Large Data Sorage)

- Queue:

  - Client sends task -> Topics get task -> Watcher/Listener triggers -> Worker picks up the task its node has a consumer -> Consumer process the task
  - Dead letter Queue - Task didn't process; Backpressure too much bursty traffic send incomplete requests

- Streams / Event Sourcing / Pub/Sub:

  - Process large amounts of data in real time (Likes, comments, chat, real time analytics etc.) - Every transaction needs to be tracked
  - Multiple consumers reading from same stream

- Distributed Lock:

  - Longer term locking across multiple systems - stored in Redis Cache if there don't process - TTL to prevent deadlock

- Distributed Cache:
  - Store data in Memory
  - Eviction Policy - too much data in DB - LFU, LRU
  - Invalidation Policy - check if data is up to date - TTL / Write Through / Write Behind
  - Cache Write Strategy - Write Through / Write Behind

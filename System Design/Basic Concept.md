# Basic Concepts in System Design

![horizontal-vs-vertical-scaling](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/horizontal-vs-vertical-scaling.webp)

## vertical scaling

vertical scaling means adding more resources to a single node and adding additional CPU, RAM and Disk to cope with an increasing workload. 

pros: 

1. cost-effective: upgrading a pre-existing server costs less than buying a new one. And you are less likely to add backup and software when scaling vertically. Maintenance costs may potentially remain the same too.
2. Less need for software changes: You are less likely to change how the software on a server works or how it is implemented.
3. Less complex process communication: When a single node handles all the layers of your services, it will not have to synchronize and communicate with other machines to work.

cons:

1. Single point of failure: Having all your operations on a single server increases the risk of losing all your data if a hardware or software failure was to occur.
2. Upgrade limitation: There is always a limitation of how much you can upgrade on a single machine. Every machine has its threshold for RAM, storage and processing power.

## Horizontal scaling

Horizontal scaling refers to adding additional nodes or machines to your infrastructure to cope with new demands. 

pros:

1. Increase fault tolerance: Distributing your services on different nodes can save you from single point failure.
2. Increase performance: When you add more machines to your services, it provides for endpoints for your connections and increase the performance.

cons: 

1. Cost: Adding new machines costs more than upgrading an existing machine.
2. Complex maintenance and operation: You will need to add software like load balancing to make all your machines cooperate with each other.

## Load balance

A load balancer works in front of servers and distributes the set of requested operations effectively across multiple servers and ensures that no single server gets too many requests which will degrades the overall performance of the application.

Why we need load balancer?

1. Single point of failure: It will avoid single point of failure. When we have multiple servers, we need load balancer to help us distribute requests into different servers.
2. Overload servers: Load balancer can spread requests across multiple servers.
3. Public IP address: Only expose load balancer's public IP address to clients, which make it privacy safe for those servers behind load balancer. In addition, the world has been running out of the version four IP address, so it just hard and expensive to get enough public IP address.

Where we put load balancer?

1. Between client and servers
2. Between server and job servers
3. Between job server and cache servers
4. Between cache server and database servers

Load balancing algorithms

1. Round robin: request are distributed across the server in a sequential manner.
   - Easy to implement
   - *Not consider the load already, thus a risk of one server with high workload
2. Weighted round robin: Each of the resources is provided a weighted score. Depending on the weighted score, the load balancer will decide deliver the request to which server.
   - Ensure even workload among all servers
   - *Have additional requests to check servers' weighted score
3. Least connection/response time
4. Randomly distribute

Layer 4 & Layer 7 Load balancer reference

## Master-Slave Database

**What:** Master-Slave contains one master and many slaves servers. Master handles writing operations only and slaves handle reading operations. Thus, true data is only kept in master database. Then master will transfer/synchronize data to slaves databases, which is known as **replication**.

**Why:** In a large scale system, read/write traffic is one server bottleneck and impact the experience. 

**How:** To improve it, there are 3 ways:

- Cache only: Put data into persistent cache with some caching algorithms. **Cons**: But only selected data will be put in cache, database server might still have high workload.
- Master-Slave: in high read system, spread high read workload among these slaves and use one master to write. Then master replicates new data to slaves. **Cons**: every request will read/write the disk, which is time consuming.
- Hybrid cache and master-slave: First go to cache to find data, if not exist, then got to master-slave database to search. ![master-slave](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/master-slave.png)

## Scalability for Dummies

### Part 1: Clones

Users should always get the same results of their request back, independent what server he landed on. This leads to the first rule of scalability: **every server contains exactly the same codebase and does not store any user-related data, like sessions or profile picture, on local disc or memory**. 

Sessions need to be stored in a centralized data store which is accessible to all the application servers. It can be external database or persistent cache, like Redis.

### Part 2: Database

After following this, servers can now horizontally scale. But might have another issue: application become slow and sometimes breaks down.

Here are 2 paths:

- To do master-slave replication, read from slaves and write to master and upgrade master server with more RAM;
- De-normalize right from the beginning and include no Joins operations. In this way, you still use MySQL, but treat it as a No-SQL database.

### Part 3: Cache

Even we use master-slave model database, users will still suffer from slow page requests because read/write directly on database is time consuming. The solution is to implement of a cache.

A cache is a simple key-value store and it should reside as a buffering layer between your application and your data storage. Whenever your application has to read data, it should first try to retrieve data from the cache. If it does not hit, then it will read from your database.

### Part 4: Asynchronism

- Do time-consuming work in advance: We can turn dynamic content into static content. For example, pages of website, maybe built with a massive framework or CSM. We can pre-compute the overall data and upload these pre-rendered HTML pages to AWS S3 or other CDN (Content Delivery Network). Next time, users requests will get these pre-compute data directly from these Cloud front and will save a lot of time.
- Handle tasks asynchronously: If a customer has a special request. We can handle this special task asynchronously. Once we get the special request, we will sends a job to a job queue and immediately signals back the customer and inform him his task is in queue, please wait. 

## Trade-offs

### [Performance VS Scalability](M:\Typora\MyNotes\MyNotes\System Design\Performance VS Scalability.md)

Scalability is being able to handle large amounts of users/data/traffic. 

Performance is about speed. 

## Availability VS Consistency

The CAP theorem states that, in a distributed system, we can only have two out of: Consistency, Availability and Partition Tolerance.

- Consistency: All clients see the same data at the same time.
- Availability: All clients can perform read and write accesses at any time since the system always responds.
- Partition Tolerance: The system can continue to operate as a whole even if individual nodes fail or cannot communicate with other nodes.

In distribution system, networks aren't completely reliable, and we must tolerate partitions in the system. This means, in CAP theorem, we are left two options: Consistency and Availability.

- CP - Consistency & Partition: This is referring to a system where availability is sacrificed only in case of a network partition.  In this case, when a request is sent to a partitioned node. However, the node does not have the most updated data (Not consistency). It will keep waiting for a response from the partitioned node and might result in a timeout error. CP is a good choice is your business needs atomic reads and writes. e.g. Banking applications need to be able to reliably debit and transfer funds from accounts. They depend on **consistency and partition tolerance** to prevent accounting errors.
- AP - Availability & Partition: This is referring to a system where consistency is sacrificed only in case of a network partition. In this case a request can be always responsible, no matter the data is the most latest or not. e.g. DNS (i.e. the domain name system) is AP theorem. Due to a large number of servers, DNS system is available all the time. If a DNS server fails, the next one will take its place. However, consistency is not guaranteed. If an entry is modified, it may take several days to pass the changes to all servers in the system.

### Consistency patterns

### Weak consistency

After a write, reads may or may not see it. A best effort approach is taken.

This approach is seen in systems such as memcached. Weak consistency works well in real time use cases such as VoIP, video chat, and realtime multiplayer games. For example, if you are on a phone call and lose reception for a few seconds, when you regain connection you do not hear what was spoken during connection loss. (data is lose -> weak consistency)

### Eventual consistency

After a write, reads will eventually see it (typically within milliseconds). Data is replicated asynchronously.

This approach is seen in systems such as DNS and email. Eventual consistency works well in highly available systems.

### Strong consistency

After a write, reads will see it. Data is replicated synchronously.

This approach is seen in file systems and RDBMSes. Strong consistency works well in systems that need **transactions**.



### Availability patterns

There are two complementary patterns to support high availability: fail-over and replication.

- Fail-over: It means in case of an issue (e.g. crashed node of a cluster, no network) application will be handled (will run) by different node of a cluster.
- Replication: Replicate data from site A to site B. Usually one wants this to be a different DC (data center) as you want to survive disasters recovery.

#### Fail-over

##### Active-passive

With active-passive fail-over, heartbeats are sent between the active and passive servers on standby. If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes the service.

The length of downtime is determined by whether the passive node is a **hot** standby or a **cold** standby. 

Active-passive failover can also be referred to as master-slave failover. 

##### Active-active

In active-active, both servers are managing traffic, spreading the load between them. 

Active-active failover is also referred as master-master failover.

##### Pros & Cons

- Pros
  - High availability
- Cons
  - Need more hardware and additional complexity
  - There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive



#### Replication

##### Master-slave

**Master**: It serve for reads and writes and replicate newly writes to its slaves.

**Slave**: It serve for reads. It will standby and when a master goes offline, the system can continue to read-only mode until a slave is promoted to a master or a new master is provisioned.

<img src="https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/C9ioGtn.png" alt="img" style="zoom:25%;" />



##### Master-master

Both masters serve reads and writes and coordinate with each other on writes. If either master goes down, the system can continue to operate with both reads and writes.

<img src="https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/krAHLGg.png" alt="img" style="zoom:33%;" />

##### Pros & Cons

- Pros:
  - high availability
- Cons:
  - Potential loss of data if the master fails before any newly written data can be replicated to other nodes
  - Need more hardware and additional complexity



[四层负载均衡中的NAT模式与IP隧道模式](https://www.bilibili.com/video/BV1WS4y1F7uN?t=4.2)

[四层和七层负载均衡的区别介绍](https://blog.csdn.net/weixin_33738555/article/details/92861552?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-92861552-blog-90673399.pc_relevant_aa&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-92861552-blog-90673399.pc_relevant_aa&utm_relevant_index=2)


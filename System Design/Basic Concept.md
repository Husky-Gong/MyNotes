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

- To do master-slave replication, read from slaves and write to master and upgrade master server with more RAM



[四层负载均衡中的NAT模式与IP隧道模式](https://www.bilibili.com/video/BV1WS4y1F7uN?t=4.2)

[四层和七层负载均衡的区别介绍](https://blog.csdn.net/weixin_33738555/article/details/92861552?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-92861552-blog-90673399.pc_relevant_aa&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-92861552-blog-90673399.pc_relevant_aa&utm_relevant_index=2)


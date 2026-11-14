# 🚧 System Design — Bottlenecks

> A bottleneck is the limiting component of a system that restricts the overall performance or capacity of the system.

---

## 📚 Table of Contents

1. [What is a Bottleneck?](#-what-is-a-bottleneck)
2. [Real-Life Example](#-real-life-example)
3. [Bottleneck in Software Systems](#-bottleneck-in-software-systems)
4. [Common Types of Bottlenecks](#-common-types-of-bottlenecks)
5. [Database Bottleneck](#-database-bottleneck)
6. [CPU Bottleneck](#-cpu-bottleneck)
7. [Memory Bottleneck](#-memory-bottleneck)
8. [Network Bottleneck](#-network-bottleneck)
9. [External API Bottleneck](#-external-api-bottleneck)
10. [Lock & Contention Bottleneck](#-lock--contention-bottleneck)
11. [Single Server Bottleneck](#-single-server-bottleneck)
12. [How to Identify Bottlenecks](#-how-to-identify-bottlenecks)
13. [How to Solve Bottlenecks](#-how-to-solve-bottlenecks)
14. [Scaling and Bottlenecks](#-scaling-and-bottlenecks)
15. [Developer Assessment Platform Example](#-developer-assessment-platform-example)
16. [Interview Questions](#-system-design-interview-questions)
17. [Quick Revision](#-quick-revision)
18. [Key Takeaways](#-key-takeaways)

---

# 🚧 What is a Bottleneck?

**Bottleneck** হলো System-এর এমন একটি component যার limited capacity পুরো system-এর performance বা scalability-কে সীমাবদ্ধ করে।

সহজভাবে:

```text
Bottleneck
    ↓
System-এর limiting component
    ↓
Overall system capacity সীমাবদ্ধ করে
```

### Simple Definition

> A bottleneck is the slowest or most constrained part of a system that limits the overall system performance.

---

# 🚗 Real-Life Example

ধরো একটি বড় highway:

```text
10-Lane Road
      ↓
    1-Lane
      ↓
10-Lane Road
```

মাঝখানের 1-lane road-এর কারণে traffic জমে যাবে।

এই 1-lane অংশটাই হলো:

```text
🚧 BOTTLENECK
```

Software system-এও একই concept।

---

# 💻 Bottleneck in Software Systems

ধরো একটি system:

```text
Client
  ↓
Load Balancer
  ↓
Application Servers
  ↓
Database
```

যদি Application Servers প্রতি second-এ:

```text
10,000 requests
```

handle করতে পারে কিন্তু Database পারে:

```text
2,000 queries
```

তাহলে:

```text
Application Servers → 10,000 RPS
Database            → 2,000 QPS
```

Database হয়ে যাবে:

```text
🚧 BOTTLENECK
```

কারণ database-এর capacity পুরো system-এর capacity সীমাবদ্ধ করছে।

---

# 🔥 Common Types of Bottlenecks

একটি distributed system-এ bottleneck বিভিন্ন জায়গায় হতে পারে:

```text
1. CPU Bottleneck
2. Memory Bottleneck
3. Database Bottleneck
4. Disk / I/O Bottleneck
5. Network Bottleneck
6. External API Bottleneck
7. Lock / Contention Bottleneck
8. Connection Pool Bottleneck
9. Queue Bottleneck
10. Single Server Bottleneck
```

---

# 🗄️ Database Bottleneck

Database bottleneck সবচেয়ে common bottleneck-এর একটি।

Architecture:

```text
Client
  ↓
API Server
  ↓
Database 🚧
```

Database bottleneck হওয়ার কারণ:

```text
• Slow Queries
• Missing Index
• Large Table Scan
• Too Many Connections
• Heavy JOINs
• Too Many Writes
• Disk I/O
• Lock Contention
```

### Example

```text
Application
    ↓
PostgreSQL
    ↓
Only 2,000 QPS
```

কিন্তু application:

```text
10,000 RPS
```

generate করছে।

তাহলে Database bottleneck।

### Possible Solutions

```text
Query Optimization
        ↓
Database Indexing
        ↓
Connection Pooling
        ↓
Caching
        ↓
Read Replicas
        ↓
Database Scaling
```

---

# 🧠 CPU Bottleneck

যখন server-এর CPU capacity প্রায় সম্পূর্ণ ব্যবহার হয়ে যায়:

```text
Application Server
       ↓
CPU = 100%
       ↓
🚧 Bottleneck
```

Possible causes:

```text
• CPU-intensive algorithms
• Complex calculations
• Too much processing
• Inefficient code
• Too many concurrent requests
```

Possible solutions:

```text
• Optimize algorithms
• Improve code efficiency
• Horizontal scaling
• Background processing
• Add more CPU
```

---

# 💾 Memory Bottleneck

যখন application-এর RAM usage খুব বেশি হয়ে যায়:

```text
Server
   ↓
RAM = 95%+
   ↓
🚧 Bottleneck
```

Possible problems:

```text
• Out Of Memory
• Slow Garbage Collection
• Application Crash
• Swapping
• Performance degradation
```

Possible solutions:

```text
• Memory optimization
• Reduce object size
• Fix memory leaks
• Add more RAM
• Horizontal scaling
```

---

# 🌐 Network Bottleneck

Network-এর capacity insufficient হলে network bottleneck তৈরি হতে পারে।

ধরো:

```text
Network Capacity
= 1 Gbps
```

কিন্তু application-এর প্রয়োজন:

```text
2 Gbps
```

তাহলে:

```text
Application
    ↓
Network
    ↓
1 Gbps 🚧
```

Possible solutions:

```text
• Increase bandwidth
• Compression
• Reduce payload size
• CDN
• Optimize network communication
```

---

# 🔌 External API Bottleneck

তোমার application একটি third-party service ব্যবহার করছে।

```text
Your Application
       ↓
External API 🚧
       ↓
Response
```

ধরো তোমার application:

```text
10,000 requests/sec
```

handle করতে পারে।

কিন্তু external API:

```text
500 requests/sec
```

allow করে।

তাহলে:

```text
External API = Bottleneck
```

Possible solutions:

```text
• Caching
• Queue
• Retry with backoff
• Batch requests
• Rate-limit aware design
• Alternative provider
```

---

# 🔒 Lock & Contention Bottleneck

একাধিক request একই shared resource-এর জন্য অপেক্ষা করলে contention তৈরি হতে পারে।

```text
Request 1 ──┐
Request 2 ──┤
Request 3 ──┼──> Shared Lock 🚧
Request 4 ──┤
Request 5 ──┘
```

উদাহরণ:

```text
Database Row Lock
Distributed Lock
Shared Resource
File Lock
```

একটি request resource ব্যবহার করার সময় অন্য requests অপেক্ষা করতে পারে।

Possible solutions:

```text
• Reduce lock duration
• Reduce shared state
• Optimize transactions
• Use better concurrency strategies
```

---

# 🖥️ Single Server Bottleneck

ধরো system-এ একটিই application server:

```text
             ┌──────────────┐
Users ──────>│ App Server 🚧│
             └──────────────┘
                    ↓
                 Database
```

Traffic বাড়লে server overloaded হয়ে যেতে পারে।

Better architecture:

```text
                 ┌── Server 1
                 │
Users
  ↓
Load Balancer ───┼── Server 2
                 │
                 └── Server 3
                       ↓
                    Database
```

এখানে **Horizontal Scaling** করা হলো।

---

# 🔍 How to Identify Bottlenecks

Bottleneck identify করার জন্য monitoring এবং metrics খুব গুরুত্বপূর্ণ।

### CPU Metrics

```text
CPU Utilization
Load Average
CPU Time
```

### Memory Metrics

```text
RAM Usage
Heap Usage
Garbage Collection
Swap Usage
```

### Database Metrics

```text
Query Latency
Queries Per Second
Connection Count
CPU Usage
Disk I/O
Lock Wait Time
```

### Application Metrics

```text
Request Latency
Requests Per Second
Error Rate
Queue Length
Active Connections
```

### Network Metrics

```text
Bandwidth Usage
Network Latency
Packet Loss
Throughput
```

---

# 🔄 Bottleneck Identification Flow

```text
                    System
                       ↓
                Monitor Metrics
                       ↓
                Find Slow Area
                       ↓
              Identify Bottleneck
                       ↓
               Find Root Cause
                       ↓
              Apply Optimization
                       ↓
                Measure Again
                       ↓
             Repeat if Necessary
```

---

# 🛠️ How to Solve Bottlenecks

Bottleneck solve করার আগে অবশ্যই **root cause** identify করতে হবে।

### Step 1 — Measure

```text
What is slow?
How slow?
When does it become slow?
```

### Step 2 — Identify

```text
CPU?
Memory?
Database?
Network?
External API?
Lock?
```

### Step 3 — Optimize

```text
Query Optimization
Caching
Indexing
Compression
Code Optimization
```

### Step 4 — Scale

```text
Horizontal Scaling
Vertical Scaling
Read Replicas
Sharding
CDN
Queue
```

### Step 5 — Measure Again

Optimization কাজ করেছে কিনা metrics দিয়ে verify করতে হবে।

---

# ⚖️ Scaling and Bottlenecks

একটি গুরুত্বপূর্ণ বিষয়:

> Application server scale করলেই সব bottleneck solve হয় না।

ধরো:

```text
              ┌── App Server
              │
Client ───────┼── App Server
              │
              └── App Server
                     ↓
                  Database 🚧
```

তুমি:

```text
3 Servers
   ↓
10 Servers
   ↓
50 Servers
```

করলেও Database যদি একই থাকে:

```text
Database 🚧
```

তাহলে database-ই system-এর limiting factor থাকবে।

---

# 🚀 Common Scaling Solutions

## 1. Horizontal Scaling

আরও server যোগ করা।

```text
1 Server
   ↓
3 Servers
   ↓
10 Servers
```

---

## 2. Vertical Scaling

একটি server-এর resource বাড়ানো।

```text
4 CPU / 8 GB RAM
        ↓
16 CPU / 32 GB RAM
```

---

## 3. Caching

Frequently accessed data cache করা।

```text
Client
  ↓
Application
  ↓
Redis
  ↓
Database
```

Cache hit হলে database-এর load কমে।

---

## 4. Read Replicas

Read workload distribute করা।

```text
              ┌── Read Replica
              │
Application ──┼── Read Replica
              │
              └── Primary DB
```

---

## 5. Message Queue

Heavy asynchronous work-এর জন্য queue ব্যবহার করা যায়।

```text
Application
     ↓
Message Queue
     ↓
Workers
     ↓
Database
```

উদাহরণ:

```text
Kafka
RabbitMQ
BullMQ
```

---

# 🎯 Developer Assessment Platform Example

ধরো আমাদের Developer Assessment Platform:

```text
Candidate
    ↓
Load Balancer
    ↓
API Servers
    ↓
PostgreSQL
```

একসাথে:

```text
50,000 Candidates
```

assessment দিচ্ছে।

তারা বারবার:

```text
GET /assessments/:id
GET /problems/:id
POST /submissions
GET /results/:id
```

request করছে।

যদি সব request database-এ যায়:

```text
50,000 Candidates
       ↓
API Servers
       ↓
PostgreSQL 🚧
```

তাহলে PostgreSQL bottleneck হতে পারে।

---

# 💡 Possible Improved Architecture

```text
                    Candidates
                         ↓
                  Load Balancer
                         ↓
                ┌────────┴────────┐
                ↓                 ↓
          API Server 1      API Server 2
                │                 │
                └────────┬────────┘
                         ↓
                       Redis
                    ↙         ↘
              Cache Hit      Cache Miss
                  ↓              ↓
              Response      PostgreSQL
```

Frequently accessed data যেমন:

```text
Published Assessments
Problem Metadata
Assessment Configuration
```

cache করা যেতে পারে।

তবে submissions-এর মতো গুরুত্বপূর্ণ writes-এর ক্ষেত্রে data consistency requirements বিবেচনা করতে হবে।

---

# 🎤 System Design Interview Questions

### Q1. What is a bottleneck?

**Answer:**

> A bottleneck is the component that limits the overall capacity or performance of a system.

---

### Q2. Can a database be a bottleneck?

Yes.

Possible reasons:

```text
Slow queries
Missing indexes
Too many connections
High write load
Disk I/O
Lock contention
```

---

### Q3. If application servers are slow, what will you check?

```text
CPU
Memory
Database latency
Network latency
External APIs
Locks
Connection pools
```

---

### Q4. Does adding more servers always solve bottlenecks?

**No.**

কারণ bottleneck অন্য কোনো component-এ থাকতে পারে।

```text
10 App Servers
      ↓
1 Database 🚧
```

আরও App Server যোগ করলেও Database bottleneck থাকতে পারে।

---

### Q5. How do you find a bottleneck?

```text
Monitor
   ↓
Measure
   ↓
Compare Capacity vs Demand
   ↓
Find Limiting Component
   ↓
Optimize / Scale
```

---

# 📝 Quick Revision

```text
Bottleneck
    ↓
Limiting Component
    ↓
Limits Overall System
```

### Common Bottlenecks

```text
CPU
Memory
Database
Disk / I/O
Network
External API
Locks
Connections
Queue
Single Server
```

### Common Solutions

```text
Caching
Indexing
Query Optimization
Horizontal Scaling
Vertical Scaling
Read Replicas
Sharding
CDN
Message Queue
Compression
```

---

# 🧠 Key Takeaways

1. **Bottleneck হলো system-এর limiting component।**

2. Bottleneck শুধু Database-এ হয় না।

3. CPU, Memory, Network, Disk, External API, Lock—সবই bottleneck হতে পারে।

4. Bottleneck solve করার আগে **measure এবং identify** করতে হবে।

5. Application server scale করলেই সব bottleneck solve হয় না।

6. **Caching** read-heavy workload-এর database pressure কমাতে পারে।

7. **Read Replicas** read workload distribute করতে পারে।

8. **Message Queue** asynchronous workload handle করতে সাহায্য করতে পারে।

9. **Monitoring + Metrics** bottleneck identify করার জন্য অত্যন্ত গুরুত্বপূর্ণ।

10. Bottleneck optimization-এর পর আবার **measure** করতে হবে।

---

# 🔥 Final Interview Formula

```text
Traffic
   ↓
Workload
   ↓
System Components
   ↓
Measure Performance
   ↓
Identify Bottleneck
   ↓
Find Root Cause
   ↓
Optimize
   ↓
Scale
   ↓
Measure Again
```

---

# ⭐ One-Line Definition

> **A bottleneck is the component of a system whose limited capacity restricts the overall system's performance or scalability.**

---

# 🚀 Remember

```text
Don't ask only:

"How do I scale the system?"

Also ask:

"Which component will become the bottleneck first?"

That question leads to better System Design.
```

# 🏗️ System Design — Interview Preparation

> **System Design শেখার জন্য সহজ বাংলা নোট — Backend & Software Engineering Interview Preparation**

---

## 📚 Table of Contents

- [System Design কী?](#-system-design-কী)
- [কেন System Design গুরুত্বপূর্ণ?](#-কেন-system-design-গুরুত্বপূর্ণ)
- [System Design-এর মূল ৪টি সিদ্ধান্ত](#-system-design-এর-মূল-৪টি-সিদ্ধান্ত)
    - [1. Components](#1-components)
    - [2. Data Boundaries](#2-data-boundaries)
    - [3. Interfaces](#3-interfaces)
    - [4. Operating Behavior](#4-operating-behavior)
- [Golden Rules](#-golden-rules)
- [Design বনাম Implementation](#-design-বনাম-implementation)
- [System Design Interview Approach](#-system-design-interview-approach)
- [Simple থেকে Large Scale](#-simple-থেকে-large-scale)
- [Interview Checklist](#-interview-checklist)
- [Quick Revision](#-quick-revision)
- [Key Takeaways](#-key-takeaways)

---

# 🔹 System Design কী?

**System Design** হলো এমন একটি প্রক্রিয়া যেখানে একটি Software System-এর বিভিন্ন অংশ কীভাবে একে অপরের সাথে কাজ করবে, তা নির্ধারণ করা হয়।

সহজ ভাষায়:

> **System Design = একটি Software System-এর Architecture, Components, Data Flow এবং Interaction কীভাবে কাজ করবে তা পরিকল্পনা করা।**

System Design-এর মূল উদ্দেশ্য হলো নির্দিষ্ট কিছু **Requirements** পূরণ করার জন্য একটি কার্যকর ও নির্ভরযোগ্য System Architecture তৈরি করা।

### একটি সাধারণ উদাহরণ

ধরো আমরা একটি Backend Application তৈরি করছি:

```text
    Client
    │
    ▼
    **REST** **API**
    │
    ▼
    Application Server
    │          │
    │          │
    ▼          ▼
    Cache     Database
এখানে আমাদের সিদ্ধান্ত নিতে হবে:

Application Server কোথায় থাকবে? Database কোনটি ব্যবহার করব? Cache দরকার কি না? Client কীভাবে Server-এর সাথে যোগাযোগ করবে? Traffic বেড়ে গেলে কী হবে? কোনো Server বা Database কাজ না করলে কী হবে?

এই architectural decision-গুলোই মূলত System Design।

🎯 কেন System Design গুরুত্বপূর্ণ?

ছোট একটি Application-এর ক্ষেত্রে Architecture সাধারণত খুব simple হতে পারে:

Client → Server → Database

কিন্তু User এবং Traffic বাড়তে থাকলে System-এর complexity বাড়ে:

    ┌──────────────┐
    │    Client    │
    └──────┬───────┘
    │
    ▼
    ┌──────────────┐
    │Load Balancer │
    └──────┬───────┘
    │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
    Server    Server    Server
    │         │         │
    └─────────┼─────────┘
    │
    ┌────────┴────────┐
    ▼                 ▼
    Cache            Database

Traffic বাড়লে আমাদের চিন্তা করতে হয়:

Scalability Performance Reliability Availability ### Database Load Caching ### Load Balancing ### Failure Handling ### Rate Limiting ### Data Consistency

তাই বড় Software System তৈরি করার আগে System Design সম্পর্কে পরিষ্কার ধারণা থাকা অত্যন্ত গুরুত্বপূর্ণ।

🧩 System Design-এর মূল ৪টি সিদ্ধান্ত

একটি Backend System Design করার সময় সাধারণত চারটি গুরুত্বপূর্ণ বিষয় নিয়ে চিন্তা করতে হয়:

┌─────────────────────────────────────┐ │          **SYSTEM** **DESIGN**              │ ├─────────────────────────────────────┤ │                                     │ │  1️⃣ Components                     │ │                                     │ │  2️⃣ Data Boundaries                │ │                                     │ │  3️⃣ Interfaces                     │ │                                     │ │  4️⃣ Operating Behavior             │ │                                     │ └─────────────────────────────────────┘ 1️⃣ Components

Components হলো System-এর প্রধান Building Blocks।

একটি Backend System-এ বিভিন্ন ধরনের Component থাকতে পারে:

### Application Server

Database
Cache
### Message Queue
### Load Balancer
### Object Storage
### Search Engine
### Authentication Service
### Background Worker
Example
    Client
    │
    ▼
    Load Balancer
    │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
    Server    Server    Server
    │         │         │
    └─────────┼─────────┘
    │
    ┌────────┴────────┐
    ▼                 ▼
    Redis           PostgreSQL
    │
    ▼
    Message Queue

প্রতিটি Component-এর একটি নির্দিষ্ট উদ্দেশ্য থাকা উচিত।

উদাহরণ
Component	উদ্দেশ্য
Application Server	Business Logic চালানো
Database	Permanent Data সংরক্ষণ
Redis	Frequently accessed data দ্রুত দেওয়া
Load Balancer	Traffic distribute করা
Message Queue	Asynchronous কাজ পরিচালনা
Object Storage	File/Image সংরক্ষণ
Search Engine	দ্রুত Search করা
⭐ গুরুত্বপূর্ণ Interview Rule

কোনো Component শুধু জনপ্রিয় বলে System-এ যোগ করা উচিত নয়।

সবসময় প্রশ্ন করতে হবে:

*এই Component কেন দরকার?*

অর্থাৎ:

Component
    ↓
কোন Problem Solve করছে?
    ↓
তারপর Architecture-এ যোগ করো
2️⃣ Data Boundaries

Data Boundaries বলতে বোঝায় System-এর কোন Data কোথায় থাকবে এবং কোন Storage হবে সেই Data-এর Source of Truth।

Example
    Application
    │
    ┌────────┴────────┐
    ▼                 ▼
    PostgreSQL           Redis
    │                 │
    │                 └── Cache
    │
    └── Source of Truth

এখানে:

PostgreSQL → Permanent / Authoritative Data Redis → Temporary Cached Data Source of Truth কী?

Source of Truth হলো এমন একটি Data Store যেখানে আসল এবং authoritative data সংরক্ষিত থাকে।

উদাহরণ:

### User Information

    │
    ▼
    PostgreSQL
    │
    └── Source of Truth

Redis-এ একই User-এর Data থাকতে পারে:

PostgreSQL → Original Data Redis      → Cached Copy

Redis-এর Data হারিয়ে গেলেও PostgreSQL থেকে আবার Data পাওয়া যাবে।

Interview-এ নিজেকে প্রশ্ন করো Data কোথায় Store হবে? কোন Database ব্যবহার করব? কোন Data Cache করা হবে? কোন Data Permanent? কোন Data Temporary? Source of Truth কোনটি? 3️⃣ Interfaces

Interfaces হলো System-এর বিভিন্ন অংশ কীভাবে একে অপরের সাথে যোগাযোগ করবে।

Client এবং Backend-এর মধ্যে যোগাযোগের একটি সাধারণ মাধ্যম:

Client
    │
    │ **HTTP** Request
    ▼
**REST** **API**
    │
    ▼
### Backend Server
Example **REST** **API**
**POST**   /api/users
**GET**    /api/users
**GET**    /api/users/:id
**PATCH**  /api/users/:id
**DELETE** /api/users/:id

এখানে **API** হলো Client এবং Backend-এর মধ্যে একটি Interface।

### Internal Interface

শুধু Client ↔ Server নয়।

একটি বড় System-এ বিভিন্ন Service-ও একে অপরের সাথে যোগাযোগ করতে পারে:

### User Service

    │
    │ **API** / Message
    ▼
### Order Service
    │
    │ Event
    ▼
### Payment Service

Communication হতে পারে:

**REST** **API** GraphQL gRPC ### Message Queue Event / Pub-Sub ### Interview Questions

System Design করার সময় চিন্তা করো:

Client কীভাবে Backend-এর সাথে কথা বলবে? Service কীভাবে অন্য Service-এর সাথে যোগাযোগ করবে? Synchronous communication দরকার নাকি Asynchronous? **API** contract কেমন হবে? 4️⃣ Operating Behavior

Operating Behavior বলতে বোঝায় System বিভিন্ন পরিস্থিতিতে কীভাবে behave করবে।

বিশেষ করে:

### High Traffic

### Server Failure
### Database Failure
### Network Failure
### Slow Request
### Service Failure
### Sudden Traffic Spike
### Normal Situation
Client
    │
    ▼
Server
    │
    ▼
Database
### High Traffic

যদি Traffic অনেক বেড়ে যায়:

    Client
    │
    ▼
    Load Balancer
    │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
    Server    Server    Server

একাধিক Server ব্যবহার করে Traffic Handle করা যায়।

### Database Load

Database-এর উপর অতিরিক্ত চাপ কমানোর জন্য Cache ব্যবহার করা যেতে পারে:

Client
    │
    ▼
Server
    │
    ▼
 Redis
    │
    ├── Cache Hit → Return Data
    │
    └── Cache Miss
    │
    ▼
    Database
### Background Work

যে কাজগুলো সঙ্গে সঙ্গে করার দরকার নেই, সেগুলো Queue-এর মাধ্যমে Background-এ করা যেতে পারে।

Client
    │
    ▼
**API** Server
    │
    ▼
### Message Queue
    │
    ▼
Worker
    │
    ▼
Email / Notification
### Failure Handling

System-এর কোনো অংশ fail করলে পুরো System যেন অচল না হয়ে যায়—এটিও Design-এর অংশ।

🧠 Golden Rules

System Design-এর সময় কিছু গুরুত্বপূর্ণ Mindset মনে রাখা উচিত।

🥇 Rule 1 — প্রতিটি Component-এর পেছনে কারণ থাকতে হবে

Diagram-এ অনেকগুলো Box থাকলেই Design ভালো হয় না।

উদাহরণ:

Client ↓ ### Load Balancer ↓ **API** Gateway ↓ ### Service Mesh ↓ Microservices ↓ Kafka ↓ Redis ↓ Database

এটি দেখতে Advanced হলেও necessarily ভালো Design নয়।

প্রতিটি Component-এর জন্য প্রশ্ন করতে হবে:

*আমি এটি কেন ব্যবহার করছি?*

যদি কোনো Component-এর নির্দিষ্ট Problem না থাকে, তাহলে সেটি বাদ দেওয়াই ভালো হতে পারে।

🥈 Rule 2 — Simple Design is Better

একটি System-এর জন্য কোনো single Universal Architecture নেই।

সঠিক Design নির্ভর করে:

Requirements Traffic Budget ### Team Size ### Team Expertise ### Business Needs ### Reliability Requirements ### Performance Requirements ### Simple System Client ↓ Backend ↓ Database

অনেক Application-এর জন্য এটিই যথেষ্ট হতে পারে।

### More Complex System

Client ↓ **CDN** ↓ ### Load Balancer ↓ **API** Servers ↓ Cache ↓ Services ↓ ### Message Queue ↓ Database

Complexity তখনই যোগ করা উচিত যখন Requirement-এর কারণে সেটি দরকার।

❌ Most Complex Design ≠ Best Design

✅ Most Appropriate Design = Best Design

🥉 Rule 3 — Requirement অনুযায়ী Design করো

System Design শুরু করার আগে Requirement বুঝতে হবে।

প্রথমে জিজ্ঞেস করো:

### Functional Requirements

System কী কী কাজ করবে?

উদাহরণ:

User can:
- Create account
- Login
- Upload file
- Send message
- Search
Non-Functional Requirements

System কেমনভাবে কাজ করবে?

উদাহরণ:

- Highly available
- Scalable
- Low latency
- Secure
- Reliable
💻 Design বনাম Implementation

একটি গুরুত্বপূর্ণ বিষয়:

Implementation এবং System Design এক জিনিস নয়।

Implementation

Implementation হলো মূলত Code-এর ভিতরের বিস্তারিত কাজ।

উদাহরণ:

Class ↓ Function ↓ Variable ↓ Algorithm ↓ ### Database Query ### System Design

System Design হলো বড় Architecture এবং Component Interaction নিয়ে চিন্তা করা।

Client
   ↓
**API**
   ↓
### Application Server
   ↓
Cache
   ↓
Database
সহজভাবে
Implementation
    ↓
*এই Function কীভাবে কাজ করবে?*

### System Design

    ↓
*এই পুরো System কীভাবে কাজ করবে?*
🎯 System Design Interview Approach

System Design Interview-এ সরাসরি Diagram আঁকা শুরু করা উচিত নয়।

একটি ভালো Flow:

Requirements
      ↓
Clarify
      ↓
### Estimate Scale
      ↓
High-Level Design
      ↓
Database / Data Model
      ↓
**API** / Interfaces
      ↓
Scalability
      ↓
### Failure Handling
      ↓
Trade-offs
Step 1 — Requirements Clarify করো

প্রথমেই প্রশ্ন করো:

Who are the users?

What can users do?

How much traffic?

How much data?

What are the important features?

What are the performance requirements? Step 2 — Scale সম্পর্কে ধারণা নাও

উদাহরণ:

Users          → 10 Million Daily Requests → 50 Million Data           → 10 TB

এগুলো জানা গেলে Architecture সম্পর্কে ভালো সিদ্ধান্ত নেওয়া যায়।

Step 3 — High-Level Architecture তৈরি করো

প্রথমে simple diagram:

Client ↓ ### Load Balancer ↓ ### Application Server ↓ Database

তারপর Requirement অনুযায়ী Component যোগ করো:

Client ↓ ### Load Balancer ↓ ### Application Servers ↓ ┌──────────────┬──────────────┐ ▼              ▼              ▼ Cache        Database       Queue Step 4 — Data Design

চিন্তা করো:

কোন Data Store হবে? **SQL** নাকি NoSQL? কোন Data Cache হবে? Database Schema কেমন হবে? Index কোথায় দরকার? Step 5 — **API** / Interface Design

উদাহরণ:

**POST** /users **GET**  /users/:id **POST** /orders **GET**  /orders/:id

প্রতিটি **API**-এর:

Input Output Authentication Authorization ### Error Handling

নিয়ে চিন্তা করতে হবে।

Step 6 — Scalability

প্রশ্ন করো:

*Traffic 10x হয়ে গেলে কী হবে?*

সম্ভাব্য Solution:

### Horizontal Scaling

### Load Balancing Caching ### Database Replication ### Read Replicas ### Message Queue **CDN** Sharding

সবগুলো একসাথে ব্যবহার করতে হবে না।

Requirement অনুযায়ী ব্যবহার করতে হবে।

Step 7 — Failure Handling

প্রশ্ন করো:

*যদি Database Down হয়ে যায়?*

অথবা:

*যদি একটি Application Server Crash করে?*

অথবা:

*যদি Traffic হঠাৎ 100x বেড়ে যায়?*

System কীভাবে Recover করবে সেটি Design-এর গুরুত্বপূর্ণ অংশ।

🚀 Simple থেকে Large Scale

System Design শেখার সবচেয়ে ভালো পদ্ধতিগুলোর একটি হলো:

Start Small → Understand Requirements → Scale Gradually

Level 1 — Basic
Client
  ↓
Server
  ↓
Database
Level 2 — Scaling
Client
  ↓
### Load Balancer
  ↓
### Multiple Servers
  ↓
Database
Level 3 — Performance
Client
  ↓
### Load Balancer
  ↓
Servers
  ↓
Redis
  ↓
Database
Level 4 — Async Processing
Client
  ↓
**API** Servers
  ↓
Queue
  ↓
Workers
Level 5 — Large Scale
    ┌──► Cache
    │
Client → Load Balancer → **API** Servers
    │
    ├──► Database
    │
    ├──► Queue → Workers
    │
    └──► Object Storage

প্রয়োজন অনুযায়ী System ধীরে ধীরে evolve করবে।

✅ System Design Interview Checklist

Interview-এর সময় নিচের বিষয়গুলো মনে রাখো:

□ Requirements Clarify করেছি? □ Functional Requirements বুঝেছি? □ Non-Functional Requirements বুঝেছি? □ Expected Traffic জানি? □ Data Size সম্পর্কে ধারণা আছে? □ Main Components identify করেছি? □ Database নির্বাচন করেছি? □ Source of Truth নির্ধারণ করেছি? □ **API** / Interfaces design করেছি? □ Cache দরকার কি? □ Load Balancer দরকার কি? □ Async Processing দরকার কি? □ Failure Handling চিন্তা করেছি? □ Scalability চিন্তা করেছি? □ Security চিন্তা করেছি? □ Monitoring / Logging দরকার কি? □ Trade-offs ব্যাখ্যা করতে পারি? ⚖️ Trade-offs

System Design-এ অনেক সময় একটি Solution-এর সাথে অন্য একটি Solution-এর Trade-off থাকে।

উদাহরণ:

Performance  ↔  Consistency

Availability ↔  Consistency

Complexity   ↔  Scalability

Cost         ↔  Performance

তাই Interview-এ শুধু বলতে হবে না:

*আমি Redis ব্যবহার করব।*

বরং বলতে হবে:

"আমি Redis ব্যবহার করব কারণ এই Data frequently accessed এবং caching-এর মাধ্যমে Database Load ও Response Latency কমানো যাবে।"

অর্থাৎ:

Decision ↓ Reason ↓ Trade-off

এটাই গুরুত্বপূর্ণ।

📝 Quick Revision System Design কী?

একটি Software System-এর Architecture এবং বিভিন্ন Component-এর Interaction পরিকল্পনা করার প্রক্রিয়া।

### Core Decisions

## Components ## Data Boundaries ## Interfaces ## Operating Behavior Components

System-এর প্রধান Building Blocks।

Server Database Cache Queue ### Load Balancer Storage ### Data Boundaries

Data কোথায় থাকবে এবং Source of Truth কোনটি তা নির্ধারণ করা।

Interfaces

System-এর বিভিন্ন অংশ কীভাবে যোগাযোগ করবে।

**REST** GraphQL gRPC ### Message Queue Events ### Operating Behavior

System বিভিন্ন পরিস্থিতিতে কীভাবে কাজ করবে।

### High Traffic

### Server Failure ### Database Failure ### Network Failure ### Traffic Spike 💡 Interview Mindset

System Design Interview-এ মনে রাখো:

Don't start with technology.

Start with requirements.

তারপর:

Requirements
     ↓
Problem
     ↓
Architecture
     ↓
Components
     ↓
Data
     ↓
Interfaces
     ↓
Scale
     ↓
Failures
     ↓
Trade-offs
🔥 Key Takeaways
1️⃣ System Design মানে শুধু Diagram আঁকা নয়

System Design হলো:

Problem → Requirements → Architecture → Trade-offs

2️⃣ প্রতিটি Component-এর একটি কারণ থাকতে হবে Why Redis? Why Queue? Why Load Balancer? Why Database Replication? Why Microservices?

প্রতিটি প্রশ্নের উত্তর দিতে পারতে হবে।

3️⃣ Simple Design দিয়ে শুরু করো

প্রথমেই Complex Architecture তৈরি করার দরকার নেই।

Simple ↓ Requirement ↓ Scale ↓ Complexity

Requirement যতটা চাইবে, Architecture ততটাই Complex হবে।

4️⃣ Design এবং Implementation আলাদা Implementation → Code-level details

### System Design

→ System-level architecture 5️⃣ Requirement First

সবচেয়ে গুরুত্বপূর্ণ নিয়ম:

Requirements clear না করে Architecture Design শুরু করা উচিত নয়।

🎯 Final Interview Formula

System Design Interview-এ এই Formula মনে রাখতে পারো:

    **REQUIREMENTS**
    ↓
    **SCALE** / **ESTIMATE**
    ↓
    **HIGH**-**LEVEL** **DESIGN**
    ↓
    **DATA** & **DATABASE**
    ↓
    **API** / **INTERFACES**
    ↓
    **SCALABILITY**
    ↓
    **FAILURE** **HANDLING**
    ↓
    **TRADE**-**OFFS**
🏆 Final Thought

"The best system is not the system with the most components. The best system is the simplest system that correctly satisfies the requirements and can evolve when those requirements grow."

System Design Interview-এ তাই:

Understand the Problem
        ↓
### Clarify Requirements
        ↓
### Start Simple
        ↓
### Explain Your Decisions
        ↓
### Scale When Necessary
        ↓
Discuss Trade-offs
🚀 Keep Learning

System Design শেখার সময় ধীরে ধীরে নিচের Topics-গুলো Cover করা গুরুত্বপূর্ণ:

Scalability ### Load Balancing Caching ### Database Scaling ### Database Replication ### Database Sharding **CAP** Theorem Consistency Availability ### Message Queues Event-Driven Architecture Microservices ### Rate Limiting **CDN** ### Distributed Systems ### Fault Tolerance Monitoring & Observability ⭐ Remember

Start Small. Think Clearly. Scale Gradually. Always Know Why.

Happy System Designing! 🚀

এই **README**-টা **GitHub-এর জন্য সরাসরি copy-paste করার মতো** করে তৈরি করা হয়েছে এবং interview revision-এ
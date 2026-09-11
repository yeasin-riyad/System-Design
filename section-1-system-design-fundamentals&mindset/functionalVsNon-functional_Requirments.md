System Design — Interview Notes

System Design হলো এমন একটি প্রক্রিয়া, যেখানে একটি Software System-এর বিভিন্ন অংশ কীভাবে একে অপরের সাথে কাজ করবে তা নির্ধারণ করা হয়।

সহজভাবে:

System Design = নির্দিষ্ট Requirements পূরণ করার জন্য একটি Software System-এর Architecture, Components, Data Flow এবং Interaction পরিকল্পনা করা।

**REQUIREMENTS**

System Design শুরু করার আগে Requirements পরিষ্কারভাবে বুঝতে হবে।

Requirements মূলত তিনটি গুরুত্বপূর্ণ অংশে ভাগ করা যায়:

### Functional Requirements

Non-Functional Requirements Constraints

সহজভাবে মনে রাখো:

Functional Requirements → **WHAT**? System কী করবে?

Non-Functional Requirements → **HOW** **WELL**? System কত ভালোভাবে করবে?

Constraints → **LIMITS**? কোন সীমাবদ্ধতার মধ্যে করবে?

==================================================

# FUNCTIONAL REQUIREMENTS

Functional Requirements বলে দেয় System কী কী কাজ করবে।

অর্থাৎ:

*User এই System দিয়ে কী করতে পারবে?*

এগুলো মূলত System-এর Features এবং Functionalities।

Example: E-commerce System

User:

Register করতে পারবে Login করতে পারবে Product Search করতে পারবে Product দেখতে পারবে Product Cart-এ Add করতে পারবে Order করতে পারবে Payment করতে পারবে Order Track করতে পারবে

এগুলো Functional Requirements কারণ এগুলো বলে:

System কী করবে?

সহজভাবে:

### Functional Requirement

↓ **WHAT**? ↓ System কী করবে? ↓ Features

Example:

*User Product Search করতে পারবে*

→ Functional Requirement

================================================== ## NON-FUNCTIONAL REQUIREMENTS

Non-Functional Requirements বলে দেয় System কীভাবে এবং কোন Quality-তে কাজ করবে।

অর্থাৎ:

*System-এর functionality কত ভালোভাবে কাজ করবে?*

এগুলো System-এর Quality Attributes নিয়ে কাজ করে।

Example:

*User Product Search করতে পারবে*

→ Functional Requirement

কিন্তু:

*Search **API** 200ms-এর মধ্যে Response দিতে হবে*

→ Non-Functional Requirement

কারণ এখানে System কী করবে সেটা নয়, বরং কত দ্রুত করবে সেটা বলা হয়েছে।

Common Non-Functional Requirements:

Performance Scalability Availability Security Reliability ### Fault Tolerance Maintainability

Performance:

System কত দ্রুত Response দেবে?

Example:

**API** Response Time < 200ms

Scalability:

User বা Traffic বাড়লে System কি Handle করতে পারবে?

Example:

1,**000** Users ↓ **100**,**000** Users ↓ 10 Million Users

Availability:

System কতটা সময় Available থাকবে?

Example:

99.9% Availability 99.99% Availability 99.**999**% Availability

Security:

System কতটা Secure হবে?

Examples:

Authentication Authorization Encryption ### Password Hashing ### Rate Limiting ### Input Validation

Reliability:

System সঠিকভাবে এবং Consistently কাজ করবে কি না।

Example:

### Payment Success

↓ ### Order Created

Payment সফল হওয়ার পর Order হারিয়ে যাওয়া উচিত নয়।

Fault Tolerance:

কোনো Component Fail করলেও System কতটা কাজ চালিয়ে যেতে পারবে।

Example:

Server 1 → **DOWN**

Server 2 → **RUNNING** Server 3 → **RUNNING**

Server 1 Fail করলেও অন্য Server Request Handle করতে পারবে।

Maintainability:

System-এর Code এবং Architecture ভবিষ্যতে কত সহজে Maintain বা Modify করা যাবে।

সহজভাবে:

Non-Functional Requirement ↓ **HOW** **WELL**? ↓ System কত ভালোভাবে কাজ করবে? ↓ Performance Scalability Availability Security Reliability

================================================== ## CONSTRAINTS

Constraint হলো System Design করার সময় আমাদের যে সীমাবদ্ধতা বা বাধাগুলোর মধ্যে থাকতে হবে।

অর্থাৎ:

"কোন সীমার মধ্যে থেকে আমাদের System Design করতে হবে?"

Common Constraints:

Budget Time ### Team Size Technology Infrastructure ### Business Rules Scale

Budget Constraint:

ধরো Infrastructure Budget সর্বোচ্চ $**500**/month।

এটি একটি Constraint।

তখন এমন Architecture ব্যবহার করতে হবে যা Budget-এর মধ্যে থাকবে।

Team Constraint:

ধরো Team-এ মাত্র ৩ জন Developer।

তাহলে খুব Complex Microservices Architecture তৈরি করা Practical নাও হতে পারে।

### Small Team

↓ ### Simple Architecture ↓ ### Modular Monolith

Technology Constraint:

কখনো Organization নির্দিষ্ট Technology ব্যবহার করতে বলতে পারে।

Example:

Must use:

Node.js PostgreSQL **AWS**

এগুলো Design-এর Constraints।

Time Constraint:

ধরো:

***MVP** 2 মাসের মধ্যে Launch করতে হবে।*

এটিও Constraint।

### Simple Architecture

↓ ### Fast Development ↓ **MVP** Launch

Scale Constraint:

ধরো System-কে Handle করতে হবে:

Users → 10 Million Requests → **100**,**000** / second Data → 10 TB

এই Scale Architecture-এর উপর বড় প্রভাব ফেলবে।

================================================== **FUNCTIONAL** VS **NON**-**FUNCTIONAL** VS **CONSTRAINTS**

Functional Requirements:

System কী করবে?

Example:

*User Order করতে পারবে*

Non-Functional Requirements:

System কত ভালোভাবে করবে?

Example:

*Order **API** 200ms-এর মধ্যে Response দেবে*

Constraints:

কোন সীমার মধ্যে করবে?

Example:

*Monthly Infrastructure Budget $**500***

সহজভাবে:

Functional → **WHAT**?

Non-Functional → **HOW** **WELL**?

Constraints → **LIMITS**?

================================================== **REAL**-**WORLD** **EXAMPLE**

ধরো Interviewer বললো:

*Design an Online Food Delivery System.*

প্রথমে Requirements বের করতে হবে।

Functional Requirements:

User Restaurant দেখতে পারবে User Food Search করতে পারবে User Food Order করতে পারবে User Payment করতে পারবে Restaurant Order Receive করবে Delivery Partner Order Receive করবে User Order Track করতে পারবে

Non-Functional Requirements:

Search Response < 200ms ### Highly Available Scalable Secure Reliable Payment Operation Consistent হতে হবে

Constraints:

### Budget Limited

**MVP** 3 মাসের মধ্যে Launch করতে হবে Existing PostgreSQL ব্যবহার করতে হবে Team ছোট # Initially একটি নির্দিষ্ট Region-এ Operate করবে **SYSTEM** **DESIGN**-এর মূল ৪টি সিদ্ধান্ত

একটি Backend System Design করার সময় চারটি গুরুত্বপূর্ণ বিষয়:

Components ### Data Boundaries Interfaces ### Operating Behavior

==================================================

# COMPONENTS

Components হলো System-এর প্রধান Building Blocks।

Examples:

### Application Server

Database Cache ### Message Queue ### Load Balancer ### Object Storage ### Search Engine ### Background Worker

Example:

Client ↓ ### Load Balancer ↓ ### Application Servers ↓ ┌──────────────┬──────────────┐ ↓ ↓ ↓ ### Cache Database Queue ↓ Worker

Common Components:

### Application Server

→ Business Logic Execute করে

Database → Permanent Data Store করে

Redis / Cache → Frequently Accessed Data দ্রুত সরবরাহ করে

### Load Balancer

→ Traffic বিভিন্ন Server-এর মধ্যে ভাগ করে

### Message Queue

→ Asynchronous কাজ পরিচালনা করে

Worker → Background Job Execute করে

### Object Storage

→ Image, Video, File ইত্যাদি Store করে

Important Question:

*এই Component কেন ব্যবহার করছি?*

কোনো Component শুধু জনপ্রিয় বলে Architecture-এ যোগ করা উচিত নয়।

প্রতিটি Component-এর একটি নির্দিষ্ট উদ্দেশ্য থাকতে হবে।

================================================== ## DATA BOUNDARIES

Data Boundaries বলতে বোঝায়:

System-এর কোন Data কোথায় Store হবে এবং কোন Storage হবে সেই Data-এর Source of Truth।

Example:

Application │ ├──────────────┐ ↓ ↓ PostgreSQL Redis │ │ │ └── Cache │ └── Source of Truth

PostgreSQL → Permanent Data

Redis → Cached Data

PostgreSQL → Source of Truth

Source of Truth হলো এমন একটি Data Store যেখানে System-এর মূল এবং authoritative Data সংরক্ষিত থাকে।

================================================== ## INTERFACES

Interfaces হলো System-এর বিভিন্ন অংশ কীভাবে একে অপরের সাথে যোগাযোগ করবে তার মাধ্যম।

Example:

Client │ │ **HTTP** Request ↓ **REST** **API** │ ↓ ### Backend Server

Communication হতে পারে:

**REST** **API** GraphQL gRPC ### Message Queue Events Pub/Sub

Service-to-Service Communication:

### User Service

│ │ **API** / Event ↓ ### Order Service │ │ Event ↓ ### Payment Service

System Design করার সময় প্রশ্ন:

"এই Component অন্য Component-এর সাথে কীভাবে communicate করবে?"

================================================== ## OPERATING BEHAVIOR

Operating Behavior বলতে বোঝায় System বিভিন্ন পরিস্থিতিতে কীভাবে কাজ করবে।

বিশেষ করে:

### High Traffic

### Server Failure ### Database Failure ### Network Failure ### Slow Request ### Traffic Spike ### Service Failure

High Traffic:

Client ↓ ### Load Balancer ↓ ┌────────┬────────┐ ↓ ↓ ↓ ### Server Server Server

Database Load কমানো:

Client ↓ Server ↓ Redis ↓ Database

Background Processing:

Client ↓ **API** Server ↓ ### Message Queue ↓ Worker ↓ Email / Notification / Processing

Failure Handling:

নিজেকে প্রশ্ন করো:

What if the Database goes down?

What if one Application Server crashes?

What if the Network fails?

What if Traffic suddenly increases?

What if a Third-party Service is unavailable?

================================================== **GOLDEN** **RULES**

Rule 1: প্রতিটি Component-এর পেছনে কারণ থাকতে হবে।

Diagram-এ অনেকগুলো Box থাকলেই ভালো System Design হয় না।

নিজেকে প্রশ্ন করো:

Why Redis?

Why Queue?

Why Load Balancer?

Why **CDN**?

Why Microservices?

Why Database Replication?

Rule 2: Simple Design is Better.

কোনো System-এর জন্য একটি Universal Solution নেই।

Design নির্ভর করে:

Requirements Traffic Budget ### Team Size ### Team Expertise ### Business Needs Performance Reliability

Most Complex Design ≠ Best Design

Most Appropriate Design = Best Design

Rule 3: Requirement First.

Requirements clear না করে Architecture Design শুরু করা উচিত নয়।

================================================== # DESIGN VS IMPLEMENTATION

Implementation এবং System Design এক জিনিস নয়।

Implementation:

Code-level details

Class ↓ Function ↓ Variable ↓ Algorithm ↓ ### Database Query

System Design:

System-level Architecture

Client ↓ **API** ↓ ### Application Server ↓ Cache ↓ Database

সহজভাবে:

Implementation:

*এই Function কীভাবে কাজ করবে?*

System Design:

*এই পুরো System কীভাবে কাজ করবে?*

================================================== # SYSTEM DESIGN INTERVIEW APPROACH

System Design Interview-এ সরাসরি Diagram আঁকা শুরু করা উচিত নয়।

একটি ভালো Flow:

Requirements ↓ ### Clarify Requirements ↓ ### Estimate Scale ↓ High-Level Design ↓ Database / Data Model ↓ **API** / Interfaces ↓ Scalability ↓ ### Failure Handling ↓ Trade-offs

Requirements Clarify করো

প্রথমে বুঝতে হবে:

Who are the users?

What can users do?

How much traffic?

How much data?

What are the important features?

What are the performance requirements?

What are the availability requirements?

Are there any constraints?

Scale Estimate করো

Example:

Users → 10 Million Daily Requests → 50 Million Data → 10 TB Requests/Second → 10,**000**+

High-Level Architecture তৈরি করো

প্রথমে Simple Architecture:

Client ↓ ### Load Balancer ↓ ### Application Server ↓ Database

তারপর Requirements অনুযায়ী Components যোগ করো:

Client ↓ ### Load Balancer ↓ ### Application Servers │ ├── Cache │ ├── Database │ └── Message Queue ↓ Worker

### Data Design

চিন্তা করতে হবে:

কোন Data Store ব্যবহার করব? **SQL** নাকি NoSQL? কোন Data Cache করা হবে? Database Schema কেমন হবে? কোন Field-এ Index দরকার? Source of Truth কোথায়? **API** / Interface Design

Example:

**POST** /users **GET** /users/:id

**POST** /orders **GET** /orders/:id

**POST** /payments **GET** /payments/:id

প্রতিটি **API**-এর ক্ষেত্রে চিন্তা করতে হবে:

Request Response Authentication Authorization Validation ### Error Handling ### Rate Limiting Scalability

Interview-এ গুরুত্বপূর্ণ প্রশ্ন:

*Traffic যদি 10x হয়ে যায় তাহলে কী হবে?*

Possible Solutions:

### Horizontal Scaling

### Load Balancing Caching ### Database Replication ### Read Replicas ### Message Queue **CDN** ### Database Sharding

সবগুলো একসাথে ব্যবহার করতে হবে না।

Requirement অনুযায়ী Solution নির্বাচন করতে হবে।

### Failure Handling

নিজেকে প্রশ্ন করো:

What if the Database goes down?

What if one Application Server crashes?

What if the Network fails?

What if Traffic suddenly increases?

What if a Third-party Service is unavailable?

================================================== **TRADE**-**OFFS**

System Design-এ প্রায় সব Decision-এর কিছু না কিছু Trade-off থাকে।

Examples:

Performance ↔ Consistency

Availability ↔ Consistency

Complexity ↔ Scalability

Cost ↔ Performance

Interview-এ শুধু বলা উচিত নয়:

*আমি Redis ব্যবহার করব।*

বরং বলতে হবে:

"আমি Redis ব্যবহার করব কারণ এই Data frequently accessed এবং Cache ব্যবহার করলে Database Load ও Response Latency কমানো যাবে।"

অর্থাৎ:

Decision ↓ Reason ↓ Trade-off

================================================== **SIMPLE** থেকে **LARGE** **SCALE**

System Design শেখার একটি ভালো পদ্ধতি:

### Start Small

↓ ### Understand Requirements ↓ ### Scale Gradually

Level 1:

Client ↓ Server ↓ Database

Level 2:

Client ↓ ### Load Balancer ↓ ### Multiple Servers ↓ Database

Level 3:

Client ↓ ### Load Balancer ↓ Servers ↓ Redis ↓ Database

Level 4:

Client ↓ **API** Servers ↓ ### Message Queue ↓ Workers

Level 5:

Client ↓ ### Load Balancer ↓ **API** Servers ├── Cache ├── Database ├── Queue → Workers └── Object Storage

================================================== # SYSTEM DESIGN INTERVIEW CHECKLIST

[ ] Requirements Clarify করেছি?

[ ] Functional Requirements বুঝেছি?

[ ] Non-Functional Requirements বুঝেছি?

[ ] Constraints বুঝেছি?

[ ] Expected Traffic জানি?

[ ] Data Size সম্পর্কে ধারণা আছে?

[ ] Main Components identify করেছি?

[ ] Database নির্বাচন করেছি?

[ ] Source of Truth নির্ধারণ করেছি?

[ ] **API** / Interfaces design করেছি?

[ ] Cache দরকার কি?

[ ] Load Balancer দরকার কি?

[ ] Async Processing দরকার কি?

[ ] Failure Handling চিন্তা করেছি?

[ ] Scalability চিন্তা করেছি?

[ ] Security চিন্তা করেছি?

[ ] Monitoring / Logging দরকার কি?

[ ] Trade-offs ব্যাখ্যা করতে পারি?

================================================== **QUICK** **REVISION**

System Design:

Software System-এর Architecture, Components, Data Flow এবং Interaction কীভাবে কাজ করবে তা পরিকল্পনা করার প্রক্রিয়া।

Functional Requirements:

System কী করবে?

Examples:

Register Login Search Order Payment

Non-Functional Requirements:

System কত ভালোভাবে কাজ করবে?

Examples:

Performance Scalability Availability Security Reliability ### Fault Tolerance Maintainability

Constraints:

কোন সীমাবদ্ধতার মধ্যে System তৈরি করতে হবে?

Examples:

Budget Time ### Team Size Technology Infrastructure ### Business Rules # Scale **ONE**-**MINUTE** **REVISION**

### Functional Requirements

↓ **WHAT**? ↓ System কী করবে?

Non-Functional Requirements ↓ **HOW** **WELL**? ↓ System কত ভালোভাবে করবে?

Constraints ↓ **LIMITS**? ↓ কোন সীমার মধ্যে করবে?

================================================== **KEY** **TAKEAWAYS** System Design মানে শুধু Diagram আঁকা নয়।

System Design:

Problem ↓ Requirements ↓ Architecture ↓ Trade-offs

### Requirements First

Requirements clear না করে Architecture Design শুরু করা উচিত নয়।

Functional = What

User কী করতে পারবে?

→ Functional Requirement

Non-Functional = How Well

System কত দ্রুত?

কতটা Scalable?

কতটা Available?

কতটা Secure?

→ Non-Functional Requirement

Constraints = Limits

Budget Time Team Technology Infrastructure

→ Constraints

Every Component Needs a Reason

Component ↓ Problem ↓ Solution

Simple Design is Better

সবচেয়ে Complex Design মানেই সবচেয়ে ভালো Design নয়।

সঠিক Design হলো এমন Design যা Requirements পূরণ করে এবং অপ্রয়োজনীয় Complexity এড়ায়।

### Scale Gradually

### Start Simple

↓ ### Understand Requirements ↓ ### Estimate Scale ↓ ### Add Complexity ↓ ### Scale Gradually

==================================================
# FINAL INTERVIEW FORMULA
    **REQUIREMENTS**
    ↓
    ┌───────────┼───────────┐
    ↓           ↓           ↓

Functional Non-Functional Constraints │ │ │ └───────────┼───────────┘ ↓ **SCALE** **ESTIMATION** ↓ **HIGH**-**LEVEL** **DESIGN** ↓ **DATA** & **DATABASE** ↓ **API** / **INTERFACES** ↓ **SCALABILITY** ↓ **FAILURE** **HANDLING** ↓ **TRADE**-**OFFS**

================================================== **FINAL** **THOUGHT**

সবচেয়ে ভালো System হলো এমন System নয় যেখানে সবচেয়ে বেশি Components আছে।

সবচেয়ে ভালো System হলো এমন System যা:

Requirements সঠিকভাবে পূরণ করে Constraints মেনে চলে অপ্রয়োজনীয় Complexity এড়ায় Scalable Reliable Maintainable Future Requirements অনুযায়ী Evolve করতে পারে

System Design Interview-এ মনে রাখো:

Understand the Problem ↓ ### Clarify Requirements ↓ ### Identify Constraints ↓ ### Start Simple ↓ ### Explain Your Decisions ↓ ### Scale When Necessary ↓ Discuss Trade-offs

================================================== **REMEMBER**

**START** **SMALL** ↓ **UNDERSTAND** **REQUIREMENTS** ↓ **IDENTIFY** **CONSTRAINTS** ↓ **DESIGN** **CLEARLY** ↓ **KNOW** **WHY** ↓ **SCALE** **GRADUALLY** ↓ **UNDERSTAND** **TRADE**-**OFFS**

"Start Small. Think Clearly. Scale Gradually. Always Know Why."
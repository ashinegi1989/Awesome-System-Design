ji
#System-Design
System Design Interview

#Question 1
𝐇𝐨𝐰 𝐭𝐨 𝐢𝐦𝐩𝐫𝐨𝐯𝐞 𝐝𝐚𝐭𝐚𝐛𝐚𝐬𝐞 𝐩𝐞𝐫𝐟𝐨𝐫𝐦𝐚𝐧𝐜𝐞?
Here are the top ways to improve database performance:
1. Indexing
Create the right indexes based on query patterns to speed up data retrieval.

2. Materialized Views
Store pre-computed query results for quick access, reducing the need to process complex queries repeatedly.

3. Vertical Scaling
Increase the capacity of the hashtag
#database server by adding more CPU, RAM, or storage.

4. Denormalization
Reduce complex joins by restructuring data, which can improve query performance.

5. Database Caching
Store frequently accessed data in a faster storage layer to reduce load on the database.

6. Replication
Create copies of the primary database on different servers to distribute read load and enhance availability.

7. Sharding
Divide the database into smaller, manageable pieces, or shards, to distribute load and improve performance.

8. Partitioning
Split large tables into smaller, more manageable pieces to improve query performance and maintenance.

9. Query Optimization
Rewrite and fine-tune queries to execute more efficiently.

10. Use of Appropriate Data Types
Select the most efficient data types for each column to save space and speed up processing.

11. Limiting Indexes
Avoid excessive indexing, which can slow down write operations; use indexes judiciously.

12. Archiving Old Data
Move infrequently accessed data to an archive to keep the active database smaller and faster.

#Question 2
20 Popular Open Source Projects Started or Supported By Big Companies

Google
- Kubernetes
- TensorFlow
- Go
- Angular

Meta
- React
- PyTorch
- GraphQL
- Cassandra

Microsoft
- VSCode
- TypeScript
- Playwright

Netflix
- Chaos Monkey
- Hystrix
- Zuul

LinkedIn
- Kafka
- Samza
- Pinot

RedHat
- Ansible
- OpenShift
- Ceph Storage

3: List of Distributed databse vs Reqular Database
Here are some examples:

Distributed Databases:

1. Google Bigtable
2. Apache Cassandra
3. Apache HBase
4. MongoDB
5. Amazon DynamoDB

Regular Databases:

1. MySQL
2. PostgreSQL
3. Microsoft SQL Server
4. Oracle Database
5. IBM DB2


******************
Reverse proxy vs forward proxy

Here are concise examples and tools for forward and reverse proxies:

## Forward Proxy Example
- *Squid*: A popular open-source forward proxy server.
- *Use case*: Block access to specific websites, cache frequently accessed resources, and monitor internet usage.

## Reverse Proxy Example
- *NGINX*: A popular open-source reverse proxy server.
- *Use case*: Distribute incoming traffic across multiple servers, improve security, and cache frequently accessed resources.

## Tools
- *NGINX*: Can be used as both a forward and reverse proxy.
- *Squid*: Primarily used as a forward proxy.
- *HAProxy*: A popular load balancer and reverse proxy.
- *AWS Elastic Load Balancer (ELB)*: A cloud-based load balancer that can act as a reverse proxy.

These tools can help you manage traffic, improve security, and optimize performance for your applications.

**********************
Event Loop

Here's a concise explanation:

## What is an Event Loop?
An event loop is a design pattern that allows a program to handle multiple tasks and events concurrently by registering callbacks and processing them in a loop. It's a way to manage asynchronous operations and improve responsiveness.

## Relation with Single Thread
In single-threaded models, event loops are used to handle multiple tasks and events without blocking. The event loop yields control back to the program, allowing other tasks to be processed while waiting for I/O operations or other events to complete.

## Example
Here's an example of a single-threaded event loop in Node.js:
const fs = require('fs');

fs.readFile('file.txt', (err, data) => {
  console.log(data.toString());
});

console.log('Reading file...');
In this example, Node.js uses an event loop to handle the `readFile` operation asynchronously. While waiting for the file to be read, Node.js can process other tasks, such as logging "Reading file...". When the file is read, the callback is executed, logging the file contents

example:
1. Node.js
2. JavaScript (web browsers)
3. Redis
4. Nginx
5. libuv
6. Twisted
7. EventMachine
8. Netty
9. Vert.x
10 years
10. Tornado
*****************

*How Event Loop Handles Requests and Responses*

When an event loop sends a request to a backend API, it doesn't maintain a persistent connection with the backend. Instead, it uses a mechanism called *async callbacks* or *async notifications* to handle responses.

## Here's a high-level overview of the process:
1. *Request Sent*: The event loop sends a request to the backend API.
2. *Connection Closed*: The connection is closed after sending the request.
3. *Backend Processing*: The backend API processes the request.
4. *Response Sent*: The backend API sends a response back to the event loop.
5. *Callback Notification*: The event loop is notified about the incoming response through a callback mechanism.

## How Event Loop Gets Response without Maintaining Connection
The event loop uses various techniques to get responses from the backend API without maintaining a persistent connection:

1. *Polling*: The event loop can poll the backend API for responses.
2. *Callbacks*: The backend API can use callbacks to notify the event loop about responses.
3. *Async Notifications*: The event loop can receive async notifications from the backend API when a response is available.

## Underlying Mechanisms
The event loop uses underlying mechanisms like:

1. *Socket Programming*: The event loop uses socket programming to send and receive data over the network.
2. *TCP/IP*: The event loop relies on TCP/IP protocol to ensure reliable data transfer between the client and server.

## How Event Loop Knows Response is Coming
The event loop knows a response is coming through various mechanisms:

1. *Socket Notifications*: The event loop receives notifications when data is available on the socket.
2. *Callback Functions*: The event loop uses callback functions to handle responses.

By using these mechanisms, the event loop can efficiently handle requests and responses without maintaining a persistent connection with the backend API.

***********

Memcached vs Redis

System Design DropBox:


================================================================================
SYSTEM DESIGN GUIDE: DATABASE PARADIGMS, TOPOLOGIES, AND EVOLUTION
================================================================================
--------------------------------------------------------------------------------
1. SHARED DATABASE WITH READ REPLICAS (MONOLITHIC RELATIONAL)
--------------------------------------------------------------------------------
* Legacy Example: MySQL 5.6
* Modern Cloud Example: Amazon Aurora
* Popular Industry Use Case: E-commerce & Retail Apps (e.g., Early Shopify, retail storefronts).
* Why It Is Used There: The primary node handles high-speed checkouts and cart updates. 
  Read replicas serve product catalog browsing and customer history pages without slowing 
  down the checkout experience.
* Detailed Description: This structure splits database traffic by operation type. 
  A single Primary database node handles all writes, while streaming transaction logs 
  to one or more Read Replicas that handle read-only queries. This offloads heavy reporting 
  workloads from the main transactional engine.

[PRACTICAL QUERY CODE]
-- WRITE TRANSACTION (Routed by the App to the PRIMARY instance)
INSERT INTO orders (customer_id, order_total, status) 
VALUES (101, 250.00, 'Completed');

-- READ-ONLY WORKLOAD (Routed by the App to the READ REPLICA instance)
SELECT customer_id, SUM(order_total) 
FROM orders 
WHERE status = 'Completed' 
GROUP BY customer_id;


--------------------------------------------------------------------------------
2. DISTRIBUTED APPS WITH APPLICATION-SIDE SHARDING (SHARED-NOTHING)
--------------------------------------------------------------------------------
* Legacy Example: Oracle 11g (Manually partitioned shards)
* Modern Distributed SQL Example: CockroachDB
* Popular Industry Use Case: Core Banking & Global Payment Gateways (e.g., Fintech apps).
* Why It Is Used There: Banking requires absolute data accuracy (ACID compliance) and data 
  residency. Sharding keeps a European user's data on European servers and a US user's data 
  on US servers to comply with privacy laws, while operating as a single unified platform.
* Detailed Description: Data is horizontally sliced into distinct rows (shards) and distributed 
  across isolated database servers. In legacy systems, the application code had to hold the 
  "routing map" to find data. Modern Distributed SQL engines handle this data distribution 
  completely behind the scenes while maintaining strict data consistency across global regions.

[PRACTICAL QUERY CODE]
-- CockroachDB automates the shard lookup transparently across global regions:
SELECT customer_name, region 
FROM global_customers 
WHERE region IN ('US-East', 'EU-West') 
AND account_balance > 10000;


--------------------------------------------------------------------------------
3. SHARED-DISK ARCHITECTURE (DATABASE CLUSTERS)
--------------------------------------------------------------------------------
* Legacy Example: Oracle RAC (Real Application Clusters)
* Modern Equivalent: Microsoft Azure SQL Database HyperScale
* Popular Industry Use Case: Airlines & Enterprise Resource Planning (ERP) Systems.
* Why It Is Used There: These systems require massive compute power to handle thousands of 
  employees or travelers updating highly connected, centralized records simultaneously. 
  If a hardware node fails, the flight booking system stays online without data loss.
* Detailed Description: Instead of splitting up data, Shared-Disk architecture keeps all data 
  in a centralized, massive physical storage pool. Multiple independent compute servers sit 
  on top of this single storage layer. If one compute server crashes, another node instantly 
  takes over because it is looking at the exact same physical storage disk.

[PRACTICAL QUERY CODE]
-- Run simultaneously from Server Node 1 AND Server Node 2 
-- Both compute servers read from the shared central disk layout concurrently.
SELECT product_id, inventory_count 
FROM central_warehouse_stock 
WHERE safety_stock_level < 10;


--------------------------------------------------------------------------------
4. MASSIVELY PARALLEL PROCESSING (MPP) DATA WAREHOUSES
--------------------------------------------------------------------------------
* Legacy Example: Teradata / Greenplum
* Modern Equivalent: Amazon Redshift (RA3 instances)
* Popular Industry Use Case: Telecommunications & Telecom Network Analytics (e.g., AT&T, Vodafone).
* Why It Is Used There: Telecom companies generate billions of call detail records daily. 
  MPP systems are used to scan through months of historical connection logs to detect dropped 
  call trends, network coverage gaps, and fraudulent user activity.
* Detailed Description: Designed specifically for deep historical analytics rather than daily 
  transactions. An MPP system binds compute and storage tightly together into internal 
  "worker nodes." When a massive multi-million-row query arrives, a leader node breaks the 
  query into fragments and forces all worker nodes to scan their local disks in parallel.

[PRACTICAL QUERY CODE]
-- The leader node distributes this query across dozens of worker nodes to aggregate billions of rows.
SELECT 
    date_trunc('month', sale_date) AS sale_month,
    product_category,
    SUM(revenue) AS total_revenue,
    COUNT(DISTINCT customer_id) AS unique_buyers
FROM massive_sales_fact
GROUP BY 1, 2
ORDER BY total_revenue DESC;


--------------------------------------------------------------------------------
5. NOSQL DISTRIBUTED DATABASES (NON-RELATIONAL)
--------------------------------------------------------------------------------
* Legacy Example: Early self-managed MongoDB / Apache Cassandra clusters
* Modern Cloud NoSQL Example: Amazon DynamoDB / MongoDB Atlas
* Popular Industry Use Case: Social Media & Real-Time Activity Feeds (e.g., Instagram, X, Netflix profiles).
* Why It Is Used There: Social platforms handle unpredictable, semi-structured data (likes, comments, 
  varying post lengths) from hundreds of millions of users. Speed is critical; retrieving a user profile 
  must happen in milliseconds, which NoSQL achieves by avoiding slow table joins.
* Detailed Description: NoSQL abandoned traditional tables, rows, and rigid structural schemas to achieve 
  global internet scale. It stores data as flexible documents (JSON) or simple Key-Value pairs across 
  clusters of cheap servers. It scales horizontally to handle millions of requests per second but cannot 
  perform complex analytical SQL table joins efficiently.

[PRACTICAL QUERY CODE]
// STEP 1: Insert a flexible, nested JSON document containing unpredictable user data.
db.users.insertOne({
    "user_id": "usr_9921",
    "name": "Alice Smith",
    "preferences": { "theme": "dark", "notifications": true },
    "login_history": ["2026-07-16", "2026-07-17"]
});

// STEP 2: Instantly retrieve the record using a direct key lookup.
db.users.findOne({ "user_id": "usr_9921" });


--------------------------------------------------------------------------------
6. CLOUD-NATIVE SEPARATED ARCHITECTURE (THE SNOWFLAKE MODEL)
--------------------------------------------------------------------------------
* Modern Example: Snowflake (Multi-Cluster Shared Data)
* Popular Industry Use Case: Business Intelligence (BI), Data Science, & Multi-Source Enterprise Analytics.
* Why It Is Used There: Enterprises collect data from dozens of different sources (Salesforce, Google 
  Analytics logs, production SQL). Snowflake acts as the single central source of truth where data scientists 
  and business analysts run workloads simultaneously without cross-team resource competition.
* Detailed Description: Snowflake completely separated the database into three independent tiers: Storage, 
  Compute (Virtual Warehouses), and Cloud Services. Data is stored cheaply in cloud object storage, while 
  compute power can be spun up or down instantly as isolated clusters. This ensures that a heavy data loading 
  pipeline and a corporate dashboard query run on completely separate hardware resources, looking at the same 
  data without ever slowing each other down.

[PRACTICAL QUERY CODE]
-- Snowflake queries standard structured tables and raw JSON strings simultaneously.
SELECT 
    u.user_id,
    u.name,
    -- The colon (:) operator extracts fields directly out of raw, variant JSON
    u.raw_json_preferences:theme::string AS user_theme,
    SUM(s.revenue) AS total_spent
FROM snowflake_structured_users u
JOIN snowflake_sales_fact s ON u.user_id = s.user_id
WHERE u.raw_json_preferences:notifications = true
GROUP BY u.user_id, u.name, user_theme;


================================================================================
WHY THE LEGACY STRUCTURES ARCHITECTURALLY FAILED
================================================================================
As data scaled into Terabytes and Petabytes, older database models hit three fatal bottlenecks:

1. COUPLED COMPUTE AND STORAGE: In MPP and NoSQL systems, if data disk space filled up, you were 
   forced to buy more server nodes. This meant you paid for extra CPU and RAM you didn't actually 
   need, driving engineering costs sky-high.

2. SEVERE RESOURCE CONTENTION: Compute power was finite. If an automated data pipeline was running 
   a heavy data load, and a data scientist ran a massive query at the same time, the entire system 
   would freeze or crash. The apps fought over the exact same CPU cycles.

3. THE SCHEMA AND QUERY DIVIDE: Relational systems choked on semi-structured data like JSON, XML, or 
   IoT logs. Conversely, NoSQL databases could store JSON easily, but they couldn't perform complex 
   analytical operations (like SQL JOINs) across different data sets efficiently. Snowflake resolved 
   this by decoupling compute from storage entirely.
================================================================================





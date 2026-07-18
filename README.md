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


System Design: Database Architecture
1. Shared Database with Read Replicas (Monolithic Relational)Legacy Example: MySQL 5.6Modern Cloud Example: Amazon AuroraDetailed Description: This structure separates database traffic by the type of operation. A single Primary (Master) database node acts as the single source of truth and handles all data modifications. It constantly streams its transaction logs to one or more Read Replicas, which maintain near-identical copies of the data. The primary drawback is that if your reporting applications flood the read replicas with massive queries, those replicas can fall behind the master (replication lag), causing your reports to show outdated data.Practical Query Code:sql-- WRITE TRANSACTION (Routed by the App to the PRIMARY instance)
-- This ensures data integrity by preventing multi-node conflicts.
INSERT INTO orders (customer_id, order_total, status) 
VALUES (101, 250.00, 'Completed');

-- READ-ONLY WORKLOAD (Routed by the App to the READ REPLICA instance)
-- Offloads heavy aggregation from the master node so checkout processes don't slow down.
SELECT customer_id, SUM(order_total) 
FROM orders 
WHERE status = 'Completed' 
GROUP BY customer_id;
Use code with caution.2. Distributed Apps with Application-Side Sharding (Shared-Nothing)Legacy Example: Oracle 11g (Manually partitioned shards)Modern Distributed SQL Example: CockroachDBDetailed Description: To break past the limits of a single database server, data is horizontally sliced into distinct rows (shards) and distributed across isolated database servers. In the legacy approach, the databases were completely "blind" to each other, forcing your software application to house the routing map (e.g., knowing that Customer 101 lives on Server A, and Customer 505 lives on Server B). Modern Distributed SQL engines fix this by making the database cluster look like a single database to your application, automatically routing queries and managing data balance behind the scenes.Practical Query Code:sql-- In legacy systems, your app code had to establish separate network connections 
-- to "US_Database" and "EU_Database", run queries independently, and merge them.
-- In modern CockroachDB, you run this single query, and the engine fetches the shards:
SELECT customer_name, region 
FROM global_customers 
WHERE region IN ('US-East', 'EU-West') 
AND account_balance > 10000;
Use code with caution.3. Shared-Disk Architecture (Database Clusters)Legacy Example: Oracle RAC (Real Application Clusters)Modern Equivalent: Microsoft Azure SQL Database HyperScaleDetailed Description: Unlike sharding where data is split up, Shared-Disk architecture keeps all data together in a massive, centralized physical storage pool (like a Storage Area Network). Multiple independent compute servers sit on top of this storage pool. If Server 1 goes down, Server 2 instantly takes over because it is looking at the exact same disk storage. The structural limitation is the storage network itself—as you add more compute servers, they eventually choke the storage network bus trying to read and write to the same central disks simultaneously.Practical Query Code:sql-- You can run this exact query at the exact same millisecond 
-- from Server Node 1 AND Server Node 2. 
-- Both compute servers read from the shared central disk layout concurrently.
SELECT product_id, inventory_count 
FROM central_warehouse_stock 
WHERE safety_stock_level < 10;
Use code with caution.4. Massively Parallel Processing (MPP) Data WarehousesLegacy Example: Teradata / GreenplumModern Equivalent: Amazon Redshift (RA3 instances)Detailed Description: Designed specifically for analytics rather than day-to-day transactions. An MPP system binds compute (CPU/RAM) and storage (Disk) tightly together into internal "worker nodes" governed by a "leader node". When a massive query arrives, the leader node breaks the execution plan into micro-tasks and distributes them across the worker nodes, which scan their local disks in parallel. However, if your data outgrows the disks, you are forced to buy more worker nodes, paying for expensive, unused CPU just to get more storage capacity.Practical Query Code:sql-- The leader node distributes this query across dozens of worker nodes.
-- Each worker scans a specific chunk of the 'massive_sales_fact' table 
-- on its local disk, and the leader aggregates the final result.
SELECT 
    date_trunc('month', sale_date) AS sale_month,
    product_category,
    SUM(revenue) AS total_revenue,
    COUNT(DISTINCT customer_id) AS unique_buyers
FROM massive_sales_fact
GROUP BY 1, 2
ORDER BY total_revenue DESC;
Use code with caution.5. NoSQL Distributed Databases (Non-Relational)Legacy Example: Early self-managed MongoDB / Apache Cassandra clustersModern Cloud NoSQL Example: Amazon DynamoDB / MongoDB AtlasDetailed Description: NoSQL abandoned traditional tables, rows, columns, and rigid schemas to achieve global web scale. Instead of forcing data into structured relationships, it stores data as flexible documents (JSON) or simple Key-Value pairs across standard commodity servers. It scales horizontally across thousands of machines seamlessly. The structural trade-off is that NoSQL cannot perform heavy analytical analytical queries (like joining five different tables together) efficiently, making it perfect for operational apps but poor for deep business analytics.Practical Query Code:json// NoSQL uses JSON API methods instead of standard relational SQL.
// STEP 1: Insert a flexible, nested JSON document containing unpredictable user data.
db.users.insertOne({
    "user_id": "usr_9921",
    "name": "Alice Smith",
    "preferences": { "theme": "dark", "notifications": true },
    "login_history": ["2026-07-16", "2026-07-17"]
});

// STEP 2: Instantly target and retrieve the record using a direct key lookup.
db.users.findOne({ "user_id": "usr_9921" });
Use code with caution.6. Cloud-Native Separated Architecture (The Snowflake Model)Modern Example: Snowflake (Multi-Cluster Shared Data)Detailed Description: Snowflake completely broke away from legacy constraints by separating the database into three completely independent tiers: Storage, Compute, and Cloud Services. Data is stored cheaply in cloud object storage. Compute power is spun up as independent, isolated "Virtual Warehouses". This means your data loading process can run on its own compute cluster, while your analytics team runs heavy reports on a separate compute cluster. Because they are isolated, they never compete for hardware resources, eliminating concurrency slowdowns entirely while allowing native SQL queries directly over raw JSON data.Practical Query Code:sql-- Snowflake combines the relational features of SQL with the flexibility of NoSQL.
-- It queries standard tables and raw JSON text strings simultaneously.
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
Use code with caution.



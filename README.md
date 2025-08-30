ji
# Awesome-System-Design
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
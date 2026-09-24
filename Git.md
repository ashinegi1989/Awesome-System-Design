Applied AI Master class
# Principal System Design Interview Questions

This repository tracks deep-dive architectural challenges, failure modes, and trade-offs required for Principal Software Engineer (L7+) roles.

---

## 1. Cricbuzz Text Commentary (Massive Fan-out)
* **Thundering Herd:** How do you prevent database connection spikes if edge caches expire precisely when a major match event occurs?
* **Connection Exhaustion:** How do you architect the load-balancing layer to prevent memory or port exhaustion with 5 million concurrent WebSockets?
* **Out-of-Order Delivery:** How do you guarantee strict client-side message ordering despite varying network path latencies?
* **Degraded State:** What is the fallback strategy for user experience if the real-time push cluster experiences a partial outage?

## 2. Distributed Task Scheduler (Reliability & Scale)
* **Zombie Workers:** How does the system detect mid-execution worker crashes and reassign non-idempotent tasks safely without double-execution?
* **Clock Skew:** How do you prevent server clock drift from causing duplicate executions or execution order violations?
* **Hotspot Partitioning:** How do you prevent database partition hotspots when a single merchant schedules millions of concurrent tasks?
* **Backpressure Control:** How do you throttle task submission if downstream execution pools become severely congested?

## 3. Flash Sale (High Concurrency & Atomic Inventory)
* **Cache Recovery:** What is your automated recovery strategy to prevent data desynchronization if the Redis inventory cluster crashes mid-sale?
* **Fraud Prevention:** How do you enforce idempotency against malicious network retries across the API Gateway and database layers?
* **Downstream Sinks:** How do you prevent slow third-party payment gateways from backing up the primary fulfillment Kafka queue?
* **Database Fallback:** How do you safely persist final transaction states to disk without causing locking contention on your primary relational DB?

## 4. Impressions Counting at Scale (Massive Write-Heavy)
* **Exactly-Once Semantics:** How do you design stream pipelines (Flink/Spark) to guarantee exactly-once aggregations during broker restarts?
* **Late-Arriving Events:** How do you handle deep out-of-order data from offline clients within your time-window aggregations?
* **Storage Tiering:** How do you design an automated Hot/Warm/Cold data lifecycle to control cloud costs without hurting OLAP query speed?
* **Schema Evolution:** How do you handle breaking analytical schema changes without interrupting continuous high-throughput event ingestion?

## 5. Ride Hailing Service (Geospatial Matching)
* **Spatial Amplification:** How do you scale in-memory spatial indexes (H3/S2) to handle extreme write amplification during high-density localized events?
* **Race Conditions:** How does the distributed state machine atomically lock driver states during simultaneous matching requests?
* **Telemetry Backpressure:** How does the ingestion pipeline absorb sudden connection waves when thousands of drivers exit a cellular dead zone?
* **Stale State Cleanup:** How do you handle garbage collection of orphaned location pings without impacting real-time search latencies?

---

## 6. Meta & Platform Governance Questions
* **Zero-Downtime Migration:** How do you execute a major database schema migration under live peak traffic without performance degradation?
* **Cost-Benefit Analysis:** What are the exact infrastructure cost trade-offs of choosing cloud-native managed services versus self-hosting open-source clusters?
* **Multi-Tenant Isolation:** How do you implement resource quotas and rate limiting to prevent one internal product team from impacting others?

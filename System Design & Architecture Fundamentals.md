# System Design & Architecture: Main Points

Based on the video **System Design & Architecture Course | Load Balancers, Caching, Queues, Sharding and more** by *JavaScript Mastery*.

## 1. The Starting Point
* Most applications begin with a single server and a single database. This simple setup is surprisingly robust and can handle thousands of users.
* The challenge arises when sudden traffic spikes (e.g., concert ticket sales) overwhelm the single server, causing requests to pile up, hang, or timeout.

## 2. Vertical vs. Horizontal Scaling
* **Vertical Scaling:** Upgrading to a bigger, more powerful server (more CPU/RAM). It's the easiest first step and requires no code changes, but it has a physical and financial limit.
* **Horizontal Scaling:** Running multiple identical copies of your application across several ordinary servers to share the load. This approach is highly scalable and has virtually no ceiling.

## 3. Load Balancers
* When using multiple servers, you need a **Load Balancer** (e.g., Nginx).
* It stands in front of your servers and distributes incoming user requests evenly so no single machine gets overwhelmed.
* *Trade-offs:* A load balancer introduces a single point of failure (so it requires its own backup) and demands that all backend servers are "interchangeable."

## 4. Session Management
* Horizontal scaling breaks default, in-memory session handling. If a user logs in on Server A, but their next click is routed to Server B, they will appear logged out.
* *Solution:* Move session data into a central **Shared Store** so any server can verify any logged-in user.

## 5. Database Optimization (Replicas)
* As traffic grows, multiple servers hitting a single database creates a new bottleneck.
* *Solution:* Implement connection pooling and **Read Replicas** to split read traffic across multiple copies of the database, keeping the primary database dedicated to writes.

## 6. Caching
* For expensive or frequently requested answers that don't constantly change, compute them once and store the result in a **Cache**. This drastically reduces database load.

## 7. Message Queues & Background Workers
* Slow operations (like processing payments or sending emails) make users wait if executed synchronously.
* *Solution:* Push slow tasks to a **Queue**. Dedicated background workers will process these tasks asynchronously, allowing the main app to respond to the user immediately.

## 8. Database Sharding
* When a dataset becomes too massive to fit on any single machine, you use **Sharding** — splitting the data horizontally across multiple different databases based on a routing rule (e.g., by User ID).
* *Trade-offs:* This is incredibly complex. "Cross-shard queries" (e.g., counting total users across all shards) become slow and difficult. Sharding is a last resort, used only when data won't fit any other way.

## Conclusion
System design is about solving specific bottlenecks one step at a time. The true job of an engineer isn't just knowing these terms, but understanding *why* you need each piece, *when* to add it, and what *trade-offs* (costs, single points of failure, complexity) it introduces.

https://www.youtube.com/watch?v=EaXHfuHRWwg

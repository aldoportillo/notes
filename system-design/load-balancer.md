# Load Balancer

## Introduction

Modern web applications must handle large volumes of traffic. A single server has limited throughput and can fail when overloaded with client requests. 

To address this issue, **vertical scaling** (upgrading server resources like CPU and memory) may seem like an option, but it has limitations, including high cost and an upper hardware limit. 

A more scalable and cost-effective solution is **horizontal scaling**, which involves adding multiple servers to handle traffic. However, this introduces a challenge: **how does the client know which server to send requests to?**

This is where a **load balancer** comes into play.

---

## What is a Load Balancer?

A **load balancer** is a server that sits between clients and your backend servers. It evenly distributes traffic to your horizontally scaled servers, ensuring that no single server is overwhelmed while maximizing resource utilization and availability. 

In essence, a load balancer acts as a **reverse proxy**.

### Beyond HTTP

While load balancers are commonly associated with HTTP traffic between clients and servers, they can exist at various levels in a system. For example:
- **DNS Layer:** Distributing traffic using DNS Round Robin.
- **Application Layer:** Balancing requests between multiple web or application servers.
- **Database Layer:** Managing database connections to replicas or shards.

---

## DNS Round Robin Example

A simple form of load balancing happens at the **DNS layer**. For example, querying a domain with a load balancer using `dig` might return multiple IP addresses. When you `curl` any of these IPs, you'll often see the same content, demonstrating the load balancer's role in distributing traffic.

### Try It Out:
1. Run the command:
   ```bash
   dig example.com
   ```
2. Observe multiple A records (IP addresses) for the same domain.
3. `curl` each IP and note how the same webpage is returned regardless of the IP.

---

## How Load Balancers Work

### Server Registration
A load balancer needs to know the servers in its pool. Servers typically register with the load balancer during startup and deregister when they are removed. This ensures the load balancer operates with up-to-date information about available resources.

### Traffic Distribution Strategies
Load balancers use various algorithms to determine how traffic is distributed:

1. **Random**: Distributes traffic randomly.
2. **Round Robin**: Sends requests to servers in rotation.
3. **Weighted Round Robin**: Adds weights to servers, assigning more traffic to servers with higher capacity.
4. **Performance-Based**: Performs health checks on servers and routes traffic only to healthy servers.
5. **IP-Based**: Uses the client’s IP address to hash and consistently route requests to the same server. This is particularly useful for caching.
6. **Path-Based**: Routes requests to different servers based on the request path. For example, `/api` traffic could go to one server, and `/static` traffic could go to another.

### Multiple Load Balancers
A system can have multiple load balancers:
- To handle traffic at different layers (e.g., HTTP, database).
- To use different strategies for different services or endpoints.

---

## See It in Action

Here’s a practical example of using **Nginx** as a load balancer with a **weighted round robin** approach.

### Nginx Configuration
Create or edit your `nginx.conf` file:

```conf
events { }
http {
    upstream nodejs-backend {
        server localhost:3000 weight=3; # Weighted server
        server localhost:3001;         # Default weight
    }

    server {
        listen 8081;

        location / {
            proxy_set_header load-balancer-example true;
            proxy_pass http://nodejs-backend;
        }
    }
}
```

This configuration:
- Defines an upstream group `nodejs-backend` with two servers: one weighted heavier than the other.
- Routes requests from `localhost:8081` to the `nodejs-backend` servers.

### Backend Servers
Create a simple `server.js` using Express:

```javascript
const express = require('express');
const app = express();

const port = process.env.PORT || 3000;

app.listen(port, () => {
    console.log(`Server started on port ${port}`);
});

app.get('/load_balancer', (req, res) => {
    console.log(`Request handled by server on port ${port}`);
    res.send(`Server responding from port ${port}`);
});
```

Start two instances of the server on different ports:

```bash
PORT=3000 node server.js
```

```bash
PORT=3001 node server.js
```

### Test the Load Balancer
Send requests to the load balancer and observe the output:

```bash
curl localhost:8081/load_balancer
```

You should see more responses from `port 3000` than `port 3001`, illustrating the **weighted round robin** algorithm in action.

---

## Advanced Concepts

### Health Checks
Load balancers can periodically ping servers to check their health. If a server fails a health check, it is temporarily removed from the pool.

### SSL Termination
Load balancers can handle SSL encryption/decryption to offload the computational cost from backend servers.

### Sticky Sessions
For certain applications, it’s useful to ensure that subsequent requests from a client are routed to the same server. This can be achieved using **sticky sessions**.

### Auto-Scaling
Modern load balancers integrate with cloud platforms to add or remove servers dynamically based on traffic patterns.

---

## Conclusion

Load balancers are a cornerstone of scalable, resilient systems. They allow applications to handle increased traffic efficiently, improve fault tolerance, and ensure smooth operations. 

By understanding and configuring load balancers, you unlock the ability to build systems that can grow and adapt with user demand. 

Pretty cool 😎

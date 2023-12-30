# Load Balancer

## Introduction

Due to a server having a limited throughput, the more requests made from the client side can lead to a system failure.

To fix this, we can vertically scale the server by increasing the server's resources to increase throughput but there is a limit.

The most optimal fix is to horizontally scale the server. This means adding more servers to share the workload. But how does client know where to send a request.

## Definition

A load balancer is a server that sits between clients and servers to redirect the traffic evenly to our horizontally scaled system.

A load balancer is like a reverse proxy or is a reverse proxy.

Although load balancers are thought of as existing between client and server due to the intuitive nature of HTTP, load balancers can exist anywhere in a system. You can have a load balancer between your server layer and database layer, you can even have load balancing at the DNS layer (DNS Round Robbin).

### DNS Round Robbin Exercise

Referring back to our [notes on Network Protocols](./network-protocols.md), running dig on a domain with a load balancer in the DNS layer can return two different IPs. When you curl the IPs they return the same page. This is due to the load balancer. 

## How they work

First a software load balancer needs to know that it has servers. This is usually done by configuring a new server to register with the load balancer when it is added and de-registers itself once removed.

Secondly, a load balancer needs to select a server to send traffic to:

1. Randomly
2. Round Robin: Sends traffic to servers in a rotation.
3. Weighted Round Robin: Still follow the round robin rotation but add more requests to the server with more resources if the servers don't contain the same amount of resources
4. Performance: Load balancer performs health checks on servers and if one server is not handling requests well, it will not send requests to the server and vice versa.
5. IP Based: Hash client's IP address and based of the hash it gets sent to a specific server every time. Refer back to [caching](./caching.md) to see why this is useful. Caching in server 😲
6. Path Based: There are different servers dedicated to different paths. Pretty cool option when it comes to e-commerce I think.

You can have multiple load balancers at different parts of the system or even the same part of the system. Each load balancer can use a different selection strategy.

## See it in action

In nginx.conf:

```conf
events { }
http {

    upstream nodejs-backend {
        server localhost:3000 weight=3; 
        server localhost:3001; 
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

Here we implement a weighted round robin approach.

```javascript
// In server.js
const express = require('express');
const app = express();

const port = process.env.PORT || 3000;

app.listen (port, () => {
    console.log(`Server started on port ${port}`);
})

app.get('/load_balancer', (req, res) => {
    console.log(req.headers)
    res.send('Load Balancer!');
})
```

Finally, run your server with specified port 3000 and 3001.

```bash
PORT=3000 node server.js
```

```bash
PORT=3001 node server.js
```

And curl the load balancer:

```bash
curl localhost:8081/load_balancer
```

This shows us a visual representation of a weighted round robin distribution approach. Every time we curl the load balancer, we get more requests sent to port 3000 than 3001. Pretty cool 😎

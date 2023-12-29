# Proxies

A proxy is a server that sits between a client or sets of clients and another server of sets of servers.

## Kind of Proxies

1. A forward proxy (FP) acts on behalf of the client. This helps client hide the identity of the client since the source IP address sent to the server belongs to the proxy. Refer back to [Network Protocols](./network-protocols.md). Think of a VPN.

2. A reverse proxy (RP) acts on behalf of the server. Client thinks they are interacting with the server but the DNS query actually returns the IP address of the RP. Refer back to [Client Server Model](./client-server-model.md). The RP can filter out requests that the you don't want your server to encounter, take care of caching, take care of logging. A RP can also be used as a load balancer to distribute requests to multiple servers.

## Example

```javascript
// In a simple node server listening on port 3000

app.get('/example' (req, res) => {
    console.log(req.headers)
    res.send('Hello World.\n')
})
```

```conf
#In nginx.conf - A web server that can be used as a reverse proxy
events { }

http {
    upstream nodejs-backend {
        server localhost:3000
    }

    server {
        listen 8081;

        location /{
            proxy_set_header proxy-example true;
                proxy_pass http://nodejs-backend
        }
    }
}
```

Run your node server.

If you curl localhost:3000/example. You will receive 'Hello World'. This is communicating directly with the server. Look back at [Network Protocols](./network-protocols.md).

If you curl localhost:8081/example. You will get the same results as curling 3000, but through our proxy. You can verify traffic is being sent to the proxy by looking at your traffic in the node terminal. We will now have a header with 'proxy-example': 'true'.

We can now see that our RP mutated our headers. Pretty cool. 😎

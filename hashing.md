# Hashing

Hashing is an action that can be done to transform an arbitrary piece of data onto a fixed size value.

When it comes to system design that arbitrary piece of data can be:

- IP address
- Username
- HTTP Request

In this file, we will be talking about hashing for a server selection strategy. As you can recall from the [caching](./caching.md) and [load balancer](./load-balancer.md) files, this ensures that we can access our cached responses in the server.

## Simple Approach

1. Hash the IP address of clients
    - Algorithms: MD5, SHA-1, Bcrypt
2. Mod the hashes by the number of servers.
    - Assuming we have 4 servers and client's IP is hashed to 13. That client will always be redirected to server 1. 13 % 4 = 1
    - This ensures uniformity
3. Client gets redirected to same server and receives cache hits

## What if we add a server or one dies?

Considering the MOD approach. If we add a new server, modding by 5 will send our previous client's hashed IP to server 3.

This will make us miss more cache hits in our large distributed system.

### Consistent Hashing Strategy

![consistent hashing diagram](https://imgs.search.brave.com/fUrzjSOhVZ_V3ao5D4ft1MdlH39_n6yKR6xm-f5iEos/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9yZXMu/Y2xvdWRpbmFyeS5j/b20vcHJhY3RpY2Fs/ZGV2L2ltYWdlL2Zl/dGNoL3MtLTVCQk1f/N1hGLS0vY19saW1p/dCxmX2F1dG8sZmxf/cHJvZ3Jlc3NpdmUs/cV9hdXRvLHdfODAw/L2h0dHBzOi8vcmF3/LmdpdGh1YnVzZXJj/b250ZW50LmNvbS9r/YXJhbnByYXRhcHNp/bmdoL3BvcnRmb2xp/by9tYXN0ZXIvcHVi/bGljL3N0YXRpYy9j/b3Vyc2VzL3N5c3Rl/bS1kZXNpZ24vY2hh/cHRlci1JSS9jb25z/aXN0ZW50LWhhc2hp/bmcvY29uc2lzdGVu/dC1oYXNoaW5nLnBu/Zw)

As you can see above, the servers are positioned in a circle (for visual reasons), clients get redirected to the server that is closest clockwise. We can see that request 3 is sent to a healthy node 4.

If a node fails, like node 1, the request gets sent to the next server clockwise from it. Request 1 to node 2.

This minimizes the number od requests that get forwarded to different servers when new servers are added or shut down. MInimizing the number of keys that need to be remapped.

### Rendezvous Hashing Strategy

Highest random weight hashing. It hashes the username or IP address of client. For each hash it is going to calculate a score (ranking of the server/destination), this ensures each client is always sent to the same server. If that server goes down, the request gets sent to the second ranking server.

That was a lot of words so here is an example:

```javascript
//In hashing_utils.js

function hashString(string){
    let hash = 0;
    if (string.length == 0) return hash;
    for (let i = 0; i < string.length; i++) {
        let char = string.charCodeAt(i);
        hash = ((hash<<5)-hash)+char;
        hash |= 0;
    }
    return hash;
}

function computerScore(username, server) {
    const usernameHash = hashString(username);
    const serverHash = hashString(server);
    return (usernameHash * 13 + serverHash * 11) % 67;
}

module.exports = {
    hashString,
    computerScore
}

// In hashing.js

const utils = require('./hashing_utils.js');

const serverSet1 = [
    'server0',
    'server1',
    'server2',
    'server3',
    'server4',
    'server5',
]

const serverSet2 = [
    'server0',
    'server1',
    'server2',
    'server3',
    'server4'
]

const usernames = [
    'user0',
    'user1',
    'user2',
    'user3',
    'user4',
    'user5',
    'user6',
    'user7',
    'user8',
    'user9',
]

const pickServerModulo = (username, serverSet) => {
    const hash = utils.hashString(username);
    return serverSet[hash % serverSet.length];
}

const rendezvousHash = (username, serverSet) => {
    let max = 0;
    let maxServer = null;
    for (let server of serverSet) {
        let hash = utils.computerScore(username, server);
        if (hash > max) {
            max = hash;
            maxServer = server;
        }
    }
    return maxServer;
}

console.log("Modulo Hashing");

for (let username of usernames) {
    const server1 = pickServerModulo(username, serverSet1);
    const server2 = pickServerModulo(username, serverSet2);

    const serversEqual = server1 === server2;
    console.log(`${username} => ${server1} | ${server2} | ${serversEqual}`);
}

console.log("\n Rendezvous Hashing");

for (let username of usernames) {
    const server1 = rendezvousHash(username, serverSet1);
    const server2 = rendezvousHash(username, serverSet2);

    const serversEqual = server1 === server2;

    console.log(`${username} => ${server1} | ${server2} | ${serversEqual}`);
}
```

If you run the code, you will see that 8/10 usernames map to the same server when we remove a server using the rendezvous method. When using the MOD approach, 0/10 usernames map to the same server.

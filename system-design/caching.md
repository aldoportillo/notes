# Caching

Caching in algorithms is used to avoid redoing expensive computations multiple times.

In system design, caching is used to reduce the latency of a system.

This is done by storing data in a location that is different that the original location to a place in which it is easier to access the data.

At the client level: the client doesn't have to consistently get the data from the server.
At the server level: the server doesn't have to consistently get the data from the database.

## When to use

- Speed up operating that speeds up a network request by only doing it once.
- Speed up a computationally long operation.
- You have a request that is done multiple times.

## Example

```javascript
//In database.js (created to simulate retrieving data from db)

const database = {
    ['index.html']: '<html>Hello World</html>',
}

module.exports.get = (key, callback) => {
    setTimeout(() => {
        callback(database[key])
    }, 5000)
};

//In server.js

const database = require('./database');
const express = require('express');

const app = express ();
const cache = {};

app.get('/nocache/index.html', (req, res) => {
    database.get('index.html', page => {
    res.send (page);
    })
});

app.get ('/cache/index.html', (req, res) => {
    if ('index.html' in cache) {
        res.send(cache['index.html']);
        return;
    }

    database.get('index.html', page => {
        cache['index.html'] = page;
        res.send(page);
    })
}):

app.listen (3000, function () {
    console. log ('Listening on port 3000!');
});
```

Here we are creating a 'database' that always takes 5 seconds to return the data. We then define two routes /nocache and /cache. If we spin up our node server and visit /nocache every time we visit that page. It will take 5 seconds to load. If we then visit /cache, the first time it will take 5 seconds to load but after that, it will load instantly since the value of the database call is cached.

## Types of Caches

What if the data gets updated by introducing Post/Patch/Delete? We now have two sources of truth: the cache and the database.

1. Write Through Cache
    A caching system that will overwrite whatever is in the cache and updating the database when the data gets updated.

2. Write Back Cache
    A caching system that will overwrite the cache when the data gets update but not the database. Later on the cache will update the database asynchronously: during set intervals or during cache eviction. Think about what happens to memory when a server shuts down. How can we make sure we don't lose our cache?

## Stale Caches

Assuming we have a write back cache. If a user comments on a post, all the other servers's caches will be stale except for the server the user commented on. In order to fix this we can use REDIS to store a single source of truth and all servers point to that single cache. Think of other places this could be useful or not necessary.

## Eviction Policies

1. LRU - Least Recently User
2. LFU - Least Frequently Used
3. FIFO - First in First Out
4. LILO - Last in Last Out

The decision of which eviction policy to use depends on the use case.

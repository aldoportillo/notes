# Key Value Store

A common non relational database. The best way to think of this is like a hash.

These are flexible and simple. They also offer low latency and increased throughput due to everything being stored in a hash.

These are great for:

- caching
- configuration

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
const redis = require('redis').createClient();

const app = express ();

app.get('/nocache/index.html', (req, res) => {
    database.get('index.html', page => {
    res.send (page);
    })
});

app.get ('/cache/index.html', (req, res) => {
    redis.get('index.html', (err, redisRes) => {
        if(redisRes) {
            res.send(redisRes)
            return;
        }
    })

    database.get('index.html', page => {
        redis.set('index.html', page, 'EX', 10)
        res.send(page);
    })
}):

app.listen (3000, function () {
    console. log ('Listening on port 3000!');
});
```

Here we are using redis for caching in the example created in [caching](./caching.md)
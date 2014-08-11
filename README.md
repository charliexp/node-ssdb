node-ssdb
=========

[ssdb](https://github.com/ideawu/ssdb) nodejs client library,
ssdb is a fast nosql database, an alternative to redis.

Latest version: v0.0.3 (Currently untested).

Requirements
-------------

- ssdb 1.6.8.8+

Installation
-------------

```bash
$ npm install ssdb
```

Example
--------

The traditional Node.js way:

```js
var ssdb = require('ssdb');
var client = ssdb.createClient();

client.set('key', 'val', function(err, data){
  if (!err) {
    console.log(data);
  } else throw err;
});
```

Work with TJ's [co](https://github.com/visionmedia/co):

```js
var co = require('co');
client.thunkify();

co(function *(){
  try{
    var key = 'key';
    var a = yield client.set(key, 'val');
    var b = yield client.get(key);
    console.log(a, b);  // 1 'val'
  } catch(e){
    throw e;
  }
})();
```

Work with promises:

```js
client.promisify()

client.set('key', 'val')
.then(function(){
  return client.get('key')
}).then(function(d){
  console.log(d);  // 'val'
});
```

API References
--------------

### createClient(options)

To make a ssdb client:

```js
var ssdb = require('ssdb');
var client = ssdb.createClient();
```

options (with default values):

```js
{
  host: '0.0.0.0',
  port: 8888,
  timeout: 0
}
```

### client.quit()

Quit from ssdb server.

### client.unref()

Equivalent to `client.conn.sock.unref()`, see http://nodejs.org/api/net.html#net_server_unref.

### command names

```js
ssdb.commands   // js object keys
```

### Client events

#### 'status_ok'

- parameters: `cmd`, `data`

Listener example:
```js
var util = require('util');

client.on('status_ok', function(cmd, data){
  console.log(util.format('%s replies ok, data: %s', cmd, data));
});
```

#### 'status_not_found'

- parameters: `cmd`

#### 'status_client_error'

- parameters: `cmd`

#### 'status_not_ok'

- parameters: `status`, `cmd`

### Connection Events Handling

The node connection object is `client.conn.sock`, to listen connection error as an example:

```js
client.conn.sock.on('error', function(err){
  throw err;
});
```

Connection events reference: http://nodejs.org/api/net.html

How to run tests
----------------

requires node 11.x+:

```bash
$ npm install co
$ npm install co-sleep
$ npm install mocha
mocha --harmony test.js
```

SSDB API Documentation
----------------------

Detail docs for ssdb interfaces can be found at: https://github.com/hit9/ssdb.api.docs

License
-------

[LICENSE-MIT](./LICENSE-MIT)

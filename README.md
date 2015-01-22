puffer
======

[![Build Status](https://travis-ci.org/tectual/puffer.svg)](https://travis-ci.org/tectual/puffer)

[Puffer](https://www.npmjs.com/package/puffer) is an extendable Couchbase ODM for Hapi js. It uses [Boom](https://www.npmjs.com/package/boom) to return nicer DB errors and use [Q](https://www.npmjs.com/package/q) to provide promises. 

Full documation can be found [here](http://tectual.github.io/puffer/).

## How to use

You can initialize it in your server and use it in your other files or even other plugins.

### Single module with many files

In your server.coffee have something like this:

```
Hapi = require 'hapi'
new require('puffer')( { host: '127.0.0.1', name: 'default' } )
```

Above line will create an instance of couchbase which you can use in your other files as an example in your userModel.coffee you can use this:

```
bucket = require('puffer').instance
bucket.create( 'doc1', { color: 'red' } )
      .then( (d) -> console.log d )
```

### Multiple Hapi Plugins

In your server.coffee have something like this:

```
Hapi = require 'hapi'
db = new require('puffer')( { host: '127.0.0.1', name: 'default' } )

server.register { 
  register: require('myPlugin'), 
  options: { bucket: db } 
}, (err) ->
  throw err if err
  server.start()
```
Above line will create an instance of couchbase which you should pass to plugins. In your myPlugin module you can do this:
```
exports.register = (plugin, options, next) ->
  bucket = options.bucket
  bucket.create( 'doc1', { color: 'red' } )
        .then( (d) -> console.log d )
```

Find the full list of available methods [here](main.html).

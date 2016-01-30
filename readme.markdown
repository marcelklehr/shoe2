# shoe2

binary-safe streaming sockjs for node and the browser

This package is like [shoe](https://npmjs.org/package/shoe), but it encodes and
decodes data as base64 to be binary safe (like[shoe-bin](https://npmjs.org/package/shoe-bin), but it has all the necessary events).

Since shoe-bin is no longer maintained, this package was born.

# example

The API is exactly the same as [shoe](https://npmjs.org/package/shoe).

browser code:

``` js
var shoe = require('shoe2');
var through = require('through2');

var stream = shoe('/sock');
stream.pipe(through(function (buf, enc, next) {
    console.log(buf.length + '\n');
    next();
})).pipe(stream);
```

server code:

``` js
var shoe = require('shoe2');
var http = require('http');

var ecstatic = require('ecstatic')(__dirname + '/static');
var server = http.createServer(ecstatic);
server.listen(5000);

var sock = shoe(function (stream) {
    stream.write(new Buffer(5000));
});
sock.install(server, '/sock');
```

# browser methods

``` js
var shoe = require('shoe2')
```

## var stream = shoe(uri, cb)

Return a binary-safe readable/writable stream from the sockjs path `uri`.

`uri` may be a full uri or just a path.

`shoe` will emit a `'connect'` event when the connection is open, but writes
before the connection is open will be buffered just like `net.connect()`.

# server methods

``` js
var shoe = require('shoe2')
```

## var sock = shoe(opts={}, cb)

Create a sockjs server, passing through the options from `ops`.

If `cb` is specified, a listener for the `'connection'` event is set up for
`cb(stream)`.

## sock.install(server, opts)

Install the socket on `server` at `opts.prefix`. If `opts` is a string, it will
be used as the `opts.prefix`.

# install

With [npm](https://npmjs.org) do:

```
npm install shoe2
```

# license

MIT

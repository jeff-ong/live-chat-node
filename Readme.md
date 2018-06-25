
# Live Chat Node Application
This simple application let you create disposable live chat sessions. It is developed with Node, Express, and Socket.io.

[Live demo](https://shielded-spire-23012.herokuapp.com/)

## Features

Socket.IO enables real-time bidirectional event-based communication. It consists in:

- a Node.js server
- a [Javascript client library](https://github.com/socketio/socket.io-client) for the browser (or a Node.js client)

Some implementations in other languages are also available:

- [Java](https://github.com/socketio/socket.io-client-java)
- [C++](https://github.com/socketio/socket.io-client-cpp)
- [Swift](https://github.com/socketio/socket.io-client-swift)
- [Dart](https://github.com/rikulo/socket.io-client-dart)

Its main features are:

#### Reliability

Connections are established even in the presence of:
  - proxies and load balancers.
  - personal firewall and antivirus software.

For this purpose, it relies on [Engine.IO](https://github.com/socketio/engine.io), which first establishes a long-polling connection, then tries to upgrade to better transports that are "tested" on the side, like WebSocket. Please see the [Goals](https://github.com/socketio/engine.io#goals) section for more information.

#### Auto-reconnection support

Unless instructed otherwise a disconnected client will try to reconnect forever, until the server is available again. Please see the available reconnection options [here](https://github.com/socketio/socket.io-client/blob/master/docs/API.md#new-managerurl-options).

#### Disconnection detection

A heartbeat mechanism is implemented at the Engine.IO level, allowing both the server and the client to know when the other one is not responding anymore.

That functionality is achieved with timers set on both the server and the client, with timeout values (the `pingInterval` and `pingTimeout` parameters) shared during the connection handshake. Those timers require any subsequent client calls to be directed to the same server, hence the `sticky-session` requirement when using multiples nodes.

#### Binary support

Any serializable data structures can be emitted, including:

- [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) and [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob) in the browser
- [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) and [Buffer](https://nodejs.org/api/buffer.html) in Node.js

#### Cross-browser

Browser support is tested in Saucelabs:

[![Sauce Test Status](https://saucelabs.com/browser-matrix/socket.svg)](https://saucelabs.com/u/socket)

#### Multiplexing support

In order to create separation of concerns within your application (for example per module, or based on permissions), Socket.IO allows you to create several `Namespaces`, which will act as separate communication channels but will share the same underlying connection.

#### Room support

Within each `Namespace`, you can define arbitrary channels, called `Rooms`, that sockets can join and leave. You can then broadcast to any given room, reaching every socket that has joined it.

This is a useful feature to send notifications to a group of users, or to a given user connected on several devices for example.


**Note:** Socket.IO is not a WebSocket implementation. Although Socket.IO indeed uses WebSocket as a transport when possible, it adds some metadata to each packet: the packet type, the namespace and the ack id when a message acknowledgement is needed. That is why a WebSocket client will not be able to successfully connect to a Socket.IO server, and a Socket.IO client will not be able to connect to a WebSocket server (like `ws://echo.websocket.org`) either. Please see the protocol specification [here](https://github.com/socketio/socket.io-protocol).

### In conjunction with Express

Starting with **3.0**, express applications have become request handler
functions that you pass to `http` or `http` `Server` instances. You need
to pass the `Server` to `socket.io`, and not the express application
function. Also make sure to call `.listen` on the `server`, not the `app`.

```js
var app = require('express')();
var server = require('http').createServer(app);
var io = require('socket.io')(server);
io.on('connection', function(){ /* … */ });
server.listen(3000);
```

## Documentation

Please see the documentation [here](/docs/README.md). Contributions are welcome!

## Debug / logging

Socket.IO is powered by [debug](https://github.com/visionmedia/debug).
In order to see all the debug output, run your app with the environment variable
`DEBUG` including the desired scope.

To see the output from all of Socket.IO's debugging scopes you can use:

```
DEBUG=socket.io* node myapp
```

## License

[MIT](LICENSE)

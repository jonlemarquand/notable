---
title: 'NEM Stack 4 - How Node.Js Works: A Look Behind the Scenes'
created: '2020-04-02T06:52:35.152Z'
modified: '2020-04-04T15:47:28.887Z'
---

# NEM Stack 4 - How Node.Js Works: A Look Behind the Scenes

## Node, V8, Libuv and C++

* Node: A Javascript runtime based on Google's V8 engine.
  * What converts javascript code into machine code.
* Libuv is a strong library that is focused on async i/o.
  * Responsible for the event loop and the thread pool.
  * Libuv is written in C++ (as is V8)
* Also relies on http-parser, c-ares, OpenSSL and zlib

## Processes, Threads and the Thread Pool

* Node.js process (Instance of a program in execution on a computer)
  * In that process, node.js runs a single thread (A sequence of instructions)
    * It makes it easy to block node applications.
    * No matter if you have 10 users or 10 million users accessing the site.
* In the single thread:
  * Initialise program -> Execute "top-level" code -> Require modules -> Register event callbacks -> Start event loop
  * Most of the work is done in the event loop.
  * Some tasks are too heavy to be in the event loop, so they are done in the thread pool.
    * 4 - 128 additional threads which offload heavy tasks (behind the scenes).
    * File system APIs, cryptography, compression, DNS lookups
    * To change the size of the threadpool: `process.env.UV_THREADPOOL_SIZE = 2;`
  
### The Node.js Event Loop

* All the application code that is inside callback functions (non-top-level code)
* Node.js is built around callback functions.
* Event-driven architecture:
  * Events are emitted
  * Event loops pick them up
  * Callbacks are called.
* Event loop does orchestration.

#### The Event Loop in Detail

* Start
* Callback Queues - Many phases, each with their own callback queue.
* 1 - Expired timer callbacks
  * If a timer has expired, the callback is added at this point.
* 2 - I/O polling and callbacks
  * Looking for new I/O events that are ready to be processed and putting them in the callback queue.
  * Where most of our code gets executed.
* 3 - setimmediate callbacks
  * Callbacks that are called if we want something done immediatley after the i/o callback
* 4 - Close callbacks
  * For when a webserver or websocket shuts down.
* Also the process.nexttick() queue and the other microtasks queue.
  * These are always executed after whatever the current phase is.
* A tick is basically one cycle in the event loop.
* The difference with PHP (for example), is that PHP starts a new thread for every user.
* Don't block:
  * Don't use sync versions of functions in fs, crypto and zlib modules in your callback functions.
    * Can use sync versions in top level (which runs before the event loop starts)
  * Don't perform complex calculations (e.g. loops inside loops)
    * Millions of numbers in loops inside of loops
  * Be careful with JSON in large objects
    * JSON can take a long time to parse or stringify.
  * Don't use too complex regular expressions (e.g. nested quantifiers)

## Events and Event-Driven Architecture

* In Node their are certain objects that emit events when something happens in the app.
  * These events can then be picked up by event listeners which will react by calling callback functions.
  * This is known as tthe observer pattern in javascript.
* It makes it decoupled, events are separated from the event listeners.

```javascript

const EventEmitter = require("events");

class Sales extends EventEmitter{
  constructor() {
    super();
  }
}

const myEmitter = new Sales();

myEmitter.on('newSale', () => {
  console.log('There was a new sale!')
})

myEmitter.on('newSale', () => {
  console.log('Customer name: Jonas')
})

myEmitter.emit("newSale");

```
* In this example, the 2 `myEmitter.on` are listening for the event, which is triggered with `myEmitter.emit`.
* In Node the best practice is to use ES6 classes to extend.

## Streams

* Streams are used to process (read and write) data piece by piece (chunks), without completing the whole read or write operation, and therefore without keepign all the data in memory.
  * We read part of the data, do something with it and then load more of it.
  * Think of how a youtube video is loaded.
* Perfect for handling large volumes of data, for example videos.
* More efficient data processing in terms of memory and time
  * No need to keep all the data in memory
  * We don't have to wait until all the data is available.

### Node.js Streams Fundamentals

* Streams are instances of the EventEmitter class.
* Four kinds of streams: Readable, Writable, Duplex and Transform
  * The first three are consuming streams
* Readable Streams: Streams from which we can read (consume) data
  * Http requests
  * Fs read streams
  * Important events; data, end
    * Data is when there is a new piece of data to consume
    * End when there is no more data to consume.
  * Functions:
    * pipe() - Allows us to plug streams together
    * read()
* Writable Streams: Streams to which we can write data
  * Http responses / Fs write streams
  * Important events: drain/finish
  * Functions:
    * write()
    * end()
* Duplex Streams: Both readable and writable at the same time.
  * A web socket from the net module (A communication channel between client/server)
* Transform Streams: Duplex streams that transform data as it is written or read
  * zlib Gzip creation

### Streams in Practice

```node.js

const fs = require('fs');
const server = require('http').createServer();

server.on('request', (req, res) => {
  /*Solution 1: Whole File Sending
  fs.readFile('test-file.txt', (err, data) => {
    if (err) console.log(err);
    res.end(data);
  });
*/

  //Solution 2: Streams

  // With this solution, we stream the file chunk by chunk. Then listen for the 'end' event on the readable stream to send the writable stream.
  const readable = fs.createReadStream('test-file.txt');
  readable.on('data', chunk => {
    res.write(chunk);
  })
  readable.on('end', () => {
    res.end();
  })
  readable.on('error', err => {
    console.log(err);
    res.statusCode = 500;
    res.end("File not found!");
  })
});


server.listen(8000, '127.0.0.1', () => {
  console.log('Listening...');
});
```
* Backpressure: When the response can't send the data as fast as it is receiving it.
  * How to solve - The Pipe operator:

```node.js

  const readable = fs.createReadStream('test-file.txt');
  readable.pipe(res);
  // readableSource.pipe(writableDestination)

```

## How Requiring Modules Really Works

* Each javascript file is treated as a separate module.
* Node.js uses the CommonJS module system: require(), exports, or module.exports;
* ES module system is used in browsers: import/export
* Path Resolving: How Node Decides which Modules to Load
  * Start with core modules;
  * If begins with './' or '../' -> Try to load developer module
  * If no file found -> Try to find folder with index.js in it.
  * Else  -> Go to node_modules/ (npm) and try to find there.
